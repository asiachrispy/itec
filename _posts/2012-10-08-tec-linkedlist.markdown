---
layout: post
title: "ArrayList & LinkedList知多少"
tag: "tec"
comment: true
published: true
date: 2012-10-09
---

## LinkedList 特性

对于LinkedList我认为只要了解以下3个特性就足够了。

#### 1.  使用双向链表存储元素  
 链表中的一个节点包括指向上一个节点的指针和指向下一个节点的指针，以及数据本身。从物理空间上来看，`数据的存储地址可能是不连续的`。

**节点的代码如下：**
   
 ```
private static class Entry<E> {
	E element;
	Entry<E> next;
	Entry<E> previous;

	Entry(E element, Entry<E> next, Entry<E> previous) {
	    this.element = element;
	    this.next = next;
	    this.previous = previous;
	}
}
```

#### 2.   从链表头部插入元素
这样就保证了插入的元素顺序，所以如果你想按照插入的元素顺序访问这些元素，那么你可以考虑是否使用它，因为它`严格按照放入的顺序存储`。

**插入元素的代码如下：**

```
public boolean add(E e) {
	addBefore(e, header);
      return true;
}

…

 private Entry<E> addBefore(E e, Entry<E> entry) {
	Entry<E> newEntry = new Entry<E>(e, entry, entry.previous);
	newEntry.previous.next = newEntry;
	newEntry.next.previous = newEntry;
	size++;
	modCount++;
	return newEntry;
    }

```

#### 3.   删除元素时需要顺序遍历链表查找元素
从表头顺序访问到表尾结束，就像线性查找，所以时间复杂度为O(n)，查找到之后再删除节点，并调整前后节点的指针。从代码当中也可以看到，`它允许存放null值`。

**删除元素的代码如下：**

```
// 对外提供的接口
public boolean remove(Object o) {
        if (o==null) {
            for (Entry<E> e = header.next; e != header; e = e.next) {
                if (e.element==null) {
                    remove(e);
                    return true;
                }
            }
        } else {
            for (Entry<E> e = header.next; e != header; e = e.next) {
                if (o.equals(e.element)) {
                    remove(e);
                    return true;
                }
            }
        }
        return false;
}
    
    ….
// 内部实现接口
private E remove(Entry<E> e) {
    if (e == header)
	    throw new NoSuchElementException();

    E result = e.element;
    e.previous.next = e.next;
    e.next.previous = e.previous;
    e.next = e.previous = null;
    e.element = null;
    size--;
    modCount++;
      
    return result;
}
    
```

-----------
## ArrayList 特性

对于ArrayList我认为只要了解以下3个特性就足够了。

#### 1.  使用对象数组存储元素  
 从代码中可以看出，默认初始化数组空间大小为10，当然可以自己指定初始大小。从物理空间来看，`存储的数据地址必然是连续的地址空间`。

**构造的代码如下：**

```
 private transient Object[] elementData;
 
 public ArrayList() {
	this(10);
 }
 
 public ArrayList(int initialCapacity) {
    super();
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
    this.elementData = new Object[initialCapacity];
 }
 
```

#### 2.   插入元素时需要检查是否需要扩展空间
插入元素时，计算当前空间是否足够，不够则需要扩展存储空间，并将当前数组拷贝到新的数组空间。所以能设置程序代码能预见的空间大小，`尽量避免数组空间的扩展发生`。

**插入的代码如下：**

```
public boolean add(E e) {
	ensureCapacity(size + 1);  // Increments modCount!!
	elementData[size++] = e;
	return true;
}
    
….

public void ensureCapacity(int minCapacity) {
	modCount++;
	int oldCapacity = elementData.length;
	if (minCapacity > oldCapacity) {
	    Object oldData[] = elementData;
	    int newCapacity = (oldCapacity * 3)/2 + 1;
	    if (newCapacity < minCapacity)
		    newCapacity = minCapacity;
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
}
```

#### 3.  删除元素也需要进行数据的拷贝
一旦有数据删除，则需要把被删除的元素的下一个元素到最后的元素均往前移动一个单位。另外，我们可以看到，它同样`允许存放null值`。

```
public boolean remove(Object o) {
	if (o == null) {
            for (int index = 0; index < size; index++)
		if (elementData[index] == null) {
		    fastRemove(index);
		    return true;
		}
	} else {
	    for (int index = 0; index < size; index++)
		if (o.equals(elementData[index])) {
		    fastRemove(index);
		    return true;
		}
        }
	return false;
}
    
private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // Let gc do its work
}
    ….
    
public E remove(int index) {
	RangeCheck(index);

	modCount++;
	E oldValue = (E) elementData[index];

	int numMoved = size - index - 1;
	if (numMoved > 0)
	    System.arraycopy(elementData, index+1, elementData, index,
			     numMoved);
	elementData[--size] = null; // Let gc do its work

	return oldValue;
}
```
