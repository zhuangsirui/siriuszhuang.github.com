# Rsync

## Rsync简介
Rsync 是一个和服务器端同步数据的软件，它可以差异化的同步一整个目录树中的文本文件和二进制文件，并且即使是二进制文件也都是差异化同步的。[这里](https://zh.wikipedia.org/zh-cn/Rsync)是Rsync的简介，大家感兴趣的不妨看一看。另外，[这里](https://zh.wikipedia.org/wiki/Rsync#.E6.BC.94.E7.AE.97.E6.B3.95)直接阅读其差异化同步文件算法的文章，相信看了之后会对大家的算法提升有帮助的。

因为平时大家操作服务器的时候大都是用ssh协议进行通讯的，这样安全能得到保障，配合证书也够方便，下面就先向大家讲讲如何用ssh协议与服务器端进行数据同步。

## 用SSH协议使用Rsync
其实，当我们有了SSH链接之后，使用rsync几乎是零配置的，但是如果不用ssh，那么就得在服务器端写一个配置文件，再启一个守护进程来完成同步需求了。这一步让我们以后在讨论。

在这里我假设大家已经能够熟练地使用ssh连接服务器进行日常操作。如果不懂也没关系，一样能够看懂rsync的基本操作。但是在看完之后，再去看看ssh的用法也是非常有必要的。

### 首次同步
首先，确定你的电脑能够用ssh连接远端服务器（当然也可以是你电脑上的一个虚拟机），接下来，就让我们以命令来说话吧。

	$ mkdir test-rsync && cd test-rsync
	$ touch file-1.txt file-2.txt file-3.txt
	$ ls
	file-1.txt file-2.txt file-3.txt

显而易见的，我们创建了一个叫`test-rsync`的目录，并且创建了`file-1.txt`,`file-2.txt`,`file-3.txt`三个文件，现在，我们想要在服务器上面同步一份一模一样的目录，让我们看看做怎么做:

	$ cd ..
	$ rsync -r test-rsync root@com.itpea:~/ # 将整个目录同步到服务器的`$HOME`根目录下

然后让我们看一下服务器里面是不是像我们想象的一样是否同步了我们的`test-rsync`这个目录:

	$ ssh root@com.itpea
	# ls
	test-rsync
	# cd test-rsync
	# ls
	file-1.txt file-2.txt file-3.txt

我们看到，在服务器root用户的目录下，同样的创建了一个`test-rsync`目录，里面有我们刚才在本机创建的三个`txt`文件。这样，我们首次同步就算是完成了。

### 对文件的修改同步
如果rsync就只有以上介绍的功能，那么完全就是一个scp的别名罢了。当然，你我都知道他还能做得更多，下面就来聊聊如何对文件的修改同步吧。

接着刚才的目录做演示吧:

	$ cd test-rsync
	$ touch file-4.txt # 新增加一个文件
	$ mv file-2.txt file-2
	$ rm file-2.txt
	$ rsync *.txt root@com.itpea:~/test-rsync
	$ ssh root@com.itpea ls ~/test-rsync # 不登陆服务器直接运行一条命令
	file-1.txt
	file-2.txt
	file-3.txt
	file-4.txt

因为在上一步中，我们已经将目录同步到了服务器上面，现在就可以通过一些文件通配符来同步指定的文件到服务器上创建好了的目录了。这样，我们可以看到，服务器上的`~/test-rsync`目录中，已经有了我们添加的`file-4.txt`，却没有重命名过的`file-2`文件。太棒了，这说明文件通配符起作用了，不是吗？让我们将所有的文件同步到服务器吧:

	$ rsync * root@com.itpea:~/test.rsync
	$ ssh root@com.itpea ls test-rsync
	file-1.txt
	file-2
	file-2.txt
	file-3.txt
	file-4.txt

结果和我们预期的一半一半吧，因为我们看到`file-2`已经同步到服务器了，但是`file-2.txt`应该不在了才对啊。这是因为rsync在同步这种才做的时候其实是很具有破坏性的，如果我们不小心删除了本机的文件，并且用rsync同步的时候默认也将远端的文件删除了，岂不是会天下大乱？介于此原因（应该是吧），rsync的作者要求我们想同步删除文件的时候，必须显示的说明，那就是加入`--delete`参数，这样就会减少很多灾难性的操作了。

	$ rsync -r --delete ./ root@com.itpea:~/test-rsync
	$ ssh root@com.itpea ls test-rsync
	file-1.txt
	file-2
	file-3.txt
	file-4.txt

这样，我们就同步删除了服务器上面的`file-2.txt`文件了。
