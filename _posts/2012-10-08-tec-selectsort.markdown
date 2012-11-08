---
layout: post
title: "选择排序"
tag: "tec"
comment: true
published: true
date: 2012-10-08
---

看看选择排序如何实现。


```   

    static void selectionSort (int[] array) {
          // 最后一个数不需要再比较了，所以-1
	    for (int i = 0; i < array.length - 1; i++) {  
			int min_idx = i; // 假设当前值最小的值，并记录下标
			
			for (int j = i + 1; j < array.length; j++) {
			// 如果当前值比最小值还小，则认为是找到了新的最小值，并记录下标
				if (array[j] < array[min_idx]) { 
					min_idx = j;
				}
			}
			
			// 如果有新的最小值，则和假设的最小值交换位置
			if (min_idx != i)  {
				swap(array, min_idx, i);
			}
		}
    }
```

