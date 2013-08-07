---
layout: post
title: 任务管理工具Taskwarrior
description: 任务管理工具Taskwarrior
category: blog
---

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

	sudo apt-get install task

### Mac with brew

	brew install task

## 基础用法

### 新增一个任务

添加一个任务很简单，直接`task add`后面跟上任务的描述就可以了。

	$ task add Buy a vps!
	$ task list

	ID Project Pri Due Active Age Description
	 1                         3s Buy a vps!

	1 task

### 删除一个任务

删除一个任务，需要做的是运行`task <filter> delete`。这里的`<filter>`暂时可以简单
的看做这个任务的ID，后面会详细介绍`<filter>`的用法。:)

	$ task
	[task next]

	ID Project Pri Due A Age Urgency Description
	 1                   29s       0 Buy a vps!

	1 task
	$ task 1 delete
	Permanently delete task 1 'Buy a vps!'? (yes/no) yes
	Deleting task 1 'Buy a vps!'.
	Deleted 1 task.

### 将任务标记为完成

完成一个任务和删除一个任务很相似，运行`task <filter> done`。

	$ task add Buy a vps!
	Created task 2.
	$ task
	[task next]

	ID Project Pri Due A Age Urgency Description
	 1                    1s       0 Buy a vps!

	1 task
	$ task 1 done
	Completed task 1 'Buy a vps!'.
	Completed 1 task.

### 将任务纳入隶属的项目

Taskwarrior的功能很强大，可以简单的为每个任务创建一个隶属的项目。可以有很多种方
法创建或修改任务隶属的项目组。

* 可以在添加任务的同时指定任务所隶属的项目

		$ task add project:company Fix a bug
		$ task add proj:company Fix a bug # taskwarrior 会自动匹配唯一的子命令
		$ task add pro:company Fix a bug # taskwarrior 会自动匹配唯一的子命令
		$ task add project:company.server Fix a bug # 项目可以分类成子项目

* 可以使用`task <filter> modify proj:xx`命令在已经创建的任务上添加或修改它的隶
  属项目。

		$ task 2 modify proj:company

* 查看指定项目下的任务

		$ task proj:company list

* 删除任务的隶属项目

		$ task 1 proj:

## 高级用法

### 优先级

Taskwarrior允许设置任务的优先级。分别有`L`(Low)，`M`(Middle)和`H`(High)三个级别
。

* 有了上面所讲到的project知识，理解优先级就不是什么难事了。

		$ task add project:Home priority:H Find the adjustable wrench
		$ task 1 modify priority:M
		$ task 1 modify pri:L # 同样的Taskwarrior可以自动理解子命令的简称
		$ task 1 modify pri: # 删除任务的优先级

* 值得一提的是针对优先级的选择器，也就是上面提到的`filter`

		$ task pri.below:H # High等级以下的任务
		$ task pri.over:L # Low等级以上的任务
		$ task pri.not:M # 不是Middle等级的其他所有任务

### 截止日期

既然是任务管理，没有截止日期还能算强大么？所以，理所当然的，Taskwarrior的`due`就
应运而生了（其实我一点儿都不觉得理所当然，时时刻刻感谢Taskwarrior开发者们的良苦
用心）。

* 为一个任务添加截止日期

		$ task add This is an urgent task due:tomorrow # 将到期时间设置为明天
		$ task 1 modify due:tomorrow # 将到期时间设置为明天
		$ task 1 modify due:2013-03-12 # 将到期时间设置为指定时间
		$ task 1 modify due:eom # 将到期时间设置为当月的最后一天（end-of-mouth）
		$ task 1 modify due:eoy # 将到期时间设置为今年的最后一天（end-of-year）
		$ task 1 modify due: # 删除任务的到期时间

* `due`过滤器的使用方法

		$ task due:tomorrow
		$ task due.before:today
		$ task list due.before:7d
		$ task overdue

### annotate/denotate

	$ task 2 annotate This is an annotate.
	$ task 2 annotate This is another annotate.
	$ task 2 denotate annotate

### tags

	$ task 2 +mail
	$ task +mail list
	$ task 2 -mail

### prepend,append

	$ task add music
	$ task 8 prepend Select some
	$ task 8 append for after dinner
	$ task dinner list

	ID Project Pri Due Active Age     Description
	-- ------- --- --- ------ ------- ----------------------------------
	 8                        34 secs Select some Music for after dinner

### recur,until

	$ task 1 modify due:eom recur:monthly
	$ task 2 modify due:eom recur:yearly
	$ task 3 modify due:eom recur:monthly until:eoy
	$ task recurring

## Query advance

### Limit

	$ task limit:1

### ext

	$ task description.has:foo list
