---
layout: post
title: ProvisionedAppxPackage VS AppxPackage
categories: blog
tags: [职场]
description: ProvisionedAppxPackage 和 AppxPackage 的比较
---

## 正文 ##

　　先来说说问题的由来。<br/>

　　在 Preinstall 的 component 中，有一支 component 叫做 MS_StartApp，这个 component 的行为是在预安装时为目标机器装入一些 Modern APP。所遇到的问题是，旧版的 MS_StartApp 在安装 Modern App 时，有部分 App 没有安装成功，新版的 MS_StartApp 没有这个问题。但是另外一个部门的同事，手里的机器是使用旧版本的 MS_StartApp 安装出来了，就会导致部分 App 没有安装，直观的反映就是在 Start Menu 中有一些 Tile 丢失了。<br/>
　　一个比较好的解决方案是，由我来开发一个单独运行的 component，来帮助安装上丢失的 App。<br/>

　　于是我做了一些前期调研。首先以 MS_StartApp 作为 Sample Script 来学习，发现 Modern App 的源文件是.appx 或者 .appxbundle 文件，通过 DISM 的如下命令来安装：

	DISM Command-Line Script:
	DISM /Online /Add-ProvisionedAppxPackage

　　安装完成之后，发现其实这个 App 可以是一个 ProvisionedAppxPackage，也可以是一个 AppxPackage。那么就会产生一个问题，如果我要卸载这个应用，那么我应该卸载 ProvisionedAppxPackage 还是 AppxPackage？<br/>
　　有关 AppxPackage，已有的知识告诉我可以通过 PowerShell 的方式安装：

	PowerShell Appx Module Cmdlets Script:
	PS > Add-AppxPackage

　　那么，这两种安装方式有什么不同呢？<br/>
　　总结以上两个问题，其核心问题是：**ProvisionedAppxPackage 和 AppxPackage 的区别是什么？**<br/>
　　为了回答这个核心问题，做了如下的研究，最终通过实践，初步理解了 ProvisionedAppxPackage 和 AppxPackage 的区别！<br/>
<br/>
　　在 PowerShell 的 DISM Cmdlets 中，有关 `Add-AppxProvisionedPackage` 命令，有如下重要的说明：

> The Add-AppxProvisionedPackage cmdlet adds an app package (.appx) that will install **for each new user** to a Windows image.
> 
> Use the **Online** parameter to specify the **running operating system** on your local computer, or use the Path parameter to specify the location of a mounted Windows image.
> 
> To add an app package (.appx) for **a particular user**, or to test a package while developing your app, use the **Add-AppxPackage** cmdlet instead.

　　在 PowerShell 的 Appx Module Cmdlets 中，有关 `Add-AppxPackage` 命令，有重要的说明如下：

> Adds a signed app package to **a user account**.

　　所以，猜测 ProvisionedAppxPackage 和 AppxPackage 的区别应该和**账户**有关。<br/>

　　首先，通过如下 DISM 命令，取出一份 ProvisionedAppxPackage 列表：

	DISM Command-Line Script:
	DISM /Online /Get-ProvisionedAppxPackages

　　然后，通过如下 PowerShell 命令，取出一份 AppxPackage 列表：

	PowerShell Appx Module Cmdlets Script:
	PS > Get-AppxPackage

　　Get-AppxPackage 有一个参数是 `-AllUsers`，可以取出所有账户下的 AppxPackage 列表。把这个 List 也取出来，用于对比：

	PowerShell Appx Module Cmdlets Script:
	PS > Get-AppxPackage -AllUsers

　　下图是 Julius 账户下，所取出的 ProvisionedAppxPackage 和 AppxPackage 的对比图：
<center>
  <p><img src="/images/event-viewer/03_Oxford_error.png" align="center"></p>
</center>
<br/>
　　观察以上对比结果，可以发现。。。

## 小结 ##

　　有些 AppxPackage，可以把它的安装包（.appx or .appxbundle）以 ProvisionedAppxPackage 的方式安装（例如 Windows.Photos 等。我估计可能所有的 .appx or .appxbundle 都可以用这种方式安装，因为这么做的好处是，所有的新增用户都可以在 OOBE 的时候安装这个 APP）<br/>
<br/>
　　安装完之后，发现通过 MS_StartApp 这个 component 安装的 APP，都保留了一份 ProvisionedAppxPackage，这样，在 新增用户 之后，新用户 会从 ProvisionedAppxPackage 里面，安装一个 APP 出来（已验证）<br/>
<br/>
　　有些 APP 不会保留一份 ProvisionedAppxPackage，例如 MicrosoftEdge 浏览器（这个是 Win10 OS 自带的） 就没有保留一份 ProvisionedAppxPackage，他们只是以 AppxPackage 的方式呈现给每一个用户。当 新用户 创建以后，还是会有这个 APP，这肯定是由另外一种机制控制的，可能是 SystemApps 机制。

## 总结 ##

　　关于 AppxPackage 和 ProvisionedAppxPackage 的区别，总结如下：<br/>
　　如果你开发了一款 Modern APP，想安装在用户的机器上，如果你是以 ProvisionedAppxPackage 的方式安装，那么任何新增的用户，都可以在 OOBE 时安装这款 APP，如果你用 AppxPackage 的方式安装，那么只是为当前用户安装，新增用户无法在 OOBE 时安装这款 APP。

<br/>

<div align="right">8/6/2016 1:23:56 PM </div>