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

## Basic usage

### Add a task

添加一个任务很简单，直接`task add`后面跟上任务的描述就可以了。

	$ task add Buy a vps!
	$ task list

	ID Project Pri Due Active Age Description
	 1                         3s Buy a vps!

	1 task

### Delete a task

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

### Finish a task

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

### Basic manage the tasks' project

Taskwarrior的功能很强大，可以简单的为每个任务创建一个隶属的项目。可以有很多种方
法创建或修改任务隶属的项目组。

1. 可以在添加任务的同时指定任务所隶属的项目

      $ task add project:company Fix a bug
      $ task add proj:company Fix a bug
      $ task add pro:company Fix a bug
      $ task add project:company.server Fix a bug

2. qwe

	$ task project:company.server list
	$ task add project:company.client Fix a bug
	$ task project:company.client list

## Advance usage

### priority

	$ # Priority values may be 'H', 'M' or 'L'
	$ task add project:Home priority:H Find the adjustable wrench
	$ task
	ID Proj   Pri Age Urg  Description
	 2 diyBag M   6d  4.93 Buy tools for logo.
	 1 diyBag     10d 1.05 Buy the stuff to make the bag holder.
	$ task 2 modity priority:H
	[task next 2 modity priority:H]
	No matches.
	$ task 2
	ID Proj   Pri Age Urg  Description
	 2 diyBag M   6d  4.93 Buy tools for logo.
	$ task 2 modify pri:L
	Modifying task 2 'Buy tools for logo.'.
	Modified 1 task.
	$ task 2
	ID Proj   Pri Age Urg  Description
	 2 diyBag L   6d  2.83 Buy tools for logo.
	$ task pri.below:H
	$ task pri.over:H
	$ task pri.not:M

### due

	$ task add This is an urgent task due:tomorrow
	$ task due:tomorrow
	$ task due.before:today
	$ task list due.before:7d
	$ task 2 modify due: # 删除任务的到期日期
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
