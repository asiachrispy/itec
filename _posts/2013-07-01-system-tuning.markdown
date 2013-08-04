---
layout: post
title: "系统调优"
tag: "tec"
comment: true
published: true
date: 2013-07-01
---

系统调优涉可以从以下几个方面考虑：    
1、服务架构是否合理？服务部署结构是否合理？   
2、服务器以及各个相关服务的配置参数设置是否合理？     
3、服务使用的开源软件配置是否最优？    
4、代码实现是否合理（字符串操作，异步处理）和安全（多线程，资源释放）？算法是否最优？表结构和SQL语句是否合理和高效？    

以上各个方面可以参考酷壳的[性能调优攻略](http://coolshell.cn/articles/7490.html) 以及[Web系统性能调优吐血总结分享](http://blog.csdn.net/binyao02123202/article/details/6576985),读完这2篇文章，相信你对性能调优一定会有新的认识，其实在工作中你很多时候也是这么做的，但是作为专业的码农，还是要掌握系统而专业的技能，才能把工作完成的更出色。好吧，如果你还没有在工作中实践过，现在可以小试牛刀了。    

我本人在2010年的时候做过分布式文件系统平台的工作，参与各种测试工作，包括性能测试，以及优化的工作，使用过junit jmeter JProfiler RESTClient等测试工具，当然还有vmstat, sar, iostat, free,top, tcpdump,jps,jstat,jinfo,jstack,jmap，如何使用请参考手册，我也不全记得，但一定要知道工具箱有什么可以使用。