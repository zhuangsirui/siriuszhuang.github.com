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

	$ task add Buy a vps!
	$ task list

### Delete a task

### Finish a task

### Basic manage the tasks

	$ task add project:company Fix a bug
	$ task add proj:company Fix a bug
	$ task add pro:company Fix a bug
	$ task add project:company.server Fix a bug
	$ task project:company.server list
	$ task add project:company.client Fix a bug
	$ task project:company.client list
	$ task 1 edit
	$ task 1 done
	$ task 1 delete

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
