---
layout: post
title: 如何获取比 dism.log 更详细的日志
categories: blog
tags: [职场]
description: 如何获取比 dism.log 更详细的日志
---

## 正文 ##

　　在工作中，曾经遇到过一个问题。<br/>
<br/>
　　有一个 component，名字叫做 Oxford Adaptive Learning Dictionary，是一款牛津词典的应用。这个 component，需要安装在 Windows 10 的系统上。这个应用属于 UWP 应用，用行话来讲，叫做 Modern App。Modern App 是区别于 Win32 App 的一种应用（按照我的理解，在 Win8 和 Win8.1 之后，才有了 **“应用”** 这一说法，在这之前，我们都把他们叫做 **“软件”**），它采用的是 **沙盒机制**，其程序文件是无法访问的（使用管理员账户也无法访问）。要打开这一应用，只有通过开始菜单的 All App 中的图标；要卸载这一应用，可以通过在 All App 中邮件选择卸载，或者在 Apps & Features 中，找到这一应用以后选择卸载。（Win32 程序的卸载方式可以通过 Program and Features，Modern App 不会在其中出现）<br/>
<br/>
　　背景介绍完了以后，我来描述一下所遇到的问题。Oxford Dictionary 的安装文件的后缀名是 appxbundle，通过 `dism /online /Add-ProvisionedAppxPackage` 命令来安装。在 dism 的命令显示 “操作成功完成” 之后，发现在 All Apps 里面有了 Oxford Dictionary 的图标，但是在 Apps & Features 里面却没有发现 Oxford 这一项。这点十分蹊跷，看起来像是 Oxford 没有成功安装。<br/>
　　在 All Apps 里面点击 Oxford，发现应用的图标下面出现了类似进度条的物件，Oxford 词典无论如何都无法启动。问题出现了：Oxford Dictionary 无法安装！<br/>
　　但是，`dism /online /Add-ProvisionedAppxPackage` 不是显示 “操作成功完成” 了吗？于是，试图从 dism 的 log 中寻找原因。<br/>
　　dism.log 的路径是 `C:\Windows\Logs\DISM\dism.log`。打开这个日志后，发现所有的命令的执行结果都是 `Info` 或者 `Warning` 类型，没有 `Error`；而且 dism.log 也显示 `Add-ProvisionedAppxPackage` 命令成功完成。这条路被堵死了。<br/>
<br/>
　　休息一下以后，回来再次思考这个问题。可以确定的是，Oxford Dictionary 一定没有安装成功，因为如果安装成功，那么一定可以启动这个应用。但是 dism.log 显示命令的执行是成功的。于是 debug 的思路是：**能否找到比 dism.log 更详细的安装日志？**<br/>
　　于是我请教了某一位同事，在他的帮助下，成功解决了这一问题！以下是解决步骤。<br/>
<br/>
　　第一步，找到 “事件查看器” **Event Viewer**，保存 “Windows 日志” 下面的条目，例如：应用程序、系统等等。将这些日志保存下来，是为了做比较。所以，在安装 Oxford 之前，保存一次；在安装 Oxford 之后，再保存一次。这样就可以比较前后两次日志的不同之处了。<br/>
　　后来知道，其实，像 Oxford 之类的应用的安装日志在 **系统** 日志中。
<center>
  <p><img src="/images/event-viewer/01_log_dump.png" align="center"></p>
</center>
<br/>
　　第二步，使用 Beyond Compare，比较前后日志的不同。可以查找关键字如 “Oxford” 或者其他相关的关键字。<br/>
　　如下图所示，发现了不同之处。在 `C:\Windows\Temp\AppXDeploymentServer_E33F44D0-B47D-0001-276B-3FE37DB4D101.evtx` 中记录了与 Oxford 安装相关的日志。由此顺藤摸瓜，继续寻找原因。
<center>
  <p><img src="/images/event-viewer/02_difference_found.png" align="center"></p>
</center>
<br/>
　　第三步，根据进一步的分析，事件查看器中类似 Oxford 的安装日志的位置是 `C:\Windows\System32\winevt\Logs\Microsoft-Windows-AppXDeploymentServer%4Operational.evtx`，当然也可以直接在事件查看器的 Windows 日志下点击 **系统** 来查看。如下图所示是直接打开日志文件并且搜索 Oxford 的结果，这就是 **比 dism.log 更详细的安装日志**！
<center>
  <p><img src="/images/event-viewer/03_Oxford_error.png" align="center"></p>
</center>
<br/>
　　**出错原因分析**：其实出错原因很简单，在安装 Oxford Dictionary 时，需要指定 `DependencyPackagePath`。如果没有指定，而且目标机器上没有依赖的库文件的话，安装就会失败。这也解释了为什么在某些 Win10 机器上可以安装，而某些机器不可以。<br/>
　　完整的 Oxford Dictionary 安装命令，如下所示：

	dism.exe /online /Add-ProvisionedAppxPackage /PackagePath:"c:\OxfordUniversityPressh.4488779F26797_2016.219.1640.5897_neutral_~_nttgmz6jfxeb6.appxbundle" /DependencyPackagePath:"c:\Microsoft.VCLibs.120.00_12.0.21005.1_x86__8wekyb3d8bbwe.appx" /CustomDataPath:"c:\OUP.xml" /LicensePath:"c:\OxfordUniversityPressh.4488779F26797_nttgmz6jfxeb6_License1.xml

## 小结 ##

　　通过这次问题解决的过程，知道 Windows 10 或者 Windows 8.1 的系统，使用 `dism /online /Add-ProvisionedAppxPackage` 命令会在 `C:\Windows\Logs\DISM\dism.log` 中生成日志。并且，更详细的日志，可以通过 事件管理器->Windows 日志->系统 来查看，或者打开 `C:\Windows\System32\winevt\Logs\Microsoft-Windows-AppXDeploymentServer%4Operational.evtx` 来查看。

## 后记 ##

　　当时我去请教的同事，我们可以叫他 Robell。他对 dism，PowerShell，DOS Script，VB Script 等有比较深的造诣。在这之后不久，他离开了 HP。写这篇文章，也是为了顺便纪念他。^^

<br/>

<div align="right">7/29/2016 11:18:38 AM </div>