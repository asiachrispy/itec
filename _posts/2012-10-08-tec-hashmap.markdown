---
layout: post
title: "HashMap &  LinkedHashMap & HashTable"
tag: "English"
comment: true
published: true
date: 2012-10-08
---

## HashMap 特性

#### 1. 使用数组存储元素   
既然使用数组存放元素，就得有初始化大小值，另外，使用数组另外一个问题就是扩容问题，什么时机进行扩容，扩展多少。

**代码如下**  

```
存储数据的数组
transient Entry[] table; 
默认容量
static final int DEFAULT_INITIAL_CAPACITY = 16;
最大容量
static final int MAXIMUM_CAPACITY = 1 << 30;
默认加载因子，加载因子是一个比例，当HashMap的数据大小>=容量*加载因子时，HashMap会将容量扩容
static final float DEFAULT_LOAD_FACTOR = 0.75f;
当实际数据大小超过threshold时，HashMap会将容量扩容，threshold＝容量*加载因子
int threshold;
加载因子
final float loadFactor;
```

如果初始化一个new HashMap(8)；那么HashMap的初始容量是16，而不是8，想想为什么吧。
**代码如下**  

```
     public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);

        // Find a power of 2 >= initialCapacity
        int capacity = 1;
        while (capacity < initialCapacity)
            capacity <<= 1; //这里能说明一切

        this.loadFactor = loadFactor;
        threshold = (int)(capacity * loadFactor);// 真正的结果在这
        table = new Entry[capacity];
        init();
    }

    // 自定义初始化大小，
    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
    
    // 默认的构造在这里
    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        threshold = (int)(DEFAULT_INITIAL_CAPACITY * DEFAULT_LOAD_FACTOR);
        table = new Entry[DEFAULT_INITIAL_CAPACITY];
        init();
    }

```
#### 2. Entry 为单向链表
Entry 包含key,value,next(指向下一个Entry),当然还有hash值
为什么是个单向链表？
因为使用散列(hash)算法存在碰撞的问题，就是不同的key可能被计算出想他的hash值，所以链表就将这些发生碰撞的元素放在这个链表里了。

**代码如下**  

```
static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        final int hash;

        /**
         * Creates new entry.
         */
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        }
}
```


####  3.  元素插入
当插入一个Key-Value时，分为一下几个过程：  
    1> 取Key的hashCode值h,然后对h进行hash计算，并返回hash;   
    2> 使用hash和表长度计算数组的下标i;    
    3> 查找数组中i下标的值e (是一个Entry);   
       （由于是链表结构，所以需要循环遍历）比较eEntry的hash和上面计算的hash值，并比较key;   
        如果存在都相等的元素，则将新的value覆盖旧的value，并返回旧的value;  
    	 否则，插入新的值到Entry数组中；
   
**代码如下**  

 ```
 public V put(K key, V value) {
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key.hashCode());
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```
另外，我们也看到，HashMap同样可以存储null值，而且就存储在talbe[0]链表的表头.
         
```    
 private V putForNullKey(V value) {
        for (Entry<K,V> e = table[0]; e != null; e = e.next) {
            if (e.key == null) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
        modCount++;
        addEntry(0, null, value, 0);
        return null;
 }
    
 ```
-----
    
  LinkedHashMap 
  1. 使用双向链表存储
    
 -----
   HashTable 是线程安全的，实现基本和HashMap一样，就是一些方法上加上了synchronized;初始容量分别是11和16
