---
layout: post
title: "type引发的问题"
tag: "tec"
comment: true
published: true
date: 2012-08-31
---

这周在做校园招聘项目，前端使用freemark模板引擎，同时使用struts2框架，项目中有一个搜索的请求，如下：
http://xxx/campus/search?type=3&keyWord=中兴
keyWord是查询的关键字，这里的type就是本文讨论的type，看上去似乎没有什么问题，不要着急，
下面我来说说这个type到底出现什么问题

这个URL 请求CampusSearchAction 类, 这个类最终继承了struts2 的ActionSupport ，
type 和keyWord是CampusSearchAction 的私有String成员。
在请求结束之前，这2个值均被返回给前端freemark文件，
在freemark文件中调用${type}，发现无论CampusSearchAction 给type 返回什么值，都只得到 1
（现在你会觉得奇怪了吧，这也让我和同事李涛抓狂了许久）

好了，我们来思考下这个问题:
我们大家首先能想到的是是否存在另外一个type
(好吧，我不得不说，你的确很聪明，因为这让我很激动）
 因为换了另外一个名字ttype,发现值正常；
 但是我们并没有解决问题，我们只是巧妙的避免了问题的发生，这个也不是让我激动的原因，
 真正让我激动的是，页面中依然能访问到type，且值为1，这个才是我想要的；

 那么另一个type会在哪里被赋值呢？
 大家首先想到的是CampusSearchAction的接口或者父类
 不过让你失望了，它们没有对type赋值；

 也许有人想到freemark模板本身是否有使用这个type？
 又让你失望了，我google了下，没有找到类似的问题，因为如果有使用的话，那freemark就太二了，
 不是吗，因为如果让你设计，估计你也不会这么设计吧，你知道的....

 到这里恐怕你已经有些耐不住性子了，我也暂时放下了问题，让自己放松下。

 但我相信自己的代码中没有覆盖这个type,别人的代码我不确定，
 此时我必须怀疑别人的代码，不过已经排除了后端的代码和freemark模板本身，
 最后只剩下ftl文件本身，我们看看ftl文件的结构，下面是每个ftl文件的模板结构，
 这些代码我都不确定是否存在type,
<a class="imglink left" href="/tec/images/20120831-ftl-1.png"><img src="/tec/images/20120831-ftl-1.png" /></a>


 好吧，下面我们就来逐个看看这些文件吧：

 哈哈，果然不出所料，打开第一个文件就发现
<a class="imglink left" href="/tec/images/20120831-ftl-2.png"><img src="/tec/images/20120831-ftl-2.png" /></a>

<br/>
<a class="imglink left" href="/tec/images/20120831-ftl-3.png"><img src="/tec/images/20120831-ftl-3.png" /></a>


 不论我们爱它，还是不爱它，type就在那，查看svn日志（下图）发现，这个type在2011-11-17号开始到现在一直都在这，
 直到今天才被发现，且是个无效的数据，很难相信，这个坑（垃圾代码）竟然隐藏了近1年。
 问题在于我们调用type在它的内部，这就是type被覆盖的真正原因。

<a class="imglink left" href="/tec/images/20120831-ftl-4.png"><img src="/tec/images/20120831-ftl-4.png" /></a>


 好了，再检查下其它文件，没有发现，最后再试试type是否可用，一切如你所愿。


<h3> 总结：</h3>
 这不属于技术性问题，即使你解决了这样的问题，也没有什么了不起，你只不过是掉入别人有意或无意设下的坑里，一开始你并不知道，还庆幸自己遇到了别人都没有遇到过的难题，等你筋疲力尽从坑里爬出来时，才发现自己是多么的愚蠢，不过你还算是个有良心的码农，挥挥手把坑填平，然后继续上路，你也不知道前面的路还有多少坑，但至少自己不要挖坑，如果有幸发现了坑，那就填了吧。
 1.避免问题，不等于解决问题，就像你绕开了坑，坑其实还在那里，抓住问题的本质，才是最后的胜利
 2.大胆的怀疑别人的代码吧，当然在怀疑别人之前，先怀疑自己

