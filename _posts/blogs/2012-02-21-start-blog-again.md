---
layout: post
title: Start blog again
description: Geting started!
category: blog
---

## 开博大典

又一次写开博大典了，之前写过两个博客，都是以VPS挂掉告终，无奈所选的VPS服务商都不靠谱，悲痛的损失了所有的日志以及私人Git仓库。这篇博文的最大作用，就是希望提醒看到它的人们，快备份你的数据吧。

## Rebuild my blog

百屈不挠的，我再次开了Blog，原因不明。总觉得如果日子过的混混沌沌的，连一点记忆或者值得记录、搜藏的东西都没有是一件多么恐怖的事情。

不过这次我决定在Github上开始我崭新的博客之旅，如果哪一天Github挂了的话……我……还是要继续写博客的，因为这次所有数据都在本地了。

其实，很早我就知道可以用*jekyll*写静态博客，发布到Github托管，并且有着很快的响应速度。但总觉得，万一以后写的东西多了，有很多的日志，就没法管理了，静态网站始终不是一个完全之策，就没有理会这个大家都说好的静态博客引擎。于是，就有了这人生中的第三篇开博大典。这个教训是否刚好印证了开发之大忌——过早优化是恶魔呢？

## 以后的日子

以后的日子，我将会在这里记录生活点滴，分享技术心得，吐吐槽神马的，欢迎大家拍砖引玉，没错，就是拍砖引玉……

## 感谢

现在，随便写点什么还是不写什么，说点什么还是不说什么，致谢词总是少不了。这里当然不能坏了规矩，感谢下面所有人，如果没有你们，就没有这个博客。

* [blog.leezhong.com](http://blog.leezhong.com/) 我还在搞PHP的时候看到的他的博客，也是从他那里知道搭建Blog还能如此方便。虽然联系也不多，但他总是能够回复我的问题，也是一位有抱负的有为青年，谢谢。
* [beiyuu.com](http://beiyuu.com/) 这个博客模板全部拜他所赐，是最近看到几个博客中最喜欢的，和我一样喜欢音乐还有吉他。谢谢。
* [redking1988](http://weibo.com/redking1988) 哥们儿，在我需要的时候帮助我，在他需要帮助的时候被我帮……

## EOF

本来是改结束了这篇，但是我确实忍不住，这个代码贴出来太漂亮了，就让代码来结尾吧！

bash:

	git clone git@github.com:siriuszhuang/siriuszhuang.github.com.git
	tail -f log/production.log | grep Completed

Ruby:

	class Sirius
	  class << self
	    def say(something)
	      puts something
	    end
	  end
	end
	Sirius.say "Good bye!"
