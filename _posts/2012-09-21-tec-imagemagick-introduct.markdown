---
layout: post
title: "第一节：如何在你的Mac OS X 安装ImageMagick"
tag: "tec"
comment: true
published: true
date: 2012-09-21
---
### Max OS X 上的ImageMagick安装程序

下面是我翻译的一段文章，提供英文原文，翻译仅供参考，有不妥之处忘指点。

##### 为什么要用它
ImageMagick其实很早之前就已经在Windows和Linux使用过，在项目中主要是用它来处理用户的头像，用户在注册一个网站时，网站基本都有要求用户上传个人头像，用户选择的个人头像图片有大有小，为了剪裁成统一大小的图片，或者生成多张不同大小的图片时，现在大多网站都使用ImageMagick来处理，它提供多种语言的API供客户端使用。

这次要在自己的Mac OS X 安装ImageMagick，完全是为了给本站的图片剪裁提供的工具，便捷更高效，另外一个原因是因为我是一个热爱使用CMD工作的人。

----------
##### 翻译正文
为了简单处理ImageMagick在Mac OS X的安装，[Cactuslab](http://www.cactuslab.com/)已经将它和安装包放置在一起。安装程序将它放置在/opt/ImageMagick(之前的Mountain Lion版本将它放置在/usr/local/ImageMagick)，并且创建了一个/etc/paths.d/，并将它加入到你的PATH里。

如果你在使用这个安装包的过程中有任务问题，请联系我们。如有其它问题和疑问请访问ImageMagick.org。

##### 卸载

为了卸载ImageMagick请打开一个终端程序窗口，并且输入一下2个命令。你将被提醒输入你的管理员密码。  
	
	1. sudo rm /etc/paths.d/ImageMagick   
	2. sudo rm -r /opt/ImageMagick 

-------------
英文原文来自：     <http://cactuslab.com/imagemagick/>

To simplify the process of installing ImageMagick on Mac OS X, Cactuslab has put together an installer package. The installer puts ImageMagick into /opt/ImageMagick (or /usr/local/ImageMagickbefore the Mountain Lion version) and adds it to your PATH by creating an entry in /etc/paths.d/.
Please contact us if you have any problems with this installer. For other problems or questions please visit ImageMagick.org

Uninstall

To uninstall ImageMagick, please open a Terminal.app window and enter the following two commands. You will be prompted for your administrator password.
sudo rm /etc/paths.d/ImageMagick
sudo rm -r /opt/ImageMagick

--------
#### 扩展
* Cactuslab 是什么？  
	[Cactuslab](http://www.cactuslab.com/)是由卡尔·冯·Randow和马修·布坎南创立的一个多媒体工作室,前身是新媒体公司WebMedia。   
	我们是一个web标准同时提供保证质量的iOS应用程序设计和开发工作室,关注人类并不断关注细节。     
	
> We’re a web standards and iOS application design & development studio with a commitment to quality, a focus on humans and a relentless eye for detail.   
　　

* ImageMagick 安装程序包  
	<http://cactuslab.com/imagemagick/>

* ImageMagick 中文站   
	<http://www.imagemagick.com.cn/index.html>
