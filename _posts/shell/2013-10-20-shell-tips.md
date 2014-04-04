---
layout: post
title: 实用Shell技巧
---

自定义Bash
-------
- bash_profile vs bashrc

	- `~/.bash_profile`在任何需要登陆的终端打开时运行（如ssh到远程计算机）
		
		事实上，在你登陆时，Shell会先运行`/etc/profile`。显然，这里的设置适用于所有用户。然后，Shell会运行下列文件中第一个存在的文件:` ~/.bash_profile`、`~/.bash_login`或`~/.profile`。它们则只适用于当前用户。另外，`~/.bash_logout`会在Shell关闭之前运行。

	- `~/.bashrc`在任何不需要登陆的终端打开时运行（如从桌面打开一个终端窗口）
		
		基于这个区别，通常的做法是把所有的环境设置放在`~/.bashrc`中，然后在`~/.bash_profile`中运行`~/.bashrc`。

			if [ -f ~/.bashrc ]; then . ~/.bashrc; fi

- 改变目录显示颜色
	
	在`~/.bashrc`文件中加入以下语句，然后重启Shell：

		LS_COLORS='di=0;35' ; export LS_COLORS

	其中数字0表示颜色深浅，数字35则是颜色的代号。下面列出了一些可选的颜色：

		Blue		34
		Green		32
		Light Green	1;32
		Cyan		36
		Red			31
		Purple		35
		Brown		33
		Yellow		1;33
		white		1;37
		Light Grey	0;37
		Black		30
		Dark Grey	1;30

- 改变命令提示符

	将命令提示符改为`[root ~/workspace] $`样式，其中`\e[1;32m`表示颜色，`\u`表示当前用户，`\w`表示当前目录，`\em`则是结束颜色标记（可省略）。

		PS1="\e[1;30m [\u \w] $ \em"

	下面的语句则使用了两种颜色，并加入了主机名，以及使用`\e[m`来消除对其他命令输出的影响。
	
		PS1="\e[1;30m[\u@\h] \e[1;32m\w $ \e[m"

	更多的选项包括

		\u   当前用户
		\w   当前目录
		\h   当前主机名
		\s   当前Shell名
		\d   当前日期
		\t   当前时间（24小时制，HH:MM:SS）
		\A   当前时间（24小时制，HH:MM）
		\T   当前时间（12小时制，HH:MM:SS）
		\!   当前历史记录数量

系统管理
--------
- 获取当前用户ID
		
		$ echo $EUID
		$ echo `id -u`

- 获取当前用户名
		
		$ whoami
		$ getent passwd $EUID | sed 's/:.*//'

- 以其他用户身份运行命令

		$ runuser -l httpd -c 'apachectl'

- 下载并运行远程shell脚本

		$ bash <(curl -s https://192.168.1.101/shell/install.sh)

- 查看进程

		$ ps -aux | grep python

- 删除进程

		$ kill -s 9 <pid>
		$ pkill -9 python

- 查看端口
	
		$ netstat -anp | grep 8000

查找和替换
--------
- 在当前目录下所有log文件中查找所有包含"2013-01-01"的行。

		$ find . -name "*.log" | xargs grep "2013-01-01"

- 在/var/log目录下查找包含hello的文件，并将所有hello替换为world

		$ sed -i "s/hello/world/g" `grep hello -rl /var/log`

条件逻辑
------
- `if [[` vs `if [`
		
	- `if [[`自动处理空字符串

	- `if [[`允许使用`&&`和`||`做布尔运算，`>`和`<`比较字符串

	- `if [[`支持正则表达式和通配符

	使用`if [`

		if [ "$USER" == 'httpd' -o "$USER" == 'httpd2' ]; then echo 'yes'; fi;

	使用`if [[`

		if [[ $USER == httpd || $USER == httpd2 ]]; then echo 'yes'; fi;
		if [[ $USER == httpd* ]]; then echo 'yes'; fi;