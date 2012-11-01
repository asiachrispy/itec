---
layout: post
title: "Huffman 编码压缩算法"
tag: "tec"
comment: true
published: true
date: 2012-09-01
---

之所以要转载这篇文章，是因为无论是英文原文还是[酷壳]翻译的中文，都能让你轻松的理解这个著名的压缩算法。我读过的分析算法的文章很多，但这是我只读完一遍就完全理解的算法文章，而且Huffman 编码算法本就是很有难度的，这只能说作者和译者对算法本身的理解和文笔都是深厚的。能这样简单明了的分析算法的文章真的不多，我想这也许是我们每个算法爱好者
都应该追求的目标。


英文原文我摘自[《A Simple Example of Huffman Code on a String》](http://en.nerdaholyc.com/huffman-coding-on-a-string/),译文摘自酷壳的[Huffman 编码压缩算法](http://coolshell.cn/articles/7459.html),这里我只摘出英文，看中文的可以去[酷壳](http://coolshell.cn/)查阅。

You’ve probably heard about David Huffman and his popular compression algorithm. If you didn’t, you’ll find that info on the Internet. I will not bore you with history or math lessons in this article. I’m going to try to show you a practical example of this algorithm applied to a character string. This application will only generate console output representing the code values for the symbols inputted and generate the original symbols from a given code.

The source code attached to this article will show you how Huffman Coding works so you will get a basic understanding of it. This is for the people who have difficulty understanding the mathematics of it. In a future article (I hope) we’ll be talking about how to apply this to any files to produce their compressed format. (A simple file archiver like WinZip or WinRAR.)

The idea behind Huffman coding is based upon the frequency of a symbol in a sequence. The symbol that is the most frequent in that sequence gets a new code that is very small, the least frequent symbol will get a code that is very long, so that when we’ll translate the input we want to encode the most frequent symbols will take less space than they used to and the least frequent symbols will take more space but because they’re less frequent it won’t matter that much. For this application I chose the symbol to be 8 bits long so that the symbol will be a character (char).

We could just as easily have chosen the symbol to be 16 bits long, so we could have grouped 2 characters together as a symbol or 10 bits or 20 etc. Depending on the input we expect to have, we’ll chose the size of the symbol and the way we use it. For example, if I expect to encode raw video files, I’ll chose the symbol to be the size of a pixel. Keep in mind that when increasing or decreasing the size of the symbol, it will affect the size of the code for each symbol because the bigger the size, the more symbols you can have of that size. There are less ways to write the ones and zeroes on 8 bits than there are on 16 bits. You’ll want to adjust the size of the symbol depending on how the ones and zeroes are likely to repeat themselves in a sequence.

For this algorithm you need to have a basic understanding of binary tree data structure and the priority queue data structure. In the source code we’ll actually use the priority queue code available in a previous article.

Let’s say we have the string “beep boop beer!” which in his actual form, occupies 1 byte of memory for each character. That means that in total, it occupies 15*8 = 120 bits of memory. Through encoding, the string will occupy 40 bits. (Theoretically, in this application we’ll output to the console a string of 40 char elements of 0 and 1 representing the encoded version of the string in bits. For this to occupy 40 bits we need to convert that string directly into bits using logical bit operations which we’ll not discuss now.)

To better understand this example, we’ll going to apply it on an example. The string “beep boop beer!” is a very good example to illustrate this. In order to obtain the code for each element depending on it’s frequency we’ll need to build a binary tree such that each leaf of the tree will contain a symbol (a character from the string). The tree will be build from the leafs to the root, meaning that the elements of least frequency will be farther from the root than the elements that are more frequent. You’ll see soon why we chose to do this.

To build the tree this way we’ll use a priority queue with a slight modification, that the element with the least priority is the most important. Meaning that the elements that are the least frequent will be the first ones we get from the queue. We need to do this so we can build the tree from the leaves to the root.

Firstly we calculate the frequency of each character :
<table style="width: 250px; height: 200px;">
<tbody>
<tr>
<td><span style="font-size: 12px;">Character</span></td>
<td><span style="font-size: 12px;">Frequency</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘b’</span></td>
<td><span style="font-size: 12px;">3</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘e’</span></td>
<td><span style="font-size: 12px;">4</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘p’</span></td>
<td><span style="font-size: 12px;">2</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘ ‘</span></td>
<td><span style="font-size: 12px;">2</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘o’</span></td>
<td><span style="font-size: 12px;">2</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘r’</span></td>
<td><span style="font-size: 12px;">1</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘!’</span></td>
<td><span style="font-size: 12px;">1</span></td>
</tr>
</tbody>
</table>

After calculating the frequencies, we’ll create binary tree nodes for each character and we’ll introduce them in the priority queue with the frequency as priority :

![1](http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada1.png){:.imglink}
<!-- <a href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada1.png"><img src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada1.png"/></a> -->

We now get the first two elements from the queue and create a link between them by creating a new binary tree node to have them both as successors, so that the characters are siblings and we add their priorities. After that we add the new node we created with the sum of the priorities of it’s successors as it’s priority in the queue. (The numbers represent the priority, i.e. their frequency.)

![2](http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada2.png){:.imglink}
<!-- <a href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada2.png"><img src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada2.png"/></a> -->

We repeat the same steps and we get the following :

![3](http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada31.png){:.imglink}
<!-- <a href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada31.png"><img src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada31.png"/></a> -->

![4](http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada4.png){:.imglink}
<!--<a href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada4.png"><img src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada4.png"/></a>-->

![5](http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada5.png){:.imglink}
<!--<a href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada5.png"><img src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada5.png"/></a>-->

![6](http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada61.png){:.imglink}
<!-- <a href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada61.png"><img src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/coada61.png"/></a> -->

Now after we link the last two elements we’ll get the final tree :

<a class="imglink external" href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/arbore_final.png"><img width="452" height="304" alt="" src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/arbore_final.png" title="arbore_final" class="alignnone size-full wp-image-291"></a> 

Now, to obtain the code for each symbol we just need to traverse the trees until we get to that symbol and after each step we take to the left we add a 0 to the code or 1 if we go right.

<a class="imglink external" href="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/arbore_final_numerotat.png"><img width="452" height="304" alt="" src="http://ro.nerdaholyc.com/wp-content/uploads/2012/03/arbore_final_numerotat.png" title="arbore_final_numerotat" class="alignnone size-full wp-image-292"></a> 

If we do this, we’ll get the following codes :
<table style="width: 250px; height: 200px;">
<tbody>
<tr>
<td><span style="font-size: 12px;">Character</span></td>
<td><span style="font-size: 12px;">Code</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘b’</span></td>
<td><span style="font-size: 12px;">00</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘e’</span></td>
<td><span style="font-size: 12px;">11</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘p’</span></td>
<td><span style="font-size: 12px;">101</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘ ‘</span></td>
<td><span style="font-size: 12px;">011</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘o’</span></td>
<td><span style="font-size: 12px;">010</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘r’</span></td>
<td><span style="font-size: 12px;">1000</span></td>
</tr>
<tr>
<td><span style="font-size: 12px;">‘!’</span></td>
<td><span style="font-size: 12px;">1001</span></td>
</tr>
</tbody>
</table>

To decode a string of bits we just need to traverse the tree for each bit, if the bit is 0 we take a left step and if the bit is 1 we take a right step until we hit a leaf (which is the symbol we are looking for). For example, if we have the string “101 11 101 11″ and our tree, decoding it we’ll get the string “pepe”.

It’s very important to observe that not one code is a prefix of another code for another symbol. In our example, if 00 is the code for ‘b’, 000 cannot be a code for any other symbol because there’s going to be a conflict. We’ll never reach that symbol because after taking steps for the first two bits we’ll get ‘b’, we’re never going the find the symbol for 000.

A practical aspect of implementing this algorithm is considering to build a Huffman table as soon as we have the tree. The table is basically a linked list or an array that contains each symbol with it’s code because it will make encoding something more efficient. It’s hard to look for a symbol by traversing a tree and at the same time calculating it’s code because we don’t know where exactly in the tree is that symbol located. As a principle, we use a Huffman table for encoding and a Huffman tree for decoding.

The input string : beep boop beer!

The input string in binary : 0110 0010 0110 0101 0110 0101 0111 0000 0010 0000 0110 0010 0110 1111 0110 1111 0111 0000 0010 0000 0110 0010 0110 0101 0110 0101 0111 0010 0010 0001

The encoded string : 0011 1110 1011 0001 0010 1010 1100 1111 1000 1001

As you can see there is a major difference in the ASCII version of the string and the Huffman coded version.

The source code behaves as described above. You’ll find more details in the comments in the code.

All the sources have been compiled and verified using the C99 standard. Happy programming.

Download the source files

To be clearer since there have been comments on this. This article only illustrates how the algorithm works. To use this in a real scenario you need to add the Huffman tree you created at the end or at the start of your encoded string and the reciever should know how to interpret it in order to decode the message. A good way to do this is to build a string of bits by traversing the tree in any order you want (I prefer post-order) and concatenate a 0 for each node that is not a leaf and a 1 for each node that is a leaf followed by the bits representing the original symbol (in our case, 8 bits representing the ASCII char value). Placing this string of bits at the start of your encoded message is ideal. Once the reciever has build the tree, he will know how to decode the message to obtain the original one.

To read about a more advanced version of this algorithm, you can read our article on Adaptive Huffman Coding.
Sharing options




