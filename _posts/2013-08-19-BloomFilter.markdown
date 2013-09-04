---
layout: post
title: "你应该知道的布隆过滤器(BloomFilter)"
tag: "tec"
comment: true
published: true
date: 2013-08-18
---

之所以谈论这个问题，是因为面试中偶尔会提到这个问题，
重要的是我是一个爬虫工作者（多么美妙的名字，不是吗），没有理由不知道。
另外它的价值不仅仅在于处理爬虫url排重等问题，
比如检测一个可疑的电子邮件地址是否在黑名单中；
又比如，查询一个字符串中，包含哪些字符等问题；
大家可以查询wiki[布隆过滤器](http://zh.wikipedia.org/wiki/%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8)了解更多的知识。


```   
package com.chris.review;

import java.util.BitSet;

	/**   
	 * User: zhong.huang   
	 * Date: 13-8-18   
	 *   
	 * 一些方法   
	 *   
	 * public void set(int pos): 位置pos的字位设置为true。   
	 * public void set(int bitIndex, boolean value) 将指定索引处的位设置为指定的值。   
	 * public void clear(int pos): 位置pos的字位设置为false。   
	 * public void clear() : 将此 BitSet 中的所有位设置为 false。   
	 * public int cardinality() 返回此 BitSet 中设置为 true 的位数。   
	 * public boolean get(int pos): 返回位置是pos的字位值。   
	 * public void and(BitSet other): other同该字位集进行与操作，结果作为该字位集的新值。   
	 * public void or(BitSet other): other同该字位集进行或操作，结果作为该字位集的新值。   
	 * public void xor(BitSet other): other同该字位集进行异或操作，结果作为该字位集的新值。   
	 * public void andNot(BitSet set) 清除此 BitSet 中所有的位,set - 用来屏蔽此 BitSet 的 BitSet   
	 * public int size(): 返回此 BitSet 表示位值时实际使用空间的位数。   
	 * public int length() 返回此 BitSet 的“逻辑大小”：BitSet 中最高设置位的索引加 1。   
	 * public int hashCode(): 返回该集合Hash 码， 这个码同集合中的字位值有关。   
	 * public boolean equals(Object other): 如果other中的字位同集合中的字位相同，返回true。   
	 * public Object clone() 克隆此 BitSet，生成一个与之相等的新 BitSet。   
	 * public String toString() 返回此位 set 的字符串表示形式。   
	 */   
public class BloomFilter {
    private int DEFAULT_SIZE = 2 << 24;
    private int[] seeds = {3, 7, 11, 13, 31, 37, 61};
    private BitSet bitSet;

    public BloomFilter() {
        bitSet = new BitSet(DEFAULT_SIZE);
    }

    //计算Hash值
    private int hash(String str, int n) {
        int result = 0;
        for (int i = 0; i < str.length(); i++) {
            result = seeds[n] * result + str.charAt(i);
            if (result > 2 << 24) {
                result %= 2 << 24;
            }
        }
        return result;
    }

    //判断是否在布隆过滤器中
    private boolean exists(String str) {
        int hash = 0;
        for (int i = 0; i < seeds.length; i++) {
            hash = hash(str, i);
            if (bitSet.get(hash) == false)
                return false;
        }
        return true;
    }

    //添加元素到布隆过滤器
    private void add(String str) {
        int hash;
        for (int i = 0; i < seeds.length; i++) {
            hash = hash(str, i);
            bitSet.set(hash);
        }
    }

    //初始化布隆过滤器
    private void init(int num) {
        for (int i = 0; i < num; i++) {
            add("http://www.dajie.com" + i);
        }
    }

    public static void main(String[] args) {
        BloomFilter bf = new BloomFilter();
        bf.init(10000000);
        long start = System.currentTimeMillis();
        System.out.println(bf.exists("http://www.dajie.com"));
        long end = System.currentTimeMillis();
        System.out.println(end - start);
        start = end;
        System.out.println(bf.exists("http://www.dajie.com1"));
        end = System.currentTimeMillis();
        System.out.println(end - start);
    }

}
```
