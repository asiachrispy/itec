---
layout: post
title: "ArrayBlockingQueue"
tag: "tec"
comment: true
published: true
date: 2012-10-12
---

**public class ArrayBlockingQueue<E>    
                            extends AbstractQueue<E>    
                                    implements BlockingQueue<E>, Serializable**   

### 官方英文文档描述
A bounded blocking queue backed by an array. This queue orders elements FIFO (first-in-first-out). The head of the queue is that element that has been on the queue the longest time. The tail of the queue is that element that has been on the queue the shortest time. New elements are inserted at the tail of the queue, and the queue retrieval operations obtain elements at the head of the queue.

This is a classic "bounded buffer", in which a fixed-sized array holds elements inserted by producers and extracted by consumers. Once created, the capacity cannot be increased. Attempts to put an element to a full queue will result in the put operation blocking; attempts to retrieve an element from an empty queue will similarly block.

This class supports an optional fairness policy for ordering waiting producer and consumer threads. By default, this ordering is not guaranteed. However, a queue constructed with fairness set to true grants threads access in FIFO order. Fairness generally decreases throughput but reduces variability and avoids starvation.

This class and its iterator implement all of the optional methods of the Collection and Iterator interfaces.

This class is a member of the Java Collections Framework.

### 官方中文文档描述
一个由数组支持的有界阻塞队列。此队列按 FIFO（先进先出）原则对元素进行排序。队列的头部是在队列中存在时间最长的元素。队列的尾部是在队列中存在时间最短的元素。新元素插入到队列的尾部，队列获取操作则是从队列头部开始获得元素。

这是一个典型的“有界缓存区”，固定大小的数组在其中保持生产者插入的元素和使用者提取的元素。一旦创建了这样的缓存区，就不能再增加其容量。试图向已满队列中放入元素会导致操作受阻塞；试图从空队列中提取元素将导致类似阻塞。

此类支持对等待的生产者线程和使用者线程进行排序的可选公平策略。默认情况下，不保证是这种排序。然而，通过将公平性 (fairness) 设置为 true 而构造的队列允许按照 FIFO 顺序访问线程。公平性通常会降低吞吐量，但也减少了可变性和避免了“不平衡性”。

此类及其迭代器实现了 Collection 和 Iterator 接口的所有可选 方法。

此类是 Java Collections Framework 的成员。