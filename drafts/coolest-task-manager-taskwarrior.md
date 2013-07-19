## 什么是Taskwarrior

[Taskwarrior](http://taskwarrior.org/)是一个终端下的任务管理工具，功能及其强大。
具体信息在它的官方网站上面已经介绍的很相近了。下面就列一段概述：

> It maintains a task list, allowing you to add/remove, and otherwise manipulate
> your tasks. Task has a rich set of subcommands that allow you to do
> sophisticated things. You'll find it has customizable reports, charts, GTD
> features, device synching, documentation, extensions, themes, holiday files
> and much more.

## 安装

### Ubuntu

### Mac

### Other

## Usage

### Add a task

	$ task add Buy a vps!
	$ task list

### Add a task to a project

	$ task add project:company Fix a bug
	$ task add proj:company Fix a bug
	$ task add pro:company Fix a bug
	# sub project
	$ task add project:company.server Fix a bug
	$ task project:company.server list
	$ task add project:company.client Fix a bug
	$ task project:company.client list
