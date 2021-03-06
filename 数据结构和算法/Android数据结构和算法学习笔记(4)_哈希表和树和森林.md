# 哈希表

## 特点

1. 数组(顺序表)：寻址容易
2. 链表：插入与删除容易
3. 哈希表：寻址容易，插入删除也容易的数据结构</p>

## 哈希表 Hash table

### 概念
<p>哈希表（Hash table，也叫散列表）
是根据关键码值(Key value)而直接进行访问的数据结构，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。</p>

<p>关键码值(Key value)也可以当成是key的hash值</p>

<p>这个映射函数叫做散列函数</p>

<p>存放记录的数组叫做散列表</p>

### hashtable例子

<p>Key : {14, 19, 5, 7, 21, 1, 13, 0, 18}</br>
散列表: 大小为13 的数组 a[13];</br>
散列函数: f(x) = x mod 13;</p>

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/hashtable.png?raw=true)

## 缺点

<p>扩容需要消费大量的空间和性能</p>

## 应用

<p>电话号码，字典，点歌系统，QQ，微信的好友等</p>

## 设计 拉链法

### jdk1.8以前

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/hashtable1.png?raw=true)

### jdk1.8开始

<p>当链表长度超过阈值，就转换成红黑树</p>

<![](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/hashtable2.png?raw=true)

# 树

<p></p>

# 二叉树

# 二叉树的存储结构

# 二叉树遍历
