---
layout: post
title:  "基本算法和数据结构效率总结"
date:   2016-08-21 05:38:20
categories: ACM
author: smartjinyu
image: /img/2016-08-21/dsa-spring2015.jpg
excerpt: 一些基本的数据结构和算法的效率总结，写得比较粗略
analytics: true
---


#### 基本数据结构:

##### 1. 向量 Vector
访问方式: 循秩访问 call by rank

基本操作: insert():O(n) remove():O(n) get():O(1)

查找: 无序向量的顺序查找 find():O(n) 有序向量的二分查找 search():O(logn) 

唯一化: 无序向量 deduplicate:O(n^2) 有序向量 uniquify:O(n)

排序: 见排序算法总结


##### 2. 列表 List
访问方式: 循位置访问 call by poisiton

基本操作: insert():O(1) remove():O(1) get():O(n)

查找: 有序无序均为O(n)

唯一化: 无序列表 deduplicate:O(n^2) 有序列表 uniquify:O(n)

排序: 见排序算法总结


##### 3. 平衡二叉搜索树 BBST
举例: AVL Tree、 Splay Tree、 Red Black Tree

访问方式: 循关键码访问 call by key

局部性: 

1. 单次动态修改操作后，最多只有O(1)的局部不再满足限制条件
2. 总可以在O(logn)的时间内，使得这O(1)处局部以至全树重新满足限制条件

基本操作: search():O(logn) insert:O(logn) remove:O(logn)

详见[树的相关内容总结]


##### 4. 词典 Dictionary
访问方式: 循值访问 call by value

实现: skipList、hashTable

skipList: search(): O(logn) put():O(logn)


##### 5. 优先级队列 Priority_queue
访问方式: 循优先级访问 call by priority

实现: 完全二叉堆、左式堆

- 完全二叉堆 Complete Binary Heap: 
基本操作: getMax():O(1)、insert:O(logn)、delMax():O(logn)、Floyd批量建堆算法:O(n)、
堆合并:O(m+n)

- 左式堆 Lefttist Heap: 
基本操作:堆合并:O(log(max(m,n)))，其他同上

#### 基本算法:

##### 1. 排序算法
- 起泡排序 BubbleSort: O(n^2),不断扫描相邻元素，若逆序则交换
- 归并排序 MergeSort: O(nlogn),分而治之，调用有序向量的二路归并完成排序
- 选择排序 SelectionSort: O(n^2),不断选择未排序部分的最大元素，添加到已排序部分的前端，可优化
- 堆排序 HeapSort: O(nlogn),且常系数意义下优于同级别算法，利用完全二叉堆实现
- 快速排序 quickSort: O(nlogn),最坏O(n^2),代码实现简单，比较常用，主要难点在partition()确定轴点

##### 2. 串匹配

文本长度为n，模式串长度为m

- 蛮力算法 Brutal-Force: 最坏 O(m*n) 字符表较大时平均可达 O(n)
- KMP算法: O(m+n)
- BC 算法: 最坏 O(m*n) 最好 O(n/m)
- BM= BC + GS 算法: 最坏 O(m+n) 最好 O(n/m)

![stringMatch](/img/2016-08-21/string.PNG)

#### 说明
数据结构部分主要依据的教材是《数据结构（C++语言版）》 （邓俊辉编著，清华大学出版社2013年9月第三版），同时结合了邓老师在学堂在线上的同名MOOC课程进行学习。

基本上把书上的代码都实现了一遍，放在[我的Github仓库]里,有兴趣的朋友可以前往查看。

不过因为时间和精力的关系，部分代码没有经过调试。如果需要相关代码，建议访问[清华大学出版社网站]，获取完整的教材勘误表、示例代码和测试样例。


[树的相关内容总结]:http://smartjinyu.com/acm/2016/08/16/Tree-Summary.html
[我的Github仓库]:https://github.com/smartjinyu/DSA-djh
[清华大学出版社网站]:http://dsa.cs.tsinghua.edu.cn/~deng/ds/dsacpp/