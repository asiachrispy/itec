---
layout: post
title: "远程过程调用和远程方法调用的区别"
tag: "tec"
comment: true
published: true
date: 2012-09-27

---

####这里只是介绍这2个概念以及差别，翻译还有不到之处，忘大家指点。

------
##译文
####远程过程调用和远程方法调用的区别
RPC(远程过程调用) 和RMI(远程方面调用)是2个机制，允许用户调用另一个不同于当前用户正在使用的进程。这两者的主要区别是被使用的方法或者规范。RMI使用面向对象的规范，它需要知道对象和它需要调用的对象的方法。相反，RPC不是面向对象也不处理对象。然而，它调用特定的已经建立的子程序。

RPC是一个相对比较老的基于C语言的协议，因此继承了它的规范。使用RPC你得到一个看起来特别像本地调用的子程序。RPC从本地传递到调用到远程的计算机的复杂性。RMI也做同样的事情，处理从本地传递调用到远程计算机的的复杂性。RMI是使用JAVA和JAVA虚拟机来开发的。所以它是专为JAVA应用程序来进行远程计算机方法调用而使用的。

最后，RMI和RPC只是完成同一件事的两种方式。它门都满足你所使用的语言和规范。在这两者之间使用面向对象的RMI是更好的方式，尤其是在大程序中，它提供更加清晰的代码以便一旦出现错误的时候能够跟踪。使用RPC依然是被广泛接受的，尤其是在任何一个非正式的远程协议都不可选的情况下。

总结：

* RMI是面向对象的而RPC不是
* RPC是基于C语言的而RMI只支持JAVA
* RMI是调用方法而RPC是调用功能
* RPC已经过时了而RMI是有前景的

------
##原文
查看连接在[这里](http://www.differencebetween.net/technology/protocols-formats/difference-between-rpc-and-rmi/)
###Difference Between RPC and RMI
• Categorized under [Protocols & Formats](http://www.differencebetween.net/category/technology/protocols-formats/) | [Difference Between RPC and RMI](http://www.differencebetween.net/wp-content/uploads/2010/15/general-pd19.jpg)

![](http://www.differencebetween.net/wp-content/uploads/2010/15/general-pd19.jpg)
###RPC vs RMI
RPC (Remote Procedure Call) and RMI (Remote Method Invocation) are two mechanisms that allow the user to invoke or call processes that will run on a different computer from the one the user is using. The main difference between the two is the approach or paradigm used. RMI uses an object oriented paradigm where the user needs to know the object and the method of the object he needs to invoke. In comparison, RPC isn’t object oriented and doesn’t deal with objects. Rather, it calls specific subroutines that are already established.

RPC is a relatively old protocol that is based on the C language, thus inheriting its paradigm. With RPC, you get a procedure call that looks pretty much like a local call. RPC handles the complexities involved with passing the call from the local to the remote computer. RMI does the very same thing; handling the complexities of passing along the invocation from the local to the remote computer. But instead of passing a procedural call, RMI passes a reference to the object and the method that is being called. RMI was developed by Java and uses its virtual machine. Its use is therefore exclusive to Java applications for calling methods on remote computers.

In the end, RPC and RMI are just two means of achieving the same exact thing. It all comes down to what language you are using and which paradigm you are used to. Using the object oriented RMI is the better approach between the two, especially with larger programs as it provides a cleaner code that is easier to track down once something goes wrong. Use of RPC is still widely accepted, especially when any of the alternative remote procedural protocols are not an option.

Summary:

1.RMI is object oriented while RPC isn’t
2.RPC is C bases while RMI is Java only
3.RMI invokes methods while RPC invokes functions
4.RPC is antiquated while RMI is the future
