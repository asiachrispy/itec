---
layout: post
title: "第一节：让我们瞧瞧ImageMagic能做什么"
tag: "tec"
comment: true
published: true
date: 2012-09-23
---

前面已经介绍如何安装ImageMagic，那么她到底能为你做什么，能给你带来怎样的便捷和高效，我们一起来探索吧。

### convert    
  这个是ImageMagic的最常用的工具之一，它主要用来对图像进行格式的转化，同时还可以做缩放、剪切、模糊、反转等操作。   
  
#### 转换格式    
比如把 bar.jpg 转化为 bar.png

        convert bar.jpg bar.png
#### 缩放
现在为一张普通大小的图片做一个缩略图，采用数值的方式

    convert -resize 100x100 bar.jpg thumb-one.jpg

当然，你也可以用百分比的方式， 对bar,jpg的长和高各缩小50%的大小

    convert -resize 50%x50% bar.jpg thumb-two.jpg
  
#### 加边框
用 -mattecolor 参数来加边框，"#fff000"是边框的颜色，-frame 100x100表示边框的大小

     convert -mattecolor "#fff000" -frame 60x60 bar.jpg bar-three.jpg
#### 加文字
在距离图片的左上角20x50的位置，用绿色的字写下'chrispy'
    
    convert -fill green -pointsize 30 -draw 'text 20,50 "chrispy"' bar.jpg bar-four.jpg
#### 油画效果
看看如何把一张普通的图片变成一张油画，效果非常的逼真

    convert -paint 4 bar.jpg bar-five.jpg
####漩涡
以图片的中心作为参照，把图片扭转，形成漩涡的效果
    
    convert -swirl 67 bar.jpg bar-six.jpg
    
### 扩展    
  ImageMagic提供的工具还有很多，下面列举几种
   
  * import       
    是一个用于屏幕截图的组件     
  * display      
    图像处理软件    
  * composite      
    根据一个图片或多个图片组合生成图片    
  * animate    
    利用X server显示动画图片   
  * identify     
    描述一个或较多图像文件的格式和特性       
  * mogrify    
    按规定尺寸制作一个图像，模糊，裁剪，抖动     
