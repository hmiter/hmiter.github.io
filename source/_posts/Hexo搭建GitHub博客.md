title: Hexo搭建GitHub博客
date: 2015-11-04 23:40:59
tags: Git
categories: 编程
---
# 1. 环境环境
## 1.1 安装Git
### 1.1.1下载
	https://git-for-windows.github.io/

### 1.1.2安装
1. 安装过程中，询问是否修改环境变量，选择“Use Git Bash Only”. 即只在msysGit提供的Shell

(**NOTE**: 这个步骤最好选择第二项“Use Git from the Windows Command Prompt”， 这样在Windows的命令行cmd中也可以运行git命令了。这样会对以后的一些操作带来方便，
比如Win7下安装配置gVim)

2. 配置行结束标记，保持默认“Checkout Windows-style, commit Unix-style line endings”.

### 1.1.3中文乱码问题解决方法
ls 不能显示中文目录

解决办法：在git/git-completion.bash中增加一行【4】：

	alias ls='ls --show-control-chars --color=auto'
另外，Git Shell 不支持 ls -l的缩写形式ll，也为其添加一个alias

	alias ll='ls -l'

### 1.1.4 运行 Git 前的配置
1. 配置你个人的用户名称和电子邮件地址
	
    $git config --global user.name "xxx"

    $git config --global user.email xxx@example.com

2.  配置GitHub SSH 

（1）首先使用 ssh-keygen 生成 SSH 密钥

	cd ~/.ssh/
	Administrator
@THINKPAD ~/.ssh
	$ ls
	 known_hosts
	$ssh-keygen -t rsa -C "youremail@example.com"
有提示，直接回车即可。生成key以后检查下"~"目录下的.ssh目录下是否多了2个文件 。

把 id_rsa.pub 中的全部内容复制，包括最后的一个换行。或者用命令：

	clip < ~/.ssh/id_rsa.pub
（2）配置Github SSH。

登陆GitHub->Settings->“SSH Keys”，然后，点“Add SSH Key”，起个Title，在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”。

## 1.2 安装node.js
下载：http://nodejs.org/download/

可以下载 node-v4.2.1-x64.msi

安装时直接保持默认配置即可。


----------

# 2. 配置Github
## 2.1建立Repository
建立与你用户名对应的仓库，仓库名必须为【your_user_name.github.io】

----------

# 3. 安装Hexo

关于Hexo的安装配置过程，请以官方Hexo【2】给出的步骤为准。

## 3.1 Installation

打开Git命令行，执行如下命令

	$ npm install -g hexo
## 3.2 Quick Start

**1. Setup your blog**

在电脑中建立一个名字叫「Hexo」的文件夹（比如我建在了E:\Hexo），然后在此文件夹中右键打开Git Bash。执行下面的命令

	$ cd /E/Hexo
	$ hexo init
	[info] Copying data
	[info] You are almost done! Don't forget to run `npm install` before you start b
	logging with Hexo!
Hexo随后会自动在目标文件夹建立网站所需要的文件。然后按照提示，运行 npm install（在 /D/Hexo下）

	npm install
会在E:\Hexo目录中安装 node_modules。

** 2. Start the server**

运行下面的命令（在 /E/Hexo下）

	$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
表明Hexo Server已经启动了，在浏览器中打开 http://localhost:4000/，这时可以看到Hexo已为你生成了一篇blog。

你可以按Ctrl+C 停止Server。

** 3. Create a new post**

新打开一个git bash命令行窗口，cd到/D/Hexo下，执行下面的命令

	$ hexo new "My New Post"
	[info] File created at d:\Hexo\source\_posts\My-New-Post.md
刷新http://localhost:4000/，可以发现已生成了一篇新文章 "My New Post"。

**NOTE：**

有一个问题，发现 "My New Post" 被发了2遍，在Hexo server所在的git bash窗口也能看到create了2次。

	$ hexo server
	[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
	[create] d:\Hexo\source\_posts\My-New-Post.md
	[create] d:\Hexo\source\_posts\My-New-Post.md
经验证，在hexo new "My New Post" 时，如果按Ctrl+C将hexo server停掉，就不会出现发2次的问题了。

所以，在hexo new文章时，需要stop server。

**4. Generate static files**

执行下面的命令，将markdown文件生成静态网页。

	
该命令执行完后，会在 E:\Hexo\public\ 目录下生成一系列html，css等文件。

**5. 编辑文章**

hexo new "My New Post"会在E:\Hexo\source\_posts目录下生成一个markdown文件：My-New-Post.md

可以使用一个支持markdown语法的编辑器（比如 Sublime Text 2）来编辑该文件。


**6.  部署到Github**

运行  `npm install hexo-deployer-git --save` 安装git支持

部署到Github前需要配置_config.yml文件，首先找到下面的内容

	# Deployment
	## Docs: http://hexo.io/docs/deployment.html
	deploy:
	  type:
然后将它们修改为

	# Deployment
	## Docs: http://hexo.io/docs/deployment.html
	deploy:
	  type: github
	  repository: git@github.com:zhchnchn/zhchnchn.github.io.git
	  branch: master
**NOTE1:**

Repository：必须是SSH形式的url（git@github.com:zhchnchn/zhchnchn.github.io.git），而不能是HTTPS形式的url（https://github.com/zhchnchn/zhchnchn.github.io.git），否则会出现错误：

	$ hexo deploy
	[info] Start deploying: github
	[error] https://github.com/zhchnchn/zhchnchn.github.io is not a valid repositor URL!
使用SSH url，如果电脑没有开放SSH 端口，会致部署失败。

fatal: Could not read from remote repository.

Please make sure you have the correct access rights and the repository exists.

**NOTE2：**

如果你是为一个项目制作网站，那么需要把branch设置为gh-pages。

**NOTE3：**

hexo3.0以上的版本type为git

**7. 测试**

当部署完成后，在浏览器中打开http://zhchnchn.github.io/（https://zhchnchn.github.io/） ，正常显示网页，表明部署成功。

**8. 总结：部署步骤**

每次部署的步骤，可按以下三步来进行。

	hexo clean
	hexo generate
	hexo deploy
**9. 总结：本地调试**

1. 在执行下面的命令后，

    $ hexo g #生成

    $ hexo s #启动本地服务，进行文章预览调试

浏览器输入http://localhost:4000，查看搭建效果。此后的每次变更_config.yml 文件或者新建文件都可以先用此命令调试，尤其是当你想调试新添加的主题时。

2. 可以用简化的一条命令

	hexo s -g