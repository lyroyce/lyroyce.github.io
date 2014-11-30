---
layout: post
title: 实用教程：搭建GitHub环境
---

安装Git
-------
下载并安装[Git客户端](http://git-scm.com/downloads)。

使用GitBash(For Windows)
-------
对于Windows用户而言，GitBash绝对不容错过。它提供了全套Linux命令实现，使用它和使用Linux Shell几乎没有区别。

生成SSH Key
-------
如果你想和GitHub通信，最方便和安全的方式是使用SSH。如果你不这么做，GitHub将默认使用用户名密码认证。

在Shell(GitBash)中输入以下命令：
		
	$ mkdir ~/.ssh
	$ ssh-keygen -t rsa -C "your_email@your_email.com"

默认情况下，刚刚生成的key就保存在`~/.ssh`目录下，其中`id_rsa.pub`是公钥，`id_rsa`则是私钥。

接下来，我们需要把公钥交给GitHub。可以先使用下面其中一条语句来复制你的公钥到剪贴板，然后在GitHub Account Settings的SSH Keys页面中添加这个公钥。

	$ clip < ~/.ssh/id_rsa.pub 				# Windows
	$ pbcopy < ~/.ssh/id_rsa.pub 				# Mac
	$ xclip -sel clip < ~/.ssh/id_rsa.pub 	# Linux

最后我们可以输入以下语句来测试SSH连接：

	$ ssh -T git@github.com
	Hi username! You've successfully authenticated, but GitHub does not provide shell access.

如果你处于公司内网，并且上述测试得到`Bad file number`错误，你需要尝试在你的SSH配置文件`~/.ssh/config`中添加下面的配置并再试一次。

	Host github.com
	  Hostname ssh.github.com
	  Port 443

开始GitHub
------
- 创建新项目

先在GitHub界面上创建一个新项目，运行下面命令来下载到本地：

	$ git clone git@github.com:username/projectname.git projectname

- 上传已有项目

依然先在GitHub界面上创建一个新项目，然后运行命令：

	$ git init
	$ git remote add origin git@github.com:username/projectname.git
	$ git push origin master
