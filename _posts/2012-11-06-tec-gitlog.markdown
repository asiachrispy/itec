---
layout: post
title: "Git显示漂亮日志的小技巧"
tag: "tec"
comment: true
published: true
date: 2012-11-06
---
原文：http://garmoncheg.blogspot.com/2012/06/pretty-git-log.html （墙）

Git的传统log如下所示，你喜欢吗？
![](http://coolshell.cn//wp-content/uploads/2012/06/git.log_.01.png)


看看下面这个你喜不喜欢？（点击图片看大图）

![](http://coolshell.cn//wp-content/uploads/2012/06/git.log_.02.png)


要做到这样，命令行如下：

>git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --

这样有点长了，我们可以这样：

>git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"

然后，我们就可以使用这样的短命令了：

>git lg

如果你想看看git log –pretty=format的参数，你可以看看[这篇文章](http://git-scm.com/book/zh/Git-基础-查看提交历史)。

本文摘自[酷壳 – CoolShell.cn](http://coolshell.cn/articles/7755.html)