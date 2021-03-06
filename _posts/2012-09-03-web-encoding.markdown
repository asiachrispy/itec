---
layout: post
title: "关于繁体字乱码"
tag: "tec"
comment: true
published: true
date: 2012-09-03
---

短信内容中有繁体字，服务器端接受到的内容中繁体字部分均为乱码， 解决这个问题，经历了几个过程，

#### 1.首先,短信接口要求的是gb2312编码，所以所有使用短信接口的人，都使用了GB2312编码发送短信内容，
当短信内容包含繁体字时，GB2312编码没有问题，但是解码后却会出现乱码，测试代码如下：

![a](/tec/images/2012-09-03-web-n1.jpg)

这个原因很简单，GBK编码能同时表示繁体字和简体字，而GB2312只能表示简体字，所以GBK兼容GB2312字符集及其编码。
验证的代码如下：

![b](/tec/images/2012-09-03-web-n2.jpg)

好了，这一步是关键，我们只完成了第一步

#### 2.按照上面的规范修改代码后，重新测试，发现服务器端依然显示如上所示的乱码，我首先想到的是修改服务器端编码配置

2.1 查找ngnix的编码配置，没有发现，我们只是使用它的代理功能

2.2 查找resin的编码配置，默认都没有配置，添加配置并指定GBK，测试发现无效，依然显示如上乱码

**分析**：既然客户端的编码方式正确，而服务器端的配置以及修改，那么应该是正常显示才对，既然还是乱码，

那么只有一个可能，和RESIN的配置无关，如果无关，那么解码的过程是谁(哪个环节)处理的？

这个过程思考了很久，没有答案，我向王凯咨询可能出现的原因，存在的可能比较多，但可以确定和服务端配置没有关系（也验证了上面的说法），
其中包括显示终端是否支持繁体字体显示，如果不支持，那么我们看到的乱码也未必就一定是乱码，也有可能是协议头没有指定字符集的原因等，

我发现验证协议头的字符集是最快的方式，所以立即验证，代码如下：

![c](/tec/images/2012-09-03-web-n3.jpg)

测试，发现依然显示如上乱码，再次陷入迷雾之中！
此时，我不得不再次回头思考上面的问题，确定以上步骤没有出现错误。


#### 3.当我再次看到 resin的配置文件中的这段代码时，感觉似曾相识，立即想起web.xml配置文件
##### RESIN中的配置：   
	 <character-encoding>GBK</character-encoding>

##### web.xml文件中的配置：

![c](/tec/images/2012-09-03-web-n4.jpg)

<p class='red'>这才恍然大悟，同时祷告，这次一定要有效，否则，真的无路可走！</p>

修改配置，重启服务，测试发现，我的终端日志终于显示了正确的繁体字！~~~~欢呼

<p class="red">接着去掉头部的字符集设置，依然正确！~~~~再次欢呼</p>

##4.总结
最终确定，处理繁体字乱码问题，只需要解决2个问题：

1. 客户端编码使用GBK
2. web.xml配置，处理请求编码使用GBK

**解决问题的过程远比上面描述的还要复杂，这期间会受到个人思维习惯，经验，环境，情绪影响，
我就发现自己严重受到经验的影响，一直以为是服务端配置的问题，
然后不断查找NGINX,RESIN如何配置编码的问题.**


好了，之所以写上面这段文字，确实是因为体会比较深刻，也希望能给大家带来一点收获！
