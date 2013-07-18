---
layout: post
title: Tmux使用方法
description: 一个超级shell窗口管理工具，使你的shell操作效率成倍增长。
category: blog
---

## Tmux 概览

简单的说[Tmux](http://tmux.sourceforge.net/)就是一个“窗口”管理器。窗口之所以要打
上引号是因为它不同于我们所熟知的图形界面窗口，而是在终端下的基于命令行的纯文本窗
口。

## 安装Tmux

安装tmux很简单，我是在Mac下使用的brew安装的tmux：

	brew install tmux

如果是Ubuntu的话，运行：

	sudo apt-get install tmux

其他平台也可以使用自己的软件仓库安装，如果有自虐倾向的人也可以编译安装。这里就不
赘述了。

## 使用Tmux

tmux使用很简单，所有的操作都是通过一个`prefix-key`加上另一个字符完成的。首先，我
们用`tmux new -s test`命令创建一个tmux会话。其中`tmux new`是创建一个会话的意思，
参数`-s test`是将这个会话命名为`test`。如果为新建的会话命名的话，tmux会自动的为
之命名，如果当前电脑上只有一个会话则名字为`1`，以此类推。

如果不出意外的话，现在你已经`attach`了一个tmux会话，可以在tmux中随心所欲的管理你
的工作shell窗口了。但是先别慌，首先你需要确定自己的tmux`prefix`键是什么。如果你
什么都没有配置，并且在你的`$HOME`目录中也没有`.tmux.conf`文件的话，那么你的tmux
`prefix-key`应该是`^b`，也就是按住`contorl`键的同时按`b`（实际上已经有很多人吐槽
默认按键了，不仅左手跨度大，而且直接覆盖了shell中Emac的左移光标的key-binding）。

现在你已经确认自己的`prefix-key`是什么了吧，现在就让我们在tmux下新建一个窗口。默
认新建窗口的key是`c`，也就是`create`的首字母。好的，现在先激活tmux的命令模式（也
就是按一次tmux的`prefix-key`，主意按完之后需要放开`contorl`键）。之后，在单独按
下`c`键，看看发生了什么？如果注意到tmux最下边的状态栏的话，就会发现，看起来我们
新建了一个窗口。如果想要切换到前一个窗口可以用`prefix-key`+`p`，下一个窗口可以使
用`prefix-key`+`n`。

## 在窗口和面板之间切换

tmux就是一个窗口管理工具，所以一切功能都是围绕着管理工作窗口而生的。在tmux中可以
在纯文本界面下实现几乎所有你想要的功能。
[这里](http://happycasts.net/episodes/41)有一个讲解tmux的视频，讲得非常不错，有
兴趣的同学自行跳转观看。

## 配置Tmux

> tmux的配置很简单（甚至不用配置），以至于大多数人仅仅替换一个`prefix-key`就可以
> 顺手的使用了。

tmux的配置文件是放在`$HOME`目录下面的，文件名是`.tmux.conf`。前面已经讲到过，默
认的，tmux的`prefix-key`是`^b`，但是可以通过设置绑定其他的按键。个人爱好是绑定到
`^q`这里。至于其他的配置，可以参考
[这里](https://github.com/siriuszhuang/configs/blob/master/tmux/.tmux.conf)。喜
欢的话可以运行下面的命令直接使用：

	wget https://raw.github.com/siriuszhuang/configs/master/tmux/.tmux.conf ~/

## Tmux 屏幕非全屏的解决办法

其实这一段才是我写这篇文章的最初目的，因为最近我遇到一个奇怪的事情，就是使用了一
段时间的tmux之后，稍微改变一下我iterm的大小，tmux就不能全屏了。但是我确实没有在
其他的地方attach同一个tmux会话，很苦恼。解决办法是，detach所有的tmux会话，然后用
`ps aux | grep tmux`可以看到有一个孤儿进程attach了tmux会话的，可能是因为我不小心
在使用iterm的时候直接关掉了他而不是退出，这样做attach的进程不会因此而中断。所以
只需要kill掉那个进程就ok了（**千万注意不要杀掉tmux的会话进程**）。
