---
layout: post
title:  "树的相关内容总结"
date:   2016-08-16 07:38:20
categories: ACM
author: smartjinyu
image: /img/2016-08-16/dsa-spring2015.jpg
excerpt: BinTree、BST、BBST(AVL、Splay、RedBlack)、BTree的相关内容总结
analytics: true
---


一般研究时只研究二叉树，因为任何有序多叉树可以等价转化为二叉树

知识结构：

![struct](\img\2016-08-16\struct.png)



#### 1、二叉树 BinTree

知识点：先序遍历、中序遍历、后序遍历、层次遍历的实现（递归版、迭代版）



#### 2、二叉搜索树 BST

满足在中序遍历意义下的序列单调非降，可以进行二分搜索。BST的平均树高为n^(1/2)级别，最坏情况退化为线性序列，树高为n。

为此，引入适度平衡的概念，将 BST 等价转化为平衡二叉搜索树BBST，使得树高在渐进意义下为 logn 级别。

注：BST 的等价： 中序遍历序列相同

知识点：难度不高，但请注意remove()及removeAt()操作的实现和接口语义



#### 3、平衡二叉搜索树 BBST

适度平衡：树高h在渐进意义下为 logn 级别的 BST。

局部性：

1. 经过单次动态修改操作后，至多只有O(1)处局部不再满足限制条件
2. 总可在O(logn)的时间内，使得这O(1)的局部和全树重新满足限制条件


##### 3.1、AVL树 AVL Tree

适度平衡（定义）：任意节点平衡因子（左右子树的高度差）绝对值不超过1

知识点：插入、删除操作后需要重平衡，其中删除操作可能导致失衡传播。共分为zig-zig、zig-zag、zag-zig、zag-zag四种情况进行讨论，利用3+4重构实现。

主要缺陷：单次动态调整（删除）后，全树拓扑结构的变化量可能高达 logn 级别，频繁地插入删除未免得不偿失。


##### 3.2、伸展树 Splay Tree

基于局部性原理构造，即刚被访问的元素及其附近元素，极有可能在不久之后再次被访问
        
定义：无须时刻保持全树的平衡，在足够长的真实操作序列中，能够保持分摊意义上O(logn)级别的速度

实现：每次访问一个节点后，将其提升至树根处。用双层伸展策略实现。其中zig-zag和zag-zig与AVL树完全一致，即逐层进行，先旋转父亲节点p，再旋转祖父节点g；zig-zag和zag-zig则不同，先旋转g，再旋转p。可以证明的是，使用双层伸展策略，可使每次被提升的节点v的对应分支长度以几何级数（大致一半）的速度收缩。

知识点：

1. 以上伸展算法的代码实现，参考3+4重构的思路，真正实现时只考虑结果，直接修         改对应节点、子树的孩子、父亲而不必真实地进行两次旋转
2. search()不再是静态操作，需要将最后一个被访问节点提升至树根
3. insert、remove接口的实现，难度不大


优势：局部性强，当访问的内容局部性强时，效率很高 O(logk) 级别

主要缺陷： 尽管分摊复杂度为O(logn)，但是无法避免单次最坏情况O(n)的出现，不适用于对单次效率极其敏感的场合


##### 3.3 红黑树 RedBlack Tree

适度平衡：任一节点的左右子树高度，相差不超过两倍

定义：

1. 树根始终为黑色
2. 外部节点始终为黑色
3. 其余节点若为红色，则其孩子节点必为黑色（等价于：任何一个通路都不含相的左右节点）
4. 从任一外部节点到根节点的沿途，黑节点的数目相等

等价：当把红孩子提升至与黑父亲同一高度，将他们视作同一超级节点时，红黑树与(2,4)-BTree等价，此时的B树每个节点有且仅有一个黑关键码，同时红色关键码不超过两个，若存在两个红色关键码，则黑色关键码必然居中。从B树的角度去理解红黑树的相关算法，会比较容易。

平衡性： 可以证明，树高h满足: log2(n+1) <= h <= 2*log2(n+1)   

知识点： 

1. 插入算法，并解决因插入新节点导致的父子节点均为红色的问题（双红缺陷） ，参考BTree因插入导致上溢的解决方案，依据x的uncle（x的祖父的另一个孩子）颜色分类
2. 删除算法，解决因删除节点导致的父子节点均为黑色的问题（双黑缺陷），参考BTree因删除导致下溢的解决方案
（删除较为复杂，无论是确定需要解决双黑缺陷的时机，还是双黑缺陷发生后针对不同情况的处理，讨论较为繁琐，联系BTree可能稍简单一点）

优势：持久性 Persistence（不懂自己在说什么）


#### 4、 B树 BTree

（多路平衡搜索树的另一分支，KD-Tree本次学习时跳过）

基于分级存储的理论，我们宁愿对内存进行多次访问，也要尽量渐少对外存的访问（I/O操作）。数据规模的增长速度快于内存空间的增长速度，而多路搜索树可以在分级存储时取得较好的性能。

外存的特点：每次访问耗时极长（相对于访问内存），但批量读取的速度与读取单个关键码相差无几。

可以将二叉搜索树在中序遍历的意义下等价地转化为2^k路搜索树。

以下讨论m阶B树：

定义：设每个内部节点拥有n个关键码，则必然拥有n+1个指示其子树的引用。满足以下条件：（此处[m/2]定义为m/2向上取整）

[m/2] <= n+1 <= m

由于各节点的分支数介于[m/2]和m之间，故m阶B树又称为([m/2],m)-树

高度h： 可以证明,拥有N个关键码的m阶B树的高度h满足：(讨论最瘦和最胖两个极端情况即可)
            logm(N+1) <= h <= log【m/2】([N+1]/2) + 1

复杂度：每次查找操作，耗时在O(logm(N))级别，但是相比BST，I/O操作减少到原先的 1/log2(m)

知识点：

1. 需重新实现BTNode节点类，重新实现search接口（难度不大，注意接口语义）
2. 插入算法，注意因插入导致的上溢处理。将发生上溢的节点分裂，中点归入上层节点，可能导致上溢的传播
3. 删除算法，注意因删除导致的下溢处理。分情况讨论，向左/右兄弟借关键码；或必须与兄弟合并，可能导致下溢的传播
4. 插入删除算法最坏可能导致 logm(N) 级别的分裂/合并，但在平均意义下，单次动态操作只需常数次节点的分裂\合并，其复杂度来源主要是确定位置时的search()操作


#### 说明
数据结构部分主要依据的教材是《数据结构（C++语言版）》 （邓俊辉编著，清华大学出版社2013年9月第三版），同时结合了邓老师在学堂在线上的同名MOOC课程进行学习。

基本上把书上的代码都实现了一遍，放在[我的Github仓库]里,有兴趣的朋友可以前往查看。

不过因为时间和精力的关系，部分代码没有经过调试。如果需要相关代码，建议访问[清华大学出版社网站]，获取完整的教材勘误表、示例代码和测试样例。



[我的Github仓库]:https://github.com/smartjinyu/DSA-djh
[清华大学出版社网站]:http://dsa.cs.tsinghua.edu.cn/~deng/ds/dsacpp/