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

　　研究发现，通过 PowerShell 的 DISM Cmdlets 也可以达到 DISM 安装相同的效果，DISM 安装所具有的参数，DISM Cmdlets 同样有。PowerShell 的 DISM Cmdlets 安装命令示例如下：

	PowerShell DISM Cmdlets Script:
	PS > Add-AppxProvisionedPackage -Online

　　以下 ProvisionedAppxPackage 的安装方法，都会采用 PowerShell DISM Cmdlets Script 的方式给出。

　　安装完成之后，发现其实这个 App 可以是一个 ProvisionedAppxPackage，也可以是一个 AppxPackage。那么就会产生一个问题，如果我要卸载这个应用，那么我应该卸载 ProvisionedAppxPackage 还是 AppxPackage？<br/>
　　有关 AppxPackage，已有的知识告诉我可以通过 PowerShell 的方式安装：

	PowerShell Appx Module Cmdlets Script:
	PS > Add-AppxPackage

　　那么，这两种安装方式有什么不同呢？<br/>
　　总结以上两个问题，其核心问题是：**ProvisionedAppxPackage 和 AppxPackage 的区别是什么？**<br/>
　　为了回答这个核心问题，做了如下的研究，最终通过实践，初步理解了 ProvisionedAppxPackage 和 AppxPackage 的区别！

　　在 PowerShell 的 DISM Cmdlets 中，有关 `Add-AppxProvisionedPackage` 命令，有如下重要的说明：

> The Add-AppxProvisionedPackage cmdlet adds an app package (.appx) that will install **for each new user** to a Windows image.
> 
> Use the **Online** parameter to specify the **running operating system** on your local computer, or use the Path parameter to specify the location of a mounted Windows image.
> 
> To add an app package (.appx) for **a particular user**, or to test a package while developing your app, use the **Add-AppxPackage** cmdlet instead.

　　在 PowerShell 的 Appx Module Cmdlets 中，有关 `Add-AppxPackage` 命令，有重要的说明如下：

> Adds a signed app package to **a user account**.

　　所以，猜测 ProvisionedAppxPackage 和 AppxPackage 的区别应该和**账户**有关。

　　首先，在 CDC 账户下，通过如下 PowerShell DISM Cmdlets 命令，取出一份 ProvisionedAppxPackage 列表：

	PowerShell DISM Cmdlets Script:
	PS > Get-AppxProvisionedPackage -Online

　　实践发现，无论在任何一个账户中取出 ProvisionedAppxPackage List，其内容都一样，这就验证了 ProvisionedAppxPackage 是 **for each new user** 的。<br/>
　　所以，下面的研究中就不会在其他账户中取出重复的 ProvisionedAppxPackage List。

　　然后，在 CDC 账户下，通过如下 PowerShell 命令，取出一份 AppxPackage 列表：

	PowerShell Appx Module Cmdlets Script:
	PS > Get-AppxPackage

　　筛选出 PackageFullName 和 PackageName，对比结果如下：
<center>
  <p><img src="/images/provisioned-appx-package/01_cdc_appx_compare.JPG" align="center"></p>
</center>
　　可以看到 ProvisionedAppxPackage 是 AppxPackage 的子集。在 AppxPackage 中列出的额外的项目，属于 Base OS 自带的 App。

　　新建账户 CDC_Temp,发现在进入 CDC_Temp 账户时会有 OOBE。OOBE 过后，取出一份 AppxPackage 列表，然后对比 CDC 和 CDC_Temp 的 AppxPackage 列表，筛选出 PackageFullName，对比结果如下：
<center>
  <p><img src="/images/provisioned-appx-package/02_cdc_temp_appx_compare.JPG" align="center"></p>
</center>
　　可以看到，两者的 AppxPackage 是完全相同的。

　　因为观察到进入 新账户 时有 OOBE 的过程，因此猜测，新账户 OOBE 时会把系统中的 ProvisionedAppxPackage 安装成为 AppxPackage。<br/>
　　为了验证这一猜测，使用如下命令移除 `BingFinance` 这个  ProvisionedAppxPackage，然后新建账户 CDC_Julius，静待 OOBE。<br/>
　　筛选出 PackageFullName 后，CDC 和 CDC_Julius 的 AppxPackage 对比结果如下：
<center>
  <p><img src="/images/provisioned-appx-package/03_cdc_julius_appx_compare.JPG" align="center"></p>
</center>
　　可以看到，CDC 有 BingFinance，而 CDC_Julius 没有 BingFinance。<br/>
　　由此证明，新账户 OOBE 时会把系统中的 ProvisionedAppxPackage 安装成为 AppxPackage。

## 番外 ##

### 有关 Get-AppxPackage -AllUsers ###

　　PowerShell 的 Appx Cmdlets 中的 Get-AppxPackage 命令，有一个参数是 `-AllUsers`，可以取出所有账户下的 AppxPackage 列表。

	PowerShell Appx Module Cmdlets Script:
	PS > Get-AppxPackage -AllUsers

　　在系统中只有 CDC 和 CDC_Temp 两个账户时，摘取 BingFinance 相关信息，如下所示：

	Name                   : Microsoft.BingFinance
	Publisher              : CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US
	Architecture           : X86
	ResourceId             : 
	Version                : 4.8.239.0
	PackageFullName        : Microsoft.BingFinance_4.8.239.0_x86__8wekyb3d8bbwe
	InstallLocation        : C:\Program Files\WindowsApps\Microsoft.BingFinance_4.8.239.0_x86__8wekyb3d8bbwe
	IsFramework            : False
	PackageFamilyName      : Microsoft.BingFinance_8wekyb3d8bbwe
	PublisherId            : 8wekyb3d8bbwe
	PackageUserInformation : {S-1-5-21-3858193888-3433337990-3667270340-1001 [CDC]: Installed, 
	                         S-1-5-21-3858193888-3433337990-3667270340-1002 [CDC_Temp]: Installed}
	IsResourcePackage      : False
	IsBundle               : False
	IsDevelopmentMode      : False

　　注意，`PackageUserInformation` 显示了该 AppxPackage 为哪些用户安装了。

### 有关 Remove-AppxPackage ###

　　PowerShell 的 Appx Cmdlets 中的 Remove-AppxPackage 命令，只可以为当前用户移除 AppxPackage。

	PowerShell Appx Module Cmdlets Script:
	PS > Remove-AppxPackage <string>

## 小结 ##

　　有些 AppxPackage，可以把它的安装包（.appx or .appxbundle）以 ProvisionedAppxPackage 的方式安装（例如 Windows.Photos 等。我估计可能所有的 .appx or .appxbundle 都可以用这种方式安装，因为这么做的好处是，所有的新增用户都可以在 OOBE 的时候安装这个 APP）

　　安装完之后，发现通过 MS_StartApp 这个 component 安装的 APP，都保留了一份 ProvisionedAppxPackage，这样，在 新增用户 之后，新用户 会从 ProvisionedAppxPackage 里面，安装一个 APP 出来（已验证）

　　有些 APP 不会保留一份 ProvisionedAppxPackage，例如 MicrosoftEdge 浏览器（这个是 Win10 OS 自带的） 就没有保留一份 ProvisionedAppxPackage，他们只是以 AppxPackage 的方式呈现给每一个用户。当 新用户 创建以后，还是会有这个 APP，这肯定是由另外一种机制控制的，可能是 SystemApps 机制。

## 总结 ##

　　关于 AppxPackage 和 ProvisionedAppxPackage 的区别，总结如下：<br/>
　　如果你开发了一款 Modern APP，想安装在用户的机器上，如果你是以 ProvisionedAppxPackage 的方式安装，那么任何新增的用户，都可以在 OOBE 时安装这款 APP，如果你用 AppxPackage 的方式安装，那么只是为当前用户安装，新增用户无法在 OOBE 时安装这款 APP。

　　查到了另一份资料（见参考资料第4条），是 Provisioned Apps 的官方描述：<br/>
　　Apps that are installed in the Windows image are called **provisioned apps**. Provisioned apps are staged in the image and are **scheduled to be installed for every user** of the Windows image at first logon or at the next logon, if the user account is already created.<br/>
　　和我本人的总结相类似，说明我的研究和总结是正确的！

## 参考资料 ##

1. [DISM App Package (.appx or .appxbundle) Servicing Command-Line Options](https://technet.microsoft.com/en-us/library/hh824882.aspx) （通过 DISM 安装 ProvisionedAppxPackage，这是知识点的引入）
1. [DISM Cmdlets](https://technet.microsoft.com/en-us/library/dn376474.aspx) （PowerShell 的 DISM Cmdlets）
1. [Appx Module Cmdlets](https://technet.microsoft.com/en-us/library/dn448373.aspx) （PowerShell 的 Appx Module Cmdlets）
1. [Sideload Apps with DISM](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/manufacture/desktop/sideload-apps-with-dism-s14) （有关 Apps Sideload 的文章，内容广泛）

<br/>

<div align="right">8/12/2016 3:06:42 PM </div>