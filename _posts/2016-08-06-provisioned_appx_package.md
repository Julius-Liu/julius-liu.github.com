---
layout: post
title: ProvisionedAppxPackage VS AppxPackage
categories: blog
tags: [职场]
description: ProvisionedAppxPackage 和 AppxPackage 的比较
---

## 正文 ##

　　先来说说问题的由来。<br/>
<br/>
　　在 Preinstall 的 component 中，有一支 component 叫做 MS_StartApp，这个 component 的行为是在预安装时为目标机器装入一些 Modern APP。所遇到的问题是，旧版的 MS_StartApp 在安装 Modern App 时，有部分 App 没有安装成功，新版的 MS_StartApp 没有这个问题。但是另外一个部门里手中的机器，是使用旧版本的 MS_StartApp 安装出来了，就会导致部分 App 没有安装，直观的反映就是在 Start Menu 中有一些 Tile 丢失了。<br/>
　　一个比较好的解决方案是，由我来开发一个单独运行的 component，来帮助安装上丢失的 App。<br/>
<br/>
　　于是我做了一些前期调研。首先以 MS_StartApp 作为 Sample Script 来学习，发现 Modern App 的源文件是.appx 或者 .appxbundle 文件，通过 DISM 的如下命令来安装：

	DISM Command-Line Script:
	DISM /Online /Add-ProvisionedAppxPackage

　　安装完成之后，发现其实这个 App 可以是一个 ProvisionedAppxPackage，也可以是一个 AppxPackage。有关 AppxPackage，已有的知识告诉我可以通过 PowerShell 的方式安装：

	PowerShell Appx Module Cmdlets Script:
	PS > Add-AppxPackage

　　未完成。。