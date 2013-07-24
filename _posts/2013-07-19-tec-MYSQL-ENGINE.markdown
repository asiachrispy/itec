---
layout: post
title: "MySql Engine   MyISAM & InnoDB"
tag: "tec"
comment: true
published: true
date: 2013-07-21
---
很多人对MySql 的 MyISAM & InnoDB 引擎并不陌生，而且大多都能找到一些关键的区分点，比如索引、事务安全、外键、行锁等，但这里并不打算详细讨论这些，本文要从MySql的底层实现细节来看看各自的不同。

**MyISAM**
    
    1.1引擎使用B+Tree作为索引结构，叶节点的data域存放的是数据记录的地址;
    1.2 MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录;
    1.3 在MyISAM中，主索引和辅助索引（Secondary key）在结构上没有任何区别，只是主索引要求key是唯一的，而辅助索引的key可以重复;

- MyISAM的索引方式也叫做“非聚集”的，之所以这么称呼是为了与InnoDB的聚集索引区分。
- 聚集索引这种实现方式使得按主键的搜索十分高效，但是辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。

**InnoDB**

    2.1 InnoDB也使用B+Tree作为索引结构,InnoDB的数据文件本身就是索引文件;
    2.2 表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录;
    2.3 与MyISAM索引的不同是InnoDB的辅助索引data域存储相应记录主键的值而不是地址;
    
    
 ---------------------
    
#### MyISAM

 * Mysql默认表类型，它是基于传统的ISAM类型，ISAM是Indexed Sequential Access Method (有索引的顺序访问方法) 的缩写，它是存储记录和文件的标准方法。
 
 * 非事务安全，不支持外键
 * 如果执行大量的select，insert MyISAM比较适合。

#### InnoDB

* Innodb最初是由innobase Oy公司开发，2006年5月由oracle公司并购，目前innodb采用双授权，一个是GPL授权，一个是商业授权。

* 支持事务安全的引擎，支持外键、行锁、事务是他的最大特点
* 如果有大量的update和insert，建议使用InnoDB,特别是针对多个并发和QPS较高的情况。


