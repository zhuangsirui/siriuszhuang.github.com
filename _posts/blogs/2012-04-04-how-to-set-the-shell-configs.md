---
layout: post
title: 如何设置shell的配置文件
description:
    - 对于天天都必须用的shell，有很多的配置文件，他们之间的关系与区别是什么？
category: blog
---

> 对于天天在shell下面工作的程序员来说，各种bash的配置文件都是再熟悉不过了。像`.bashrc`，`.bash_profile`和`.profile`等等。现在让我们一起来梳理一下这几个文件的关系到底是怎么回事。

## bash的加载流程

想要搞清楚这些文件的作用，首先就需要说明一下bash的加载流程。

### man bash
> 以下是bash手册中的一段摘抄，解释了bash加载的流程。

当bash作为一个交互式的login shell（或者一个非交互式但选项`--login`存在）被调用时，首先会读取`/etc/profile`文件（如果存在），读取并执行其中的shell命令。之后，bash会按顺序分别读取`~/.bash_profile`，`~/.bash_login`和`~/.profile`文件（如存在），并执行找到的第一个文件中的shell命令，之后的文件便不再读取。如果想禁止bash读取这些文件的话，可以加上`--noprofile`这个可选参数。

当一个login shell推出的时候，bash会尝试读取`~/.bash_logout`文件（如存在），并执行其中的shell命令。

当一个交互式的non-login shell被调用时，bash会读取`~/.bashrc`文件，并执行其中的命令。同样的，如果想禁用这一个行为的话，可以加上选填参数`--norc`。`--rcfile <file>`参数可以强制bash使用指定文件以替代默认的`~/.bashrc`文件。

当一个非交互式bash被激活，就像运行一个shell脚本，它会寻找`BASH_ENV`环境变量，将其中的值作为文件名分别读取并执行。这个行为就像执行了以下命令：

	if [ -n "$BASH_ENV" ]; then . "$BASH_ENV"; fi

需要注意的是`PATH`环境变量不会作为文件名去寻找并执行。

### 交互式与非交互式的区别
看了上面的解释之后，还需要明确以下login shell与non-login shell的区别。简单的说，login shell是指的需要完成登录流程激活的shell，比如，在开机的时候，或者远程连接一台计算机时的登录流程。non-login shell指的是在已有的shell中再调用shell不需要重新完成登录流程激活的shell。就像已经进入了我的Mac之后，打开的iterm2，便是non-login shell。

更详细的介绍可以看[这篇博客](http://www.isayme.org/linux-diff-between-login-and-non-login-shell.html)， 写的简单易懂。在这里谢谢他。

## 各种文件的区别以及用法

现在，在知道了bash的加载流程之后，在理解各种文件的用法就不难了。

* `/etc/profile`：login shell加载的文件，在系统启动，远程登录时，都会为所有用户将此文件加载。一些系统需要的一些环境变量可以放在这里。
* `~/.bash_profile`,`~/.bash_login`,`~/.profile`：在上面文件加载完成后加载，区别是仅为当前登录用户加载此文件（当然了，是在HOME目录下）。可以将用户自己需要的一些环境变量放到这个文件中。需要注意的是，bash在加载完成`/etc/profile`之后，会按照顺序读取第一个找到的文件。所以最好不要这三个文件同时存在。
* `~/.bashrc`：仅在non-login shell环境中加载此文件，所以，这里放入自己喜欢的别名、一些喜欢的路径或者是一些方便的shell函数是再适合不过的了。

## 一些例子

下面是我用这些配置文件的例子，也算是总结一下吧。

### ~/.profile

> 用来载入需要的环境变量，rvm、以及自己home目录中的命令等等

	# Load RVM into a shell session *as a function*
	[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"

	# Add RVM to PATH for scripting
	[[ -d $HOME/.rvm/bin ]] && PATH=$PATH:$HOME/.rvm/bin

	# Add user bin PATH
	[[ -d $HOME/bin ]] && PATH=$PATH:$HOME/bin

### ~/.bashrc

> 自己shell需要的一些别名等等，其中又把所有的别名放在了`~/.bash_aliases`下加载，也算是解耦一下吧。影响中Ubuntu就是这么干的。之前一直以为bash会自动加载`~/.bash_aliases`，可是Mac下却无效，现在知道是怎么回事了。

	# Set the default editor to vim.
	export EDITOR=vim

	# Load bash aliases.
	[[ -r "$HOME/.bash_aliases" ]] && source "$HOME/.bash_aliases"

### ~/.bash_aliases

> 上面提到的`~/.bash_aliases`

	# aliases for tmux
	alias tn="tmux new -s"
	alias ta="tmux a -t"

	# aliases for git
	alias gd="git diff | tig"

	# aliases for ps
	alias 'ps?'='ps aux | grep'

	# alias for ping
	alias p="ping"

## 关于ZSH

其实，进来一直都用的zsh替代bash，原因是一个叫*oh my zsh*的shell配置用着实在爽，但是zsh的加载流程貌似又和bash不太一样。为了让zsh继承bash的配置又不像DRY（don't repeat yourself）。好在zsh是完全兼容bash的。于是在`~/.zshrc`以及`~/.zprofile`中添加了几行好让zsh继承bash的配置。

### .zprofile

> 这个是zsh在login shell会调用的文件，让他继承`~/.bash_profile`就是了。

	# Load bash profile
	[[ -f "$HOME/.bash_profile" ]] && source "$HOME/.bash_profile"

### .zshrc

> 这个是zsh在non-login shell会加载的文件，其实和bash是差不多的。同样的，在它的最后一行让他“继承”`~/.bashrc`就好了。

	[[ -r "$HOME/.bashrc" ]] && source "$HOME/.bashrc"

	# reload non-login shell env
	alias reload="source $HOME/.zshrc"
