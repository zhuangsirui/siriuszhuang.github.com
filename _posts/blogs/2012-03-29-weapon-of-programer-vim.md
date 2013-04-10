---
layout: post
title: 程序员的传家宝VIM
description: 工欲善其事，必先利其器，程序员的紫色传家宝VIM
category: blog
---

> 很早就想写一篇关于VIM的文章了，无奈网上遍地都是VIM的练级攻略和赞歌，确实不知道
> 该如何来写，不知道怎样才能帮助到那些想用却又力不足的人。所以，尽力为之吧，如果
> 有人因为这篇文章，让自己想学VIM的决心又增大了一点，也算欣慰了。

## 关于VIM
VIM是一个纯文本的编辑器，与其他编辑器最大的区别就是他有几种不同的模式用于不同的场景，这恰恰也是VIM入门困难的原因，因为对于一个新手来说，打开了文件，按下了键盘但文字却不会出现在想要的为之，是一件很困惑的事情。但千万别让这一点小困难打败你，因为这正是VIM能够高效编辑的原因。

## VIM初入门
你可以用`yy`复制一行，可以用`vip`选择一个段落，可以用`G`跳转到最后一行，用`gg`跳转到第一行。当然VIM能做的远远不止这些，如果想熟悉VIM的基本操作，可以玩一下[这款](http://vim-adventures.com/)有意思的游戏，有爱的作者把VIM陡峭的学习曲线变成了一款“益智类”游戏。玩家一开始只能用`hjkl`键控制角色上下左右的移动，之后会“捡到”一些新技能，例如用`w`跳转到下一个单词的首字母，用`e`跳转到下一个单词的最后一个字母等等。当玩家通关之后，相信对VIM的基本操作已经比较熟悉了。

当我们打开一个全新的VIM时，看到的仅仅是一个最基础的编辑器，没有代码高亮，没有自动缩进等等，所以，至少先把一下这段加入到你的VIM配置中，Linux/Unix下的路径默认为`~/.vimrc`。

	" 让VIM不兼容VI"
	set nocompatible

	" 语法高亮"
	syntax on

	" 显示行号"
	set number

	" 不自动备份"
	set nobackup

	" 支持filetype插件"
	filetype plugin on

	" 自动缩进"
	filetype indent on


## VIM的插件与配置
现在，请拿起你的VIM，请相信他，你的紫色传家宝，在你编码的时候，你的VIM也会和你一样汲取经验，变得更强大，而且更易用。什么？不可能？那你一定没有尝试过VIM的插件以及插件管理工具。

### VIM的插件
VIM的插件有不少，大家可以在[VIM](http://www.vim.org/scripts/index.php)中找到，按照插件的文档，将插件文件解压到`~/.vim`中的对应位置就可以了。如果想要通过这种原始的方法安装VIM插件的话，[这里](http://blog.csdn.net/wooin/article/details/1858917)讲解了大部分常用的VIM插件的安装、配置方法。

但是这种传统的方法很有局限性，因为，那些插件会在你的`~/.vim`目录中杂乱的放着，管理的时候非常不方便。而且当你想重新配置一个全新的VIM时，不得不把之前整个`~/.vim`目录拷贝一次，或者把他们全部通过GIT管理，并且托管到Github上（之前我的做法）。

### 插件管理工具
如果尝试过上面安装VIM插件的方法之后，觉得安装插件很容易的请忽略这一段。因为下面我会介绍一款管理VIM插件的插件——*vundle*，有趣的是，这款插件在管理其他插件的同时也管理自己。

从现在开始，忘掉手动管理VIM插件的历史。新建一个`~/.vim`（也可以创建到其他地方，最后做一个软连接），新建你的`.vimrc`文件，加入下面几行配置：

	set nocompatible

	filetype off

	set rtp+=~/.vim/bundle/vundle/

	call vundle#rc()

	Bundle 'gmarik/vundle' " 必填项 让Vundle自己更新自己"

	" 添加自己喜欢的插件 {"

		" 直接填写名称的插件，Vundle会在vim-script.org中寻找对应的插件"
		Bundle 'ZenCoding.vim'
		Bundle 'The-NERD-tree'

		" 对于这种AA/BB格式的插件，Vundle自动会去Github中找AA用户的BB仓库"
		Bundle 'tpope/vim-rails'

		" 对于其他Git地址，则需要填写完整路径"
		Bundle 'git://git.wincent.com/command-t.git'

	" }"

	filetype plugin indent on

保存退出后，执行以下命令安装Vundle：

	mkdir ./bundle/vundle

	git clone https://github.com/gmarik/vundle.git ./bundle/vundle

再运行VIM，执行`:BundleInstall`安装自己配置的其他插件。就这么简单。以后，只需要维护一个`.vimrc`便可以让自己的VIM不断更新，随着自己一起成长。太棒了不是吗？

## 我的VIM配置

如果大家嫌还是太麻烦了，可以直接使用我的[VIM配置](https://github.com/siriuszhuang/vim.git)，或者任意修改调戏不客气！

对于刚开始使用VIM的人来说，一定要坚持，当你习惯了VIM之后，会发现已经对这种操作爱不释手了。
