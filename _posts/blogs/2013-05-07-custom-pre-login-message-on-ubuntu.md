---
layout: post
title: 让Ubuntu的登录界面显示当前的IP地址。
description: 让Ubuntu的登录界面显示当前的IP地址，这样可以直接在外部用ssh连接了。
category: blog
---

# 自定义Ubuntu登录信息

## 为什么有这个需求

有时候工作需要，我需要在自己的电脑上面安装一个服务器版的Ubuntu，在iTerm下用ssh连
接使用。每次启动虚拟机的时候，由于IP都是未知的，所以需要在虚拟机的交互界面下输入
用户名和密码登录，之后再`ifconfig`。然后才能得到虚拟机的IP地址。

作为一个合格的程序员，当然不能坐等这种无脑的行为发生在每天的生活和工作当中。于是
就有了下面的解决方法，是在[这里]()看到的。下面我会更详细的介绍这个方法的细节。

## Where is the message?

首先，我们得找到放置这个`pre-login`信息的位置。通过伟大的Google，很容易的可以知
道，这个`pre-login`信息是放在`/etc/issue`文件中的。打开以后，就会看到当前的
`pre-login`信息了。令人高兴的是，Ubuntu内建了一些字符，可以帮助我们加入一些动态
的信息进去。见下表：

	\b : Insert the baudrate of the current line.
	\d : Insert the current date.
	\s : Insert the system name, the name of the operating system.
	\l : Insert the name of the current tty line.
	\m : Insert the architecture identifier of the machine, eg. i486
	\n : Insert the nodename of the machine, also known as the hostname.
	\o : Insert the domainname of the machine.
	\r : Insert the release number of the OS, eg. 1.1.9.
	\t : Insert the current time.
	\u : Insert the number of current users logged in.
	\U : Insert the string "1 user" or " users" where is the number of current users logged in.
	\v : Insert the version of the OS, eg. the build-date etc.

## Can I write some shell cmd?

事情好像到这里就结束了，按照常理，我们只需要在这里写一些shell脚本就可以到楼下和
我的小伙伴们玩游戏了。但是，Ubuntu好像不可以解释其中的shell脚本。当然了，我们看
到的也只是一些信息，没有什么`echo`之类的在里面。这可叫我如何是好？

## What else I can do?

难道这个小小的需求尽然不能满足吗？

漫长的搜索中...

然后就找到了那边Blog。感谢作者的无私奉献！

原来，Ubuntu在每次网络启动成功后，都会默认执行`/etc/network/if-up.d`这个目录下的
所有可执行文件。这样的话，思路就很明显了，我们首先需要在`/etc/`中，创建一个除了
IP信息的欢迎信息在其中，之后每当网络配置成功的时候，系统就会自动将这个文件拷贝成
为`/etc/issue`文件，并在其后面添加IP信息。

另外值得注意的是，当这个程序运行的时候，不会加载任何的shell环境。所以所有的
`alias`都没办法工作。这样的话，我们就需要在`/bin`目录下写一个简单的shell脚本用来
获取当前的IP信息。

让我们开工吧！

	sudo echo "/sbin/ifconfig | grep "inet addr" | grep -v "127.0.0.1" | awk '{ print $2 }' | awk -F: '{ print $2 }'" > /usr/local/bin/get-ip-address
	sudo chmod +x /usr/local/bin/get-ip-address

上面这个脚本用来获取IP地址。

之后，在`/etc/network/if-up.d/show-ip-address`文件中写上下面的shell

	#!/bin/sh
	if [ "$METHOD" = loopback ]; then
		exit 0
	fi

	# Only run from ifup.
	if [ "$MODE" != start ]; then
		exit 0
	fi

	cp /etc/issue-standard /etc/issue
	/usr/local/bin/get-ip-address >> /etc/issue
	echo "" >> /etc/issue

让其可执行`sudo chmod +x /etc/network/if-up.d/show-ip-address`

## Done!

大功告成！重虚拟机吧！
