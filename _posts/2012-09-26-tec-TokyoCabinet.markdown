---
layout: post
title: "关于Tokyo Cabinet你知道多少"
tag: "tec"
comment: true
published: true
date: 2012-09-26
---

最近在研究队列，了解了一些开源的队列，包括[httpSQS](http://blog.s135.com/httpsqs_1_7/), [RabbitMQ](http://www.rabbitmq.com/tutorials/tutorial-one-java.html),[ActiveMQ](http://activemq.apache.org/)

关于Tokyo Cabinet你知道多少,下面我翻译的一段文章，原文和翻译均贴出。
有些翻译不是很到位。有待提高！

----------
## 译文如下

顺便提下，你了解Kyoto Cabinet吗？实际上，它是一个比Tokyo Cabinet 还要强大和方便的开发包。经过这样长的时间，Kyoto Cabinet 在很多方面都超过Tokyo Cabinet。我强烈建议你使用Kyoto Cabinet 。

####概述
Tokyo Cabinet 是一个管理数据库的常规开发包。数据库是一个包含记录的简单数据文件，每个记录是一个key-value对，每个键和值都是可变长度的字节序列。没有数据表和数据类型的概念。记录都是使用hash table, B+ tree或者固定长度的数组组织的。

Tokyo Cabinet是基于以下目的作为GDBM and QDBM的继承者而开发的。它们已经被实现并且Tokyo Cabinet正在替代传统的DBM产品。
      
* 提高了空间效率：更小的数据库文件   
* 提高了时间效率：更快的处理速度     
* 提供了并行：在多线程环境下具有更高的性能     
* 提高了可用性：简单化的API     
* 提高了健壮性：即使在毁灭性的情况下数据库文件也不会被损坏    
* 支持64位操作系统的架构：庞大的内存空间和数据库文件是可用的    

Tokyo Cabinet是使用C语言编写的，并且提供 C, Perl, Ruby, Java, 和 Lua的API 。Tokyo Cabinet在遵循C99 和 POSIX协议的API的平台是可用的。Tokyo Cabinet是一个免费的软件许可证，遵循GNU通用公共许可证。

-----------
## 原文如下
原文链接[在这](http://fallabs.com/tokyocabinet/)

### Tokyo Cabinet: a modern implementation of DBM
```
Copyright (C) 2006-2011 FAL Labs
Last Update: Thu, 05 Aug 2010 15:05:11 +0900
[English/Japanese]
```

BTW, do you know Kyoto Cabinet? Actually, it is more powerful and convenient library than Tokyo Cabinet. At this distance of time, Kyoto Cabinet surpasses Tokyo Cabinet in every aspects. I strongly recommend you to use Kyoto Cabinet.

Overview

Tokyo Cabinet is a library of routines for managing a database. The database is a simple data file containing records, each is a pair of a key and a value. Every key and value is serial bytes with variable length. Both binary data and character string can be used as a key and a value. There is neither concept of data tables nor data types. Records are organized in hash table, B+ tree, or fixed-length array.

Tokyo Cabinet is developed as the successor of GDBM and QDBM on the following purposes. They are achieved and Tokyo Cabinet replaces conventional DBM products.

improves space efficiency : smaller size of database file.
improves time efficiency : faster processing speed.
improves parallelism : higher performance in multi-thread environment.
improves usability : simplified API.
improves robustness : database file is not corrupted even under catastrophic situation.
supports 64-bit architecture : enormous memory space and database file are available.
Tokyo Cabinet is written in the C language, and provided as API of C, Perl, Ruby, Java, and Lua. Tokyo Cabinet is available on platforms which have API conforming to C99 and POSIX. Tokyo Cabinet is a free software licensed under the GNU Lesser General Public License.

