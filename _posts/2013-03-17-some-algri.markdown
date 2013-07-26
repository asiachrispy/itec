---
layout: post
title: "一些有意思的算法"
tag: "tec"
comment: true
published: true
date: 2013-03-17
---

下面给出的几到算法题非常有意思，也许你已经练习过，或者你能给出自己的答案，又或者你完全没有思绪，无论怎样，我都建议你一定坚持看完，并相信你一定受益匪浅，当然，如果你能给出更好的答案，那么请一定和我们一起分享。

```       
/**
 * User: Chris   
 */
public class TouTiaoInterface {

    /**
     *   所谓循环有序数组，就是把一个排好序（以升序排列为例）的数组从某个（未知）位置处截为两段，
     *   把前一段放到后一段的后面，所得到的数组。比如 { 8, 9, 10, 0, 1, 2, 3, 4, 5, 6, 7 }。
     *   如果把数组首尾相接，看成一个环形，那么数组就还是有序的，只不过最小值有可能在任何一个位置。
     *   从最小值开始向后，数值逐渐递增，到数组的最后一个元素时再回到第一个元素。
     *   
     *   1. 我没有完全理解“循环有序数组”这个概念（以为会存在多个递增的子数组,比如{8,9,10,  4,5,6,  0,1,2,3}），
     *      虽然有想到二分查找，但感觉并不合适使用
     *   2. 给出提示后，其实也还是没有完全理解
     *   3. 回来后，查找了下概念，总算明白，实现也还简单
     */
    public static int recursionSearch(int[] arr,int data) {
        if (arr == null || arr.length == 0) {
            return -1;
        }

        int pre = 0;
        for (int i = 0; i < arr.length; i++) {
            if (data == arr[i]) {
                return i;
            }

            if (pre > arr[i]) {
                return binarySearch(arr, data, i, arr.length - 1);//使用递归的二分查找
            }
            pre = arr[i];
        }
        return -1;
    }

    
    /**
     * 检测是否字符串中所有‘{’和‘}’都能匹配
     * 1. 我个人给出的是使用栈来完成
     * 2. 在给出指点后（使用一个int），我给出了解题思路（其实在解题后，我兴奋不已，这简直太棒了）
     * @param value
     * @return
     */
    public static boolean isMatch(String value) {
        if (value == null || "".equals(value)) {
            return false;
        }

        char[] chars = value.toCharArray();
        int match = 0;

        for (char c : chars) {
            if (c == '{') {
                match = match + 1;
            } else if (c == '}') {
                match = match - 1;
            } else {
                continue;
            }

            if (match < 0) {
                return false;
            }
        }

        if (match != 0) {
            return false;
        }
        return true;
    }

    /**
     *   检测字符串是否合法   只考虑16进制和小数 (0xaf90 102.101)
     *   1. 我自己给出的思路完全是错的，因为我理解错了题目（）
     *   2. 在给出提示后，依然没有完全明白给定的字符串怎么算合法的
     *   3. 在最后，才发现其实这个字符只要不是合法的16进制或者小数，就认为是非法字符，否则输入它的10进制数
     * @return
     */
    public static long str2Int(String value) {
        if (value == null || "".equals(value)) {
            return -1;
        }

        int sum = 0;
        String cValue = value.toLowerCase();// 0XAF90 to 0xaf90
        char[] chars = cValue.toCharArray();

        if (cValue.startsWith("0x")) { // String may be a hex
            cValue = cValue.substring(2, cValue.length());// (0xaf90) get after 0x like af90
            for (char c : chars) {
                if (('0' <= c && c <= '9')) {
                    sum = sum * 16 + (c - '0');
                    continue;
                } else if (('a' <= c && c <= 'f')) {
                    sum = sum * 16 + (c - 'a' + 10);
                    continue;
                } else {
                    return -1;
                }
            }
        } else { // is a float
            for (char c : chars) {
                if (('0' <= c && c <= '9')) {
                    sum = sum * 10 + (c - '0');
                    continue;
                } else if ('.' == c) {
                    break;
                } else {
                    return -1;
                }
            }
        }
        return sum;
    }

    /**
     * 递归 二分查找
     * @param arr
     * @param data
     * @param beginIndex
     * @param endIndex
     * @return index
     */
    public static int binarySearch(int[] arr, int data, int beginIndex, int endIndex) {
        int midIndex = (beginIndex + endIndex) / 2;
        if (data < arr[beginIndex] || data > arr[endIndex] || beginIndex > endIndex) {
            return -1;
        }

        if (data < arr[midIndex]) {
            return binarySearch(arr, data, beginIndex, midIndex - 1);
        } else if (data > arr[midIndex]) {
            return binarySearch(arr, data, midIndex + 1, endIndex);
        } else {
            return midIndex;
        }
    }

    public static void main(String[] args) {
        System.out.println(Integer.valueOf("1a01", 16));
        System.out.println(TouTiaoInterface.str2Int("0x1a01"));

        System.out.println(TouTiaoInterface.str2Int(("101.01")));
        System.out.println(TouTiaoInterface.str2Int(("111.01")));

        System.out.println(TouTiaoInterface.isMatch(("a} b{}")));
        System.out.println(TouTiaoInterface.isMatch(("a{ b{} c{} }")));
        System.out.println(TouTiaoInterface.isMatch(("a{ b{} c{} } }")));
        System.out.println(TouTiaoInterface.isMatch(("a{ b{} c{} } {")));

        int[] arr = new int[]{8, 9, 10, 0, 1, 2, 3, 4, 5, 6, 7};
        System.out.println("9's index = " + TouTiaoInterface.recursionSearch(arr, 9));
        System.out.println("0's index = " + TouTiaoInterface.recursionSearch(arr, 0));
        System.out.println("5's index = " + TouTiaoInterface.recursionSearch(arr, 5));

        /*result:
            6657
            6657
            101
            111
            false
            true
            false
            false
            9's index = 1
            0's index = 3
            5's index = 8
        */
    }

    /*
    查找日志中 相同域名的访问数，并按降序排列
    [root@10 chris]# cat test.log
    chris
    chris1
    chris2
    chris3
    chris2
    chris3
    chris3

    [root@10 chris]# cat test.log|sort -r|uniq -c
     3 chris3
     2 chris2
     1 chris1
     1 chris
    */
}


-----------
-----------

###下面是我最新学习过的一些算法

1. KNN k-nearest neighbor
2. PageRank
3. String Matching字符串模式匹配算法
   3.1  BM (Boyer-Moore) 基于后缀匹配的模式串匹配算法，后缀匹配就是模式串从右到左开始比较
   3.2 Sunday
   3.3 KMP

4. tf-idf模型
5. 相似图片搜索的原理
6. 相似度
   6.1 余弦相似度 Cosine-based Similarity
   6.2 欧几里德距离（Euclidean Distance）
   6.3 皮尔逊相关系数
   
7. 聚类算法
   7.1 K 均值聚类算法
   7.2 Canopy 聚类算法
   7.3 模糊 K 均值聚类算法

8. 分类算法
   8.1 决策树学习
   8.2 贝叶斯分类-贝叶斯定理
   
9. K-means算法
```