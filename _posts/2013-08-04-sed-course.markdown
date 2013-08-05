---
layout: post
title: "sed 入门到高阶"
tag: "tec"
comment: true
published: true
date: 2013-08-04
---
最近在温习一些知识，同时记录学习的笔记，并整理成文章，希望大家喜欢。另外推荐大家阅读下我转载的一篇外文[[thoughts from the red planet]](http://nathanmarz.com/blog/you-should-blog-even-if-you-have-no-readers.html).当然你也可以在我的博客里文章里找到。在后面的我会完成awk的使用文章，敬请期待吧！

--------------
假设我们已经创建了一个测试文件，文件内容如下：   
```
# # cat chris.txt              
1-Hello,asia-chris!                                                                                                 
2-if you are asia-chris,please response me!   
3-   
4-this is asia chris,not chris fire!
5-but this is chris fire, not asia chris!   
```

**注意：** 下面的sed并没有对文件的内容改变，只是把处理过后的内容输出，如果你要写回文件，你可以使用重定向，或者使用-i命令，本文为了测试，不进行编辑后的保存，但是你要知道-i将保存修改后的内容。

另外，为了更好的学习，你可以参考酷壳的[sed 简明教程](http://coolshell.cn/articles/9104.html)，很详细，值得大家阅读。

### 1> 删除命令 d

1. 默认不指定行地址将会删除所有行：
> # sed 'd' chris.txt
2. 指定行地址则删除匹配的行，如删除第一行：
> # sed '1d' chris.txt
2-if you are asia-chris,please response me!   
3-   
4-this is asia chris,not chris fire!   
5-but this is chris fire, not asia chris!   

3. 或者删除最后一行，$符号在这里表示最后一行，这点要下正则表达式中的含义区别开来：
> # sed '$d' chris.txt
1-Hello,asia-chris!     
2-if you are asia-chris,please response me!   
3-   
4-this is asia chris,not chris fire!   

4. 前面都是以行号来指定地址，也可以通过正则表达式来指定地址，如删除包含me的行：
> # sed '/me/d' chris.txt
1-Hello,asia-chris!     
3-   
4-this is asia chris,not chris fire!   
5-but this is chris fire, not asia chris!   

5. 通过指定行地址对可以删除该范围内的所有行，例如删除第2行到最后一行：
># sed '2,$d' chris.txt
1-Hello,asia-chris!  

### 2> 替换命令 s
1. 将this替换为it
># sed -e 's/me/you/' chris.txt
1-Hello,asia-chris!   
2-if you are asia-chris,please response me!   
3-   
4-it is asia chris,not chris fire!   
5-but it is chris fire, not asia chris!   

2. 上面-e参数是可选的，这个参数只是在命令行中同时指定多个操作指令时才需要用到，如：
># sed -e 's/me/you/' -e 's/but/yet/' chris.txt
1-Hello,asia-chris!      
2-if you are asia-chris,please response you!       
3-   
4-this is asia chris,not chris fire!   
5-yet this is chris fire, not asia chris!   

3. 即使在多个操作指令的情况下，-e参数也不是必需的，我一般不会加-e参数，操作指令之间可以用逗号分隔
比如上面的例子可以换成下面的写法:
># sed 's/me/you/;s/but/yet/' chris.txt

4. 上面的替换我们没有指定最后的参数，则默认替换第一次匹配：
># echo "c1 c2 c3 c4" | sed 's/ /;/'
c1;c2 c3 c4

5. 如果最后包含g，则表示全局匹配：
># echo "c1 c2 c3 c4" | sed 's/ /;/g'
c1;c2;c3;c4

6. 如果最后明确指定替换第n次的匹配，例如n=2：
># echo "c1 c2 c3 c4" | sed 's/ /;/2'
c1 c2;c3 c4

3> 插入命令 i(插入命令是指匹配的行前面插入文本)，注意我输入的过程
>  # sed '1i \   
     d--------  
     ' chris.txt    
d--------     
1-Hello,asia-chris!      
2-if you are asia-chris,please response me!   
3-   
4-this is asia chris,not chris fire!   
5-but this is chris fire, not asia chris!   

4> 追加命令 a(追加命令是指在匹配的行后面插入文本),
>  # sed '5a \   
     d--------  
     ' chris.txt     
1-Hello,asia-chris!   
2-if you are asia-chris,please response me!    
3-   
4-this is asia chris,not chris fire!    
5-but this is chris fire, not asia chris!   
d----------    

5> 打印命令
> # sed '1p' chris.txt 
1-Hello,asia-chris!   
1-Hello,asia-chris!   
2-if you are asia-chris,please response me!   
3-   
4-this is asia chris,not chris fire!   
5-but this is chris fire, not asia chris!   

大家有没有发现除了第一行重复打印之外，其它行也打印了，似乎不是我们想看到的，
造成这个问题的原因是，sed命令完成所有命令之后默认会将当前行打印输出，所以就有了上面的结果。解决方法是在使用sed命令是指定-n参数，该参数会抑制sed默认的输出：
> # sed -n '1p' chris.txt 
1-Hello,asia-chris!

6> 显示文本行号 = 
> # sed '=' chris.txt
1    
1-Hello,asia-chris!    
2    
2-if you are asia-chris,please response me!    
3    
3-    
4        
4-this is asia chris,not chris fire!    
5    
5-but this is chris fire, not asia chris!    

7> 转换命令 y
> # echo "hello, world" | sed 'y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/'
HELLO, WORLD

使用tr命令来转换成大写：
> # echo "hello, world" | tr a-z A-Z 
HELLO, WORLD

8> 退出命令 q;当sed读取到匹配的行之后即退出，不会再读入新的行，并且将当前模式空间的内容输出到屏幕。    
1. 打印前3行的信息
> # sed '3q' chris.txt 
1-Hello,asia-chris!    
2-if you are asia-chris,please response me!    
3-    

2. 打印前3行的信息可以使用P
> # sed '3p' chris.txt 
1-Hello,asia-chris!    
2-if you are asia-chris,please response me!    
3-    

但是对于大文件来说，前者比后者效率更高，因为前者读取到第N行之后就退出了。后者虽然打印了前N行，但是后续的行还是要继续读入，只不会不作处理。

就先介绍到这里吧，希望没有花费你太多时间阅读！

写于 北京.2013-8-4 晚11点

