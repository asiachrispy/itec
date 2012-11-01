---
layout: post
title: "插入排序"
tag: "tec"
comment: true
published: true
date: 2012-10-14
---

我们来看看Collections.sort(List<T> list)排序方法的实现，可以总结如下：
1. 使用Arrays.sort(T[])来排序   
2. 最终(排序数组的大小小于7)使用插入排序
3. 使用二分的方式拆分需要排序的数组，直到数组的大小小于7
4. 使用递归的方式
5.  合并被拆分后被排序的数组
   
**Collections.java 关键代码如下**

```
    public static <T extends Comparable<? super T>> void sort(List<T> list) {
	    Object[] a = list.toArray();
	    Arrays.sort(a);
	    ListIterator<T> i = list.listIterator();
	    for (int j=0; j<a.length; j++) {
	        i.next();
	        i.set((T)a[j]);
	    }
    }
```
** Arrays 关键代码如下**   

```
    public static void sort(Object[] a) {
        Object[] aux = (Object[])a.clone();
        mergeSort(aux, a, 0, a.length, 0);
    }

    /**
     * Src is the source array that starts at index 0
     * Dest is the (possibly larger) array destination with a possible offset
     * low is the index in dest to start sorting
     * high is the end index in dest to end sorting
     * off is the offset to generate corresponding low, high in src
     */
    private static void mergeSort(Object[] src,
				  Object[] dest,
				  int low,
				  int high,
				  int off) {
	int length = high - low;

	// Insertion sort on smallest arrays
        if (length < INSERTIONSORT_THRESHOLD) {
            for (int i=low; i<high; i++)
                for (int j=i; j>low &&
			 ((Comparable) dest[j-1]).compareTo(dest[j])>0; j--)
                    swap(dest, j, j-1);
            return;
        }

        // Recursively sort halves of dest into src
        int destLow  = low;
        int destHigh = high;
        low  += off;
        high += off;
        int mid = (low + high) >>> 1;
        mergeSort(dest, src, low, mid, -off);
        mergeSort(dest, src, mid, high, -off);

        // If list is already sorted, just copy from src to dest.  This is an
        // optimization that results in faster sorts for nearly ordered lists.
        if (((Comparable)src[mid-1]).compareTo(src[mid]) <= 0) {
            System.arraycopy(src, low, dest, destLow, length);
            return;
        }

        // Merge sorted halves (now in src) into dest
        for(int i = destLow, p = low, q = mid; i < destHigh; i++) {
            if (q >= high || p < mid && ((Comparable)src[p]).compareTo(src[q])<=0)
                dest[i] = src[p++];
            else
                dest[i] = src[q++];
        }
    }
```
