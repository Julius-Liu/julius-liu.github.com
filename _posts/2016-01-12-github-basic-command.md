---
layout: post
title: 使用 Git 将本地代码上传到 Github - 简单命令
categories: [blog]
tags : [Git, 版本管理工具]
descriptions: 使用 Git 将本地代码上传到 Github - 简单命令
---

## 1. 注册Github账号 ##
到[Github](https://github.com/)注册Github账号。

## 2. 在Github上建立Github仓库 ##
1) 登录后点击右下方的“new repository”按钮新建一个仓库

![new-repository](/images/github-basic-command/new-repository.png) 

2) 填写完仓库信息后点击“creat repository”按钮创建仓库（仓库名字随意填写）

![create-repository](/images/github-basic-command/create-repository.png)

## 3. 下载并安装git版本管理工具 ##
到 [git-scm.com](http://git-scm.com/downloads/) 下载并安装git版本管理工具。

## 4. 配置git版本管理工具 ##
> Please tell me who you are.

打开命令行或 Git Bash，输入下面的命令：

    git config --global user.name "YourName"
    git config --global user.email "YourEmailAddress"

若省略了"--global"，则只配置当前仓库的用户信息。

## 5. 使用SSH密钥进行认证 ##
1) 生成SSH密钥，使用SSH方式认证登录

打开Git Bash，输入下面的命令：

	ssh-keygen -C "YourEmailAddress" -t rsa

然后直接按回车使用默认路径保存密钥文件到当前用户文件夹中 <br/>
密钥文件为（.ssh），接着设置密码和再次输入密码

2) 添加SSH密钥到GitHub

到该目录找到.ssh文件夹（id_rsa为私钥文件，id_rsa.pub为公钥文件） <br/>
使用记事本打开id_rsa.pub文件并复制文件内容 <br/> 
到GitHub中点击右上角的进入 personal settings 进行设置 <br/>
![person-settings-ssh](/images/github-basic-command/person-settings-ssh.png) <br/>
然后选择左边栏中的SSH Keys添加SHH Key粘贴刚才复制的内容到Key文本框中，title文本框中填写有意义的title。

## 6. 创建本地仓库并上传代码到GitHub ##
1) 新建 Test 文件夹作为仓库根目录（文件夹名字随意命名）
 
2) 将需要上传的代码文件加入到 Test 根目录
 
3) 在根目录下建立仓库

使用命令行或Git Bash，输入下面命令：先进入到 Test 根目录下，再输入git init（初始化一个仓库）

> 如果已存在仓库文件，则把隐藏文件夹 .git 删除，重新初始化一个仓库
 
4) 将所有文件添加到仓库

使用命令行或Git Bash，输入下面命令：

	git add . 

注意：这里有一个点。
 
5) 提交 <br/>
使用命令行或Git Bash，输入下面命令：

	git commit -m “CommitInfo”
 
> git commit -a 会先把所有已经track的文件的改动add进来,然后提交(有点像svn的一次提交,不用先暂存). 对于没有track的文件,还是需要git add一下.
 
6) 添加源到GitHub

	git remote add origin git@github.com:YourName/YourRepositroy.git
 
7) 上传源到GitHub

	git push -u origin master
 
> 以上命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。

## 7. 创建本地仓库并从GitHub同步代码 ##

> 注意：这里说的“同步代码”，具体来说，是取回远程主机某个分支的更新，再与本地的指定分支合并。

1) 新建文件夹作为仓库根目录（文件夹名字为 "YourRepositroy" ）
 
2) 在仓库根目录下建立仓库

使用命令行或Git Bash，输入下面命令：先进入到 Test 根目录下，再输入git init（初始化一个仓库）
> 如果已存在仓库文件，则把隐藏文件夹 .git 删除，重新初始化一个仓库
 
3) 添加源到GitHub 
 
	git remote add origin git@github.com:YourName/YourRepositroy.git
 
4) 从GitHub 上 pull 

	git pull origin master