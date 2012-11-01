---
layout: post
title: "git 恢复删除的文件"
tag: "tec"
comment: true
published: true
date: 2012-09-01

---

今天不小心误删了_includes/about-me.html文件，需要恢复删除的文件，由于也是第一次使用恢复的功能，所以整理下来。

### 恢复步骤

#### 1. 通过git log -g命令找到需要恢复的信息commit值，可以通过提交的时间和日期来辨别.
#### 2. 通过git checkout [commit-value] 要恢复的文件路径


###下面看看我恢复的过程
####1. 找到我需要的commit信息，主要是通过时间来看判断的

```
 update
 commit dcaf8a77b890d417ff395a51b7f5283cada4408a  

 Reflog: HEAD@{33} (= <=>)

 Reflog message: commit: update
 
 Author: = <=>
  
 Date:   Sat Sep 1 15:45:50 2012 +0800
```

#### 2. 通过下面的命令我就找回了需要的文件   
git checkout dcaf8a77b890d417ff395a51b7f5283cada4408a _includes/about-me.html 


