## 图论基础

### 什么是图
图（Graph）是由顶点的有穷非空集合和顶点之间边的集合组成，通常表示为：G(V,E),其中G表示一个图，V表示图G中顶点的集合，E表示图G中边的集合。


### 图的基本性质

1. 线性表中我们把数据元素叫做元素，树种将数据元素叫做结点，在图中数据元素称之为顶点（Vertex）。  
2. 线性表中可以没有数据元素，称之为空表。树种可以没有结点，但在图中必须要有顶点。
3. 线性表中，相邻的数据元素之间具有线性关系，树结构中，相邻两层的节点具有层次关系，而图中，任意两个顶点之间都可以有关系，顶点之间的逻辑关系用边表示，边集可以为空的。

### 图的基本概念

####  无向图

无向边：若顶点Vi到Vj之间的边没有方向，则称这条边为无向边（Edge），用无偶对（Vi,Vj）表示。如果图中任意两个顶点之间的边都是无向边，则称该图为无向图（Undirected Graphs）。

在无向图中，任意连个顶点都存在边，则称为无向完全图。

<![无向完全图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_1.png?raw=true)

#### 有向图
用有序偶< Vi,Vj>来表示，Vi称为弧尾（Tail），Vj称为弧头（Head）。如果图中任意两个顶点之间的边都是有向边，则称该图为有向图（Directed Graphs）。

在有向图中，如果任意两个顶点之间都存在方向互为相反的两条弧，则称该图为有向完全图。

<![有向完全图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_2.png?raw=true)

#### 图的权
有些图的边或弧具有相关的数字，这种与图的边或弧相关的数叫做权。

<![边或弧的权](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_3.png?raw=true)

#### 连通图
在无向图G中，如果从顶点V到顶点V'有路径，则称V和V'是连通的。如果对于图中任意两个顶点Vi,Vj ∈ E，Vi和Vj都是连通的，则称G是连通图(Connected Graph)。

<![连通图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_4.png?raw=true)

#### 度
无向图顶点的边数叫度，有向图顶点的边数叫做出度和入度。

<![度](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_5.png?raw=true)

### 图的数据存储结构
也正由于图的结构比较复杂，任意两个顶点之间都可能存在联系，因此无法以数据元素在内存中的物理位置来表示元素之间的关系，也就是说，图不可能用简单的顺序存储结构来表示。

#### 邻接矩阵
图的邻接矩阵（Adjacency Matrix）存储方式是用两个数组来表示图。一个一维数组存储图中顶点信息，一个二维数组（称为邻接矩阵）存储图中的边或弧的信息。

##### 无向图

<![无向图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_6.png?raw=true)

##### 有向图

<![有向图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_7.png?raw=true)

##### 带权有向图

<![带权有向图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_8.png?raw=true)

##### 邻接矩阵的问题

 一种孩子表示法，将结点存入数组，并对结点的孩子进行链式存储，不管有多少孩子，也不会存在空间浪费问题，这个思路同样适用于图的存储。我们把这种数组与链表相结合的存储方法称为邻接表。有点像 HashTable的感觉。

#### 邻接表

##### 无向图

<![无向图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_9.png?raw=true)

##### 有向图

<![有向图](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_10.png?raw=true)

##### 带权

<![带权](https://github.com/Yang1793/NoteSpaces/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%AE%97%E6%B3%95/picture/graph_11.png?raw=true)

### 图的遍历

#### 深度优先

深度优先遍历也叫深度优先搜索（Depth First Search）。它的遍历规则：不断地沿着顶点的深度方向遍历。顶点的深度方向是指它的邻接点方向。   

具体点，给定图 G=< V,E>，用 visited[i] 表示顶点i的访问情况，则初始情况下所有的visited[i]都为false。假设从顶点 V0 开始遍历，则下一次遍历顶点是 V0 的第一个邻接点 Vi，接着遍历 Vi 的第一个邻接点 Vj，......直到所有的顶点都被访问过。

所谓的第一个是值在某一种数据结构中（邻接矩阵、邻接表），所有的邻接点存储位置最近的，通常指的是下标最小的。

#### 广度优先

 广度优先遍历也叫广度优先搜索（Breadth Firsh Search），它的遍历规则:

1. 先访问完当前的顶点的所有邻接点。  
2. 先访问顶点的邻接点先与后访问顶点的邻接点被访问。


具体点，给定图 G=< V,E>用 visited[i] 表示顶点i的访问情况，则初始情况下所有的visited[i]都为false。假设从顶点 V0 开始遍历，且顶点 V0 的邻接点下表从小到大有 Vi，Vj...Vk。按规则1，接着遍历 Vi 、Vj 和 Vk。接着按规则2，接下来应遍历 Vi的所有邻接点，之后是 Vj 的所有邻接点......最后是 Vk 的所有邻接点。接下来就是递归的过程了...
