---
layout: post
title: 程序员面试的Top10算法概念
categories: [blog]
tags: [数据结构和算法, 精华帖]
description: 程序员面试的Top10算法概念
---

> 这是一篇译文，原文地址：[Top 10 Algorithms for Coding Interview](http://www.programcreek.com/2012/11/top-10-algorithms-for-coding-interview/)

以下列出的是程序员面试中流行的算法概念。完全理解这些算法概念需要花更多的精力，所以本文只是对算法概念进行介绍。<br/>

[1) String / Array](#1) &ensp; [2) Matrix](#2) &ensp; [3) Linked List](#3) &ensp; [4) Tree / Heap](#4) <br/>
[5) Graph](#5) &ensp; [6) Sorting](#6) &ensp; [7) Recursion / Iteration](#7) &ensp; [8) Dynamic Programming](#8) <br/>
[9) Bit Manipulation](#9) &ensp; [10) Combinations&Permutations](#10) &ensp; [11) Math Problems](#11) <br/>

如果你需要了解Java的基础知识，我十分建议你阅读 "[Simple Java](http://www.programcreek.com/simple-java/)"。如果你想要知道使用一个流行的API的具体实例，你可以在 [JavaSED.com](http://www.javased.com/) 找到。

> 说明：在例题中，类似的问题用相同的数字标注。

<h2 id="1">1. String / Array</h2>

字符串是Java中的类。它包括了一个字符区域、其它区域和方法。如果你的 IDE 没有代码自动补全功能，你应当记住以下的方法：

    toCharArray()				//get char array of a String
    charAt(int x)				//get a char at the specific index
    length()				//string length
    length					//array size
    substring(int beginIndex)
    substring(int beginIndex, int endIndex)
    Integer.valueOf()			//string to integer
    String.valueOf()			//integer to string
    Arrays.sort()				//sort an array
    Arrays.toString(char[] a) 		//convert to string
    Arrays.copyOf(T[] original, int newLength)
    System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)

字符串 / 数组的数据结构很容易理解，所以面试题中经常会需要用到高级的算法进行解决，例如[动态规划](#8)，[递归](#7) 等等。

经典问题如下：<br/>
[1) Rotate Array - 字符串旋转](http://www.programcreek.com/2015/03/rotate-array-in-java/) <br/>
[2) Evaluate Reverse Polish Notation (Stack) - 逆波兰式计算结果](http://www.programcreek.com/2012/12/leetcode-evaluate-reverse-polish-notation/) <br/>
[3) Isomorphic Strings - 同型字符串](http://www.programcreek.com/2014/05/leetcode-isomorphic-strings-java/) <br/>
[4) Word Ladder (BFS) - 单词梯子](http://www.programcreek.com/2012/12/leetcode-word-ladder/) <br/>
[4) Word Ladder II (BFS) - 单词梯子2](http://www.programcreek.com/2014/06/leetcode-word-ladder-ii-java/) <br/>
[5) Median of Two Sorted Arrays - 找两个有序数组的中位数](http://www.programcreek.com/2012/12/leetcode-median-of-two-sorted-arrays-java/) <br/>
[5) Kth Largest Element in an Array - 找第K大的数](http://www.programcreek.com/2014/05/leetcode-kth-largest-element-in-an-array-java/) <br/>
[6) Wildcard Matching - 通配符比对](http://www.programcreek.com/2014/06/leetcode-wildcard-matching-java/) <br/>
[6) Regular Expression Matching - 正则表达式比对](http://www.programcreek.com/2012/12/leetcode-regular-expression-matching-in-java/) <br/>
[7) Merge Intervals - 合并间隔段](http://www.programcreek.com/2012/12/leetcode-merge-intervals/) <br/>
[8) Insert Interval - 插入间隔段](http://www.programcreek.com/2012/12/leetcode-insert-interval/) <br/>
[9) Two Sum II – Input array is sorted](http://www.programcreek.com/2014/03/two-sum-ii-input-array-is-sorted-java/) <br/>
[9) Two Sum III - Data structure design](http://www.programcreek.com/2014/03/two-sum-iii-data-structure-design-java/) <br/>
[9) 3Sum](http://www.programcreek.com/2012/12/leetcode-3sum/) <br/>
[9) 4Sum](http://www.programcreek.com/2013/02/leetcode-4sum-java/) <br/>
[10) 3Sum Closest](http://www.programcreek.com/2013/02/leetcode-3sum-closest-java/) <br/>
[11) String to Integer](http://www.programcreek.com/2012/12/leetcode-string-to-integer-atoi/) <br/>
[12) Merge Sorted Array](http://www.programcreek.com/2012/12/leetcode-merge-sorted-array-java/) <br/>
[13) Valid Parentheses](http://www.programcreek.com/2012/12/leetcode-valid-parentheses-java/) <br/>
[13) Longest Valid Parentheses](http://www.programcreek.com/2014/06/leetcode-longest-valid-parentheses-java/) <br/>
[14) Implement strStr()](http://www.programcreek.com/2012/12/leetcode-implement-strstr-java/) <br/>
[15) Minimum Size Subarray Sum](http://www.programcreek.com/2014/05/leetcode-minimum-size-subarray-sum-java/) <br/>
[16) Search Insert Position](http://www.programcreek.com/2013/01/leetcode-search-insert-position/) <br/>
[17) Longest Consecutive Sequence](http://www.programcreek.com/2013/01/leetcode-longest-consecutive-sequence-java/) <br/>
[18) Valid Palindrome](http://www.programcreek.com/2013/01/leetcode-valid-palindrome-java/) <br/>
[19) ZigZag Conversion](http://www.programcreek.com/2014/05/leetcode-zigzag-conversion-java/) <br/>
[20) Add Binary](http://www.programcreek.com/2014/05/leetcode-add-binary-java/) <br/>
[21) Length of Last Word](http://www.programcreek.com/2014/05/leetcode-length-of-last-word-java/) <br/>
[22) Triangle](http://www.programcreek.com/2013/01/leetcode-triangle-java/) <br/>
[24) Contains Duplicate](http://www.programcreek.com/2014/05/leetcode-contains-duplicate-java/) <br/>
[24) Contains Duplicate II](http://www.programcreek.com/2014/05/leetcode-contains-duplicate-ii-java/) <br/>
[24) Contains Duplicate III](http://www.programcreek.com/2014/06/leetcode-contains-duplicate-iii-java/) <br/>
[25) Remove Duplicates from Sorted Array](http://www.programcreek.com/2013/01/leetcode-remove-duplicates-from-sorted-array-java/) <br/>
[26) Remove Duplicates from Sorted Array II](http://www.programcreek.com/2013/01/leetcode-remove-duplicates-from-sorted-array-ii-java/) <br/>
[27) Longest Substring Without Repeating Characters](http://www.programcreek.com/2013/02/leetcode-longest-substring-without-repeating-characters-java/) <br/>
[28) Longest Substring that contains 2 unique characters [Google]](http://www.programcreek.com/2013/02/longest-substring-which-contains-2-unique-characters/) <br/>
[28) Substring with Concatenation of All Words](http://www.programcreek.com/2014/06/leetcode-substring-with-concatenation-of-all-words-java/) <br/>
[29) Minimum Window Substring](http://www.programcreek.com/2014/05/leetcode-minimum-window-substring-java/) <br/>
[30) Reverse Words in a String](http://www.programcreek.com/2014/02/leetcode-reverse-words-in-a-string-java/) <br/>
[31) Find Minimum in Rotated Sorted Array](http://www.programcreek.com/2014/02/leetcode-find-minimum-in-rotated-sorted-array/) <br/>
[31) Find Minimum in Rotated Sorted Array II](http://www.programcreek.com/2014/03/leetcode-find-minimum-in-rotated-sorted-array-ii-java/) <br/>
[31) Search in Rotated Sorted Array](http://www.programcreek.com/2014/06/leetcode-search-in-rotated-sorted-array-java/) <br/>
[31) Search in Rotated Sorted Array II](http://www.programcreek.com/2014/06/leetcode-search-in-rotated-sorted-array-ii-java/) <br/>
[32) Find Peak Element](http://www.programcreek.com/2014/02/leetcode-find-peak-element/) <br/>
[33) Min Stack](http://www.programcreek.com/2014/02/leetcode-min-stack-java/) <br/>
[34) Majority Element](http://www.programcreek.com/2014/02/leetcode-majority-element-java/) <br/>
[34) Majority Element II](http://www.programcreek.com/2014/07/leetcode-majority-element-ii-java/) <br/>
[35) Remove Element](http://www.programcreek.com/2014/04/leetcode-remove-element-java/) <br/>
[36) Largest Rectangle in Histogram](http://www.programcreek.com/2014/05/leetcode-largest-rectangle-in-histogram-java/) <br/>
[37) Longest Common Prefix [Google]](http://www.programcreek.com/2014/02/leetcode-longest-common-prefix-java/) <br/>
[38) Largest Number](http://www.programcreek.com/2014/02/leetcode-largest-number-java/) <br/>
[39) Simplify Path](http://www.programcreek.com/2014/04/leetcode-simplify-path-java/) <br/>
[40) Compare Version Numbers](http://www.programcreek.com/2014/03/leetcode-compare-version-numbers-java/) <br/>
[41) Gas Station](http://www.programcreek.com/2014/03/leetcode-gas-station-java/) <br/>
[44) Pascal's Triangle](http://www.programcreek.com/2014/03/leetcode-pascals-triangle-java/) <br/>
[44) Pascal’s Triangle II](http://www.programcreek.com/2014/04/leetcode-pascals-triangle-ii-java/) <br/>
[45) Container With Most Water](http://www.programcreek.com/2014/03/leetcode-container-with-most-water-java/) <br/>
[45) Candy [Google]](http://www.programcreek.com/2014/03/leetcode-candy-java/) <br/>
[45) Trapping Rain Water](http://www.programcreek.com/2014/06/leetcode-trapping-rain-water-java/) <br/>
[46) Count and Say](http://www.programcreek.com/2014/03/leetcode-count-and-say-java/) <br/>
[47) Search for a Range](http://www.programcreek.com/2014/04/leetcode-search-for-a-range-java/) <br/>
[48) Basic Calculator](http://www.programcreek.com/2014/06/leetcode-basic-calculator-java/) <br/>
[49) Anagrams](http://www.programcreek.com/2014/04/leetcode-anagrams-java/) <br/>
[50) Shortest Palindrome](http://www.programcreek.com/2014/06/leetcode-shortest-palindrome-java/) <br/>
[51) Rectangle Area](http://www.programcreek.com/2014/06/leetcode-rectangle-area-java/) <br/>
[52) Summary Ranges](http://www.programcreek.com/2014/07/leetcode-summary-ranges-java/) <br/>

<h2 id="2">2. Matrix</h2>

经常用来解决矩阵相关问题的算法包括 DFS，BFS，[动态规划](#8) 等等。

经典问题如下：<br/>
[1) Set Matrix Zeroes](http://www.programcreek.com/2012/12/leetcode-set-matrix-zeroes-java/) <br/>
[2) Spiral Matrix](http://www.programcreek.com/2013/01/leetcode-spiral-matrix-java/) <br/>
[2) Spiral Matrix II](http://www.programcreek.com/2014/05/leetcode-spiral-matrix-ii-java/) <br/>
[3) Search a 2D Matrix](http://www.programcreek.com/2013/01/leetcode-search-a-2d-matrix-java/) <br/>
[4) Rotate Image [Palantir]](http://www.programcreek.com/2013/01/leetcode-rotate-image-java/) <br/>
[5) Valid Sudoku](http://www.programcreek.com/2014/05/leetcode-valid-sudoku-java/) <br/>
[6) Minimum Path Sum (DP) [Google]](http://www.programcreek.com/2014/05/leetcode-minimum-path-sum-java/) <br/>
[7) Unique Paths (DP) [Google]](http://www.programcreek.com/2014/05/leetcode-unique-paths-java/) <br/>
[7) Unique Paths II (DP)](http://www.programcreek.com/2014/05/leetcode-unique-paths-ii-java/) <br/>
[8) Number of Islands (DFS/BFS)](http://www.programcreek.com/2014/04/leetcode-number-of-islands-java/) <br/>
[9) Surrounded Regions (BFS)](http://www.programcreek.com/2014/04/leetcode-surrounded-regions-java/) <br/>
[10) Maximal Rectangle](http://www.programcreek.com/2014/05/leetcode-maximal-rectangle-java/) <br/>
[10) Maximal Square](http://www.programcreek.com/2014/06/leetcode-maximal-square-java/) <br/>
[11) Word Search (DFS)](http://www.programcreek.com/2014/06/leetcode-word-search-java/) <br/>
[11) Word Search II](http://www.programcreek.com/2014/06/leetcode-word-search-ii-java/) <br/>

<h2 id="3">3. Linked List</h2>

Java 中链表的实现十分简单。每个节点有个值和一个指向下一个节点的链接。

{% highlight ruby %}
class Node {
    int val;
    Node next;
 
    Node(int x) {
        val = x;
        next =null;
    }
}
{% endhighlight %}

两种链表的普遍应用是栈和队列。

**Stack**

{% highlight ruby %}
class Stack {
    Node top; 
 
    public Node peek() {
        if(top !=null) { return top; }
 
        return null;
    }
 
    public Node pop() {
        if(top == null) { return null; }
        else {
            Node temp = new Node(top.val);
            top = top.next;
            return temp;
        }
    }
 
    public void push(Node n) {
        if(n !=null) {
            n.next= top;
            top = n;
        }
    }
}
{% endhighlight %}

**Queue**

{% highlight ruby %}
class Queue {
    Node first, last;
 
    public void enqueue(Node n) {
        if(first == null) {
            first = n;
            last = first;
        } else {
            last.next= n;
            last = n;
        }
    }
 
    public Node dequeue() {
        if(first == null) {
            return null;
        } else {
            Node temp = new Node(first.val);
            first = first.next;
            return temp;
        }
    }
}
{% endhighlight %}

Java 标准库中有一个类叫做 "[Stack](http://docs.oracle.com/javase/7/docs/api/java/util/Stack.html)"。在 Java SDK 中还有一个类叫做 "[Linked List](http://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html)"，可以被当做**队列**使用，具有 `add()` 和 `remove()` 方法。（Linked List 实现了 Queue 的接口）。记得在面试中适时地使用栈和队列。

经典问题如下：<br/>
[1) Add Two Numbers](http://www.programcreek.com/2012/12/add-two-numbers/) <br/>
[2) Reorder List](http://www.programcreek.com/2013/12/in-place-reorder-a-singly-linked-list-in-java/) <br/>
[3) Linked List Cycle](http://www.programcreek.com/2012/12/leetcode-linked-list-cycle/) <br/>
[4) Copy List with Random Pointer](http://www.programcreek.com/2012/12/leetcode-copy-list-with-random-pointer/) <br/>
[5) Merge Two Sorted Lists](http://www.programcreek.com/2012/12/leetcode-merge-two-sorted-lists-java/) <br/>
[6) Merge k Sorted Lists \*](http://www.programcreek.com/2013/02/leetcode-merge-k-sorted-lists-java/) <br/>
[7) Remove Duplicates from Sorted List](http://www.programcreek.com/2013/01/leetcode-remove-duplicates-from-sorted-list/) <br/>
[7) Remove Duplicates from Sorted List II](http://www.programcreek.com/2014/06/leetcode-remove-duplicates-from-sorted-list-ii-java/) <br/>
[8) Partition List](http://www.programcreek.com/2013/02/leetcode-partition-list-java/) <br/>
[9) LRU Cache](http://www.programcreek.com/2013/03/leetcode-lru-cache-java/) <br/>
[10) Intersection of Two Linked Lists](http://www.programcreek.com/2014/02/leetcode-intersection-of-two-linked-lists-java/) <br/>
[11) Remove Linked List Elements](http://www.programcreek.com/2014/04/leetcode-remove-linked-list-elements-java/) <br/>
[12) Swap Nodes in Pairs](http://www.programcreek.com/2014/04/leetcode-swap-nodes-in-pairs-java/) <br/>
[13) Reverse Linked List](http://www.programcreek.com/2014/05/leetcode-reverse-linked-list-java/) <br/>
[13) Reverse Linked List II](http://www.programcreek.com/2014/06/leetcode-reverse-linked-list-ii-java/) <br/>
[14) Remove Nth Node From End of List (Fast-Slow Pointers)](http://www.programcreek.com/2014/05/leetcode-remove-nth-node-from-end-of-list-java/) <br/>
[15) Implement Stack using Queues](http://www.programcreek.com/2014/06/leetcode-implement-stack-using-queues-java/) <br/>
[15) Implement Queue using Stacks](http://www.programcreek.com/2014/07/leetcode-implement-queue-using-stacks-java/) <br/>
[16) Palindrome Linked List](http://www.programcreek.com/2014/07/leetcode-palindrome-linked-list-java/) <br/>
[17) Implement a Queue using an Array](http://www.programcreek.com/2014/07/implement-a-queue-using-an-array-in-java/) <br/>
[18) Delete Node in a Linked List](http://www.programcreek.com/2014/07/leetcode-delete-node-in-a-linked-list-java/) <br/>

<h2 id="4">4. Tree / Heap</h2>

树结构，通常是指二叉树。每个节点有一个左节点和右节点，如下所示：

{% highlight ruby %}
class TreeNode {
    int value;
    TreeNode left;
    TreeNode right;
}
{% endhighlight %}

**与二叉树相关的概念：**

1. 二叉搜索树：对于所有的节点，都有 左子节点<=当前节点<=右子节点
2. 平衡树：左子树和右子树的高度相差小于等于1
3. 满二叉树：除了叶子节点外，所有的节点都有两个子节点
4. 完美二叉树：首先使一个满二叉树，其次，所有的叶子节点都有相同的深度，或者在同一级，而且，叶子节点的父节点都有两个子节点
5. 完全二叉树：除了最后一级外，其余层级都被填满了，另外，所有节点都靠左排列

[Heap(堆)](http://en.wikipedia.org/wiki/Heap_(data_structure)) 是一个满足堆属性的，基于树结构的数据结构。对于堆操作来说，时间复杂性是重要的（例如：查找最小值，删除最小值，插入 等等）。在 Java 语言中，[Priority Queue(优先队列)](http://www.programcreek.com/2009/02/using-the-priorityqueue-class-example/) 也是值得了解的。

经典问题如下：<br/>

[1) Binary Tree Preorder Traversal](http://www.programcreek.com/2012/12/leetcode-solution-for-binary-tree-preorder-traversal-in-java/) <br/>
[2) Binary Tree Inorder Traversal [Palantir]](http://www.programcreek.com/2012/12/leetcode-solution-of-binary-tree-inorder-traversal-in-java/) <br/>
[3) Binary Tree Postorder Traversal](http://www.programcreek.com/2012/12/leetcode-solution-of-iterative-binary-tree-postorder-traversal-in-java/) <br/>
[4) Binary Tree Level Order Traversal](http://www.programcreek.com/2014/04/leetcode-binary-tree-level-order-traversal-java/) <br/>
[4) Binary Tree Level Order Traversal II](http://www.programcreek.com/2014/04/leetcode-binary-tree-level-order-traversal-ii-java/) <br/>
[5) Validate Binary Search Tree](http://www.programcreek.com/2012/12/leetcode-validate-binary-search-tree-java/) <br/>
[6) Flatten Binary Tree to Linked List](http://www.programcreek.com/2013/01/leetcode-flatten-binary-tree-to-linked-list/) <br/>
[7) Path Sum (DFS or BFS)](http://www.programcreek.com/2013/01/leetcode-path-sum/) <br/>
[7) Path Sum II (DFS)](http://www.programcreek.com/2014/05/leetcode-path-sum-ii-java/) <br/>
[8) Construct Binary Tree from Inorder and Postorder Traversal](http://www.programcreek.com/2013/01/construct-binary-tree-from-inorder-and-postorder-traversal/) <br/>
[8) Construct Binary Tree from Preorder and Inorder Traversal](http://www.programcreek.com/2014/06/leetcode-construct-binary-tree-from-preorder-and-inorder-traversal-java/) <br/>
[9) Convert Sorted Array to Binary Search Tree [Google]](http://www.programcreek.com/2013/01/leetcode-convert-sorted-array-to-binary-search-tree-java/) <br/>
[10) Convert Sorted List to Binary Search Tree [Google]](http://www.programcreek.com/2013/01/leetcode-convert-sorted-list-to-binary-search-tree-java/) <br/>
[11) Minimum Depth of Binary Tree](http://www.programcreek.com/2013/02/leetcode-minimum-depth-of-binary-tree-java/) <br/>
[12) Binary Tree Maximum Path Sum \*](http://www.programcreek.com/2013/02/leetcode-binary-tree-maximum-path-sum-java/) <br/>
[13) Balanced Binary Tree](http://www.programcreek.com/2013/02/leetcode-balanced-binary-tree-java/) <br/>
[14) Symmetric Tree](http://www.programcreek.com/2014/03/leetcode-symmetric-tree-java/) <br/>
[15) Binary Search Tree Iterator](http://www.programcreek.com/2014/04/leetcode-binary-search-tree-iterator-java/) <br/>
[16) Binary Tree Right Side View](http://www.programcreek.com/2014/04/leetcode-binary-tree-right-side-view-java/) <br/>
[17) Implement Trie (Prefix Tree)](http://www.programcreek.com/2014/05/leetcode-implement-trie-prefix-tree-java/) <br/>
[18) Add and Search Word - Data structure design (DFS)](http://www.programcreek.com/2014/05/leetcode-add-and-search-word-data-structure-design-java/) <br/>
[19) Merge k sorted arrays [Google]](http://www.programcreek.com/2014/05/merge-k-sorted-arrays-in-java/) <br/>
[20) Populating Next Right Pointers in Each Node](http://www.programcreek.com/2014/05/leetcode-populating-next-right-pointers-in-each-node-java/) <br/>
[21) Populating Next Right Pointers in Each Node II](http://www.programcreek.com/2014/06/leetcode-populating-next-right-pointers-in-each-node-ii-java/) <br/>
[21) Unique Binary Search Trees (DP)](http://www.programcreek.com/2014/05/leetcode-unique-binary-search-trees-java/) <br/>
[21) Unique Binary Search Trees II (DFS)](http://www.programcreek.com/2014/05/leetcode-unique-binary-search-trees-ii-java/) <br/>
[22) Sum Root to Leaf Numbers (DFS)](http://www.programcreek.com/2014/05/leetcode-sum-root-to-leaf-numbers-java/) <br/>
[23) Count Complete Tree Nodes](http://www.programcreek.com/2014/06/leetcode-count-complete-tree-nodes-java/) <br/>
[24) Invert Binary Tree](http://www.programcreek.com/2014/06/leetcode-invert-binary-tree-java/) <br/>
[25) Kth Smallest Element in a BST](http://www.programcreek.com/2014/07/leetcode-kth-smallest-element-in-a-bst-java/) <br/>
[26) Lowest Common Ancestor of a Binary Search Tree](http://www.programcreek.com/2014/07/leetcode-lowest-common-ancestor-of-a-binary-search-tree-java/) <br/>
[26) Lowest Common Ancestor of a Binary Tree](http://www.programcreek.com/2014/07/leetcode-lowest-common-ancestor-of-a-binary-tree-java/) <br/>

<h2 id="5">5. Graph</h2>

图相关的问题，主要用深度优先搜索和宽度优先搜索来解决。深度优先搜索，顾名思义，你可以从根节点开始，循环地访问它的邻居节点。<br/>
下图是宽度优先搜索的简单实现的示例图。关键之处在于使用一个队列来存储节点。
![Graph for BFS](/images/coding-interview/bfs.png)

**1) 定义图的节点**

{% highlight ruby %}
class GraphNode {
    int val;
    GraphNode next;
    GraphNode[] neighbors;
    boolean visited;
 
    GraphNode(int x) {
        val = x;
    }
 
	GraphNode(int x, GraphNode[] n) {
        val = x;
        neighbors = n;
    }
 
    public String toString() {
        return"value: " + this.val;
    }
}
{% endhighlight %}

**2) 定义队列**

{% highlight ruby %}
class Queue {
    GraphNode first, last;
 
    public void enqueue(GraphNode n) {
        if(first ==null) {
            first = n;
            last = first;
        } else {
            last.next= n;
            last = n;
        }
    }
 
    public GraphNode dequeue() {
        if(first == null) {
            returnnull;
        } else {
            GraphNode temp = new GraphNode(first.val, first.neighbors);
            first = first.next;
            return temp;
        }
    }
}
{% endhighlight %}

**3) 使用队列进行BFS**

{% highlight ruby %}
public class GraphTest {
 
    public static void main(String[] args) {
        GraphNode n1 =new GraphNode(1); 
        GraphNode n2 =new GraphNode(2); 
        GraphNode n3 =new GraphNode(3); 
        GraphNode n4 =new GraphNode(4); 
        GraphNode n5 =new GraphNode(5); 
 
        n1.neighbors=new GraphNode[]{n2,n3,n5};
        n2.neighbors=new GraphNode[]{n1,n4};
        n3.neighbors=new GraphNode[]{n1,n4,n5};
        n4.neighbors=new GraphNode[]{n2,n3,n5};
        n5.neighbors=new GraphNode[]{n1,n3,n4};
 
        breathFirstSearch(n1, 5);
    }
 
    public static void breathFirstSearch(GraphNode root, int x) {
        if(root.val== x)
            System.out.println("find in root");

        Queue queue =new Queue();
        root.visited=true;
        queue.enqueue(root);

        while(queue.first != null) {
            GraphNode c =(GraphNode) queue.dequeue();
            for(GraphNode n : c.neighbors) {
                if(!n.visited) {
                    System.out.print(n +" ");
                    n.visited=true;
                    if(n.val== x)
                        System.out.println("Find "+n);
                    queue.enqueue(n);
                }
            }
        }
    }
}
{% endhighlight %}

**输出：**

    value: 2 value: 3 value: 5 Find value: 5
    value: 4

经典问题如下：<br/>
[1) Clone Graph](http://www.programcreek.com/2012/12/leetcode-clone-graph-java/) <br/>
[2) Course Schedule (BFS/DFS)](http://www.programcreek.com/2014/05/leetcode-course-schedule-java/) <br/>
[2) Course Schedule II (BFS)](http://www.programcreek.com/2014/06/leetcode-course-schedule-ii-java/) <br/>

<h2 id="6">6. Sorting</h2>

以下列出若干排序算法的时间复杂度。你可以自行了解排序算法的具体概念。

算法 | 平均时间 | 最差情况 | 所需空间
:--- | :--- | :--- | :---
冒泡排序 | n^2 | n^2 | 1
选择排序 | n^2 | n^2 | 1
插入排序 | n^2 | n^2 |
快速排序 | n\*log(n) | n^2 |
合并排序 | n\*log(n) | n\*log(n) | 看情况

> 说明：**桶排序**、**基数排序**和**计数排序** 基于与上述排序方法不同的前提，它们属于“非一般”的排序算法，故没有罗列。

以下是一些实现方式和示例程序。另外，有兴趣的话可以参考 "[Java developers sort in practice](http://www.programcreek.com/2014/03/how-developers-sort-in-java/)"。<br/>

[1) Mergesort](http://www.programcreek.com/2012/11/leetcode-solution-merge-sort-linkedlist-in-java/) <br/>
[2) Quicksort](http://www.programcreek.com/2012/11/quicksort-array-in-java/) <br/>
[3) InsertionSort](http://www.programcreek.com/2012/11/leetcode-solution-sort-a-linked-list-using-insertion-sort-in-java/) <br/>
[4) Maximum Gap (Bucket Sort)](http://www.programcreek.com/2014/03/leetcode-maximum-gap-java/) <br/>
[5) First Missing Positive (Bucket Sort)](http://www.programcreek.com/2014/05/leetcode-first-missing-positive-java/) <br/>
[6) Sort Colors (Counting Sort)](6) Sort Colors (Counting Sort)) <br/>

<h2 id="7">7. Recursion / Iteration</h2>

对程序员来说，递归是应该具有的思想（a built-in thought）。

> 问题：有n步台阶，一次只能上1步或2步，问共有多少种走法。

**Step 1: 找出走完前n步台阶和前n-1步台阶之间的关系**

为了走完n步台阶，只有两种方法：从n-1步台阶爬1步走到或从n-2步台阶处爬2步走到。如果`f(n)`是爬到第n步台阶的方法数，那么`f(n) = f(n-1) + f(n-2)`

**Step 2: 确保开始条件是正确的**

    f(0) = 0;
    f(1) = 1;

**函数如下：**

{% highlight ruby %}
public static int f(int n) {
    if(n <=2)
        return n;
    int x = f(n-1)+ f(n-2);

    return x;
}
{% endhighlight %}

递归方法的时间复杂度是 n 的指数级，递归过程如下：

    f(5)
    f(4) + f(3)
    f(3) + f(2) + f(2) + f(1)
    f(2) + f(1) + f(2) + f(2) + f(1)

**将递归转换为迭代：**

{% highlight ruby %}
public static int f(int n) {
    if(n <= 2) {
        return n;
    }
 
    int first = 1, second = 2;
    int third = 0;
 
    for(int i = 3; i <= n; i++) {
        third = first + second;
        first = second;
        second = third;
    }
 
    return third;
}
{% endhighlight %}

<h2 id="8">8. Dynamic Programming</h2>

**动态规划用来解决具有如下属性的问题：**

1. 一个问题可以通过更小子问题的解决方法来解决。
2. 有些子问题的解可能需要计算多次。
3. 子问题的解存储在一张表格里，这样每个子问题只用计算一次。
4. 需要额外的空间以节省时间。

[递归和迭代章节](#7) 的走台阶问题，符合以上4条特性，可以使用动态规划来解决。

{% highlight ruby %}
public static int[] A = new int[100];
 
public static int f3(int n) {
    if(n <= 2)
        A[n] = n;
 
    if(A[n] > 0)
        return A[n];
    else
        A[n]= f3(n-1) + f3(n-2); //store results so only calculate once!

    return A[n];
}
{% endhighlight %}

经典问题如下：<br/>

[1) Edit Distance](http://www.programcreek.com/2013/12/edit-distance-in-java/) <br/>
[1) Distinct Subsequences Total](http://www.programcreek.com/2013/01/leetcode-distinct-subsequences-total-java/) <br/>
[2) Longest Palindromic Substring](http://www.programcreek.com/2013/12/leetcode-solution-of-longest-palindromic-substring-java/) <br/>
[3) Word Break](http://www.programcreek.com/2012/12/leetcode-solution-word-break/) <br/>
[3) Word Break II](http://www.programcreek.com/2014/03/leetcode-word-break-ii-java/) <br/>
[4) Maximum Subarray](http://www.programcreek.com/2013/02/leetcode-maximum-subarray-java/) <br/>
[4) Maximum Product Subarray](http://www.programcreek.com/2014/03/leetcode-maximum-product-subarray-java/) <br/>
[5) Palindrome Partitioning](http://www.programcreek.com/2013/03/leetcode-palindrome-partitioning-java/) <br/>
[5) Palindrome Partitioning II](http://www.programcreek.com/2014/04/leetcode-palindrome-partitioning-ii-java/) <br/>
[6) House Robber [Google]](http://www.programcreek.com/2014/03/leetcode-house-robber-java/) <br/>
[6) House Robber II](http://www.programcreek.com/2014/05/leetcode-house-robber-ii-java/) <br/>
[7) Jump Game](http://www.programcreek.com/2014/03/leetcode-jump-game-java/) <br/>
[7) Jump Game II](http://www.programcreek.com/2014/06/leetcode-jump-game-ii-java/) <br/>
[8) Best Time to Buy and Sell Stock](http://www.programcreek.com/2014/02/leetcode-best-time-to-buy-and-sell-stock-java/) <br/>
[8) Best Time to Buy and Sell Stock II](http://www.programcreek.com/2014/02/leetcode-best-time-to-buy-and-sell-stock-ii-java/) <br/>
[8) Best Time to Buy and Sell Stock III](http://www.programcreek.com/2014/02/leetcode-best-time-to-buy-and-sell-stock-iii-java/) <br/>
[8) Best Time to Buy and Sell Stock IV](http://www.programcreek.com/2014/03/leetcode-best-time-to-buy-and-sell-stock-iv-java/) <br/>
[9) Dungeon Game](http://www.programcreek.com/2014/03/leetcode-dungeon-game-java/) <br/>
[10) Minimum Path Sum](http://www.programcreek.com/2014/05/leetcode-minimum-path-sum-java/) <br/>
[11) Unique Paths](http://www.programcreek.com/2014/05/leetcode-unique-paths-java/) <br/>
[12) Decode Ways](http://www.programcreek.com/2014/06/leetcode-decode-ways-java/) <br/>

<h2 id="9">9. Bit Manipulation</h2>

**位操作符：**

OR (`|`) | AND (`&`) | XOR (`^`) | Left Shift (`<<`) | Right Shift (`>>`) | Not (`~`)
`1|0 = 1` | `1&0 = 0` | `1^0 = 1` | `0010<<2 = 1000` | `1100>>2 = 0010` | `~1 = 0`

> 问题：给定一个正整数n，求它的第 i 位。（i 从0开始，并从右往左计）

{% highlight ruby %}
public static boolean getBit(int num, int i) {
    int result = num &(1<<i);
 
    if(result ==0) {
        return false;
    } else {
        return true;
    }
}
{% endhighlight %}

例如，获取 10 的第 2 位的过程如下：

    i=1, n=10
    1<<1= 10 1010&10=10 10 is not 0, so return true;

经典问题如下：<br/>

[1) Single Number](http://www.programcreek.com/2012/12/leetcode-solution-of-single-number-in-java/) <br/>
[1) Single Number II](http://www.programcreek.com/2014/03/leetcode-single-number-ii-java/) <br/>
[2) Maximum Binary Gap](http://www.programcreek.com/2013/02/twitter-codility-problem-max-binary-gap/) <br/>
[3) Number of 1 Bits](http://www.programcreek.com/2014/03/leetcode-number-of-1-bits-java/) <br/>
[4) Reverse Bits](http://www.programcreek.com/2014/03/leetcode-reverse-bits-java/) <br/>
[5) Repeated DNA Sequences](http://www.programcreek.com/2014/03/leetcode-repeated-dna-sequences-java/) <br/>
[6) Bitwise AND of Numbers Range](http://www.programcreek.com/2014/04/leetcode-bitwise-and-of-numbers-range-java/) <br/>
[7) Power of Two](http://www.programcreek.com/2014/07/leetcode-power-of-two-java/) <br/>

<h2 id="10">10. Combinations and Permutations</h2>

组合和排列的区别在于，顺序是否要考虑在内。

比如下面两个问题：

> 问题1：给出1，2，3，4 和 5，打印出这5个数的不同序列。要求：4 不能是第三个；3 和 5 不能相邻。问有多少种不同的组合？
> 
> 问题2： 有5个香蕉，4个梨和3个苹果，假设同一种类的水果都是相同的，请问有多少种不同的组合？

经典问题如下：<br/>

[1) Permutations](http://www.programcreek.com/2013/02/leetcode-permutations-java/) <br/>
[2) Permutations II](http://www.programcreek.com/2013/02/leetcode-permutations-ii-java/) <br/>
[3) Permutation Sequence](http://www.programcreek.com/2013/02/leetcode-permutation-sequence-java/) <br/>
[4) Generate Parentheses](http://www.programcreek.com/2014/01/leetcode-generate-parentheses-java/) <br/>
[5) Combination Sum (DFS)](http://www.programcreek.com/2014/02/leetcode-combination-sum-java/) <br/>
[5) Combination Sum II (DFS)](http://www.programcreek.com/2014/04/leetcode-combination-sum-ii-java/) <br/>
[5) Combination Sum III (DFS)](http://www.programcreek.com/2014/05/leetcode-combination-sum-iii-java/) <br/>
[6) Combinations (DFS)](http://www.programcreek.com/2014/03/leetcode-combinations-java/) <br/>
[7) Letter Combinations of a Phone Number (DFS)](http://www.programcreek.com/2014/04/leetcode-letter-combinations-of-a-phone-number-java/) <br/>
[8) Restore IP Addresses](http://www.programcreek.com/2014/06/leetcode-restore-ip-addresses-java/) <br/>

<h2 id="11">11. Math Problems</h2>

解决数学问题，通常需要观察问题，然后总结出规律。

经典问题如下：<br/>

[1) Reverse Integer](http://www.programcreek.com/2012/12/leetcode-reverse-integer/) <br/>
[2) Palindrome Number](http://www.programcreek.com/2013/02/leetcode-palindrome-number-java/) <br/>
[3) Pow(x,n)](http://www.programcreek.com/2012/12/leetcode-powx-n/) <br/>
[4) Subsets](http://www.programcreek.com/2013/01/leetcode-subsets-java/) <br/>
[5) Subsets II](http://www.programcreek.com/2013/01/leetcode-subsets-ii-java/) <br/>
[6) Fraction to Recurring Decimal [Google]](http://www.programcreek.com/2014/03/leetcode-fraction-to-recurring-decimal-java/) <br/>
[7) Excel Sheet Column Number](http://www.programcreek.com/2014/03/leetcode-excel-sheet-column-number-java/) <br/>
[8) Excel Sheet Column Title](http://www.programcreek.com/2014/03/leetcode-excel-sheet-column-title-java/) <br/>
[9) Factorial Trailing Zeroes](http://www.programcreek.com/2014/04/leetcode-factorial-trailing-zeroes-java/) <br/>
[10) Happy Number](http://www.programcreek.com/2014/04/leetcode-happy-number-java/) <br/>
[11) Count Primes](http://www.programcreek.com/2014/04/leetcode-count-primes-java/) <br/>
[12) Plus One](http://www.programcreek.com/2014/05/leetcode-plus-one-java/) <br/>
[13) Divide Two Integers](http://www.programcreek.com/2014/05/leetcode-divide-two-integers-java/) <br/>
[14) Multiply Strings](http://www.programcreek.com/2014/05/leetcode-multiply-strings-java/) <br/>
[15) Max Points on a Line](http://www.programcreek.com/2014/04/leetcode-max-points-on-a-line-java/) <br/>
[16) Product of Array Except Self](http://www.programcreek.com/2014/07/leetcode-product-of-array-except-self-java/) <br/>

<br/>

2/6/2016 10:16:30 PM 
