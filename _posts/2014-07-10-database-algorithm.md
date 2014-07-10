---
    layout : default
    title : 浅谈数据库上的一些算法
---

# {{ page.title  }} 


## 随便扯一些题外话

大学里，很多专业都学过数据库这们课程，但是这个课程只是教你怎么使用数据库。
上面也没有将数据库事实现原理，这让很多人摸不着头脑。
曾经我也很困惑，不就是用树来加快搜索嘛，从O(n) 的复杂度降到log(n)的复杂度。
但是我还是有几个问题没想出来。
1.这个树怎么储存的?直接一个节点一个文件吗?
2.有多个查询条件时怎么实现的？难道分开查询?
3.NOSQL　数据库到底是什么意思？难道只是不同的节点的属性可能不同。

好吧，我看了阮一峰的《[数据库的最简单实现](<http://www.ruanyifeng.com/blog/2014/07/database_implementation.html>)》,想要说一些东西了。


## 看阮一峰的那篇文章学到的

### 记录等长

在关系型数据库中，一个很明显的特点就是每条记录都是等长的。
而且每个记录的字段完全相等，且对应的字段也是等长，字段的顺序也相同。

### 查询依靠什么

维基百科上是这样描述关系键的:

```
关系键是关系数据库的重要组成部分。
关系键是一个表中的一个或几个属性，用来标识该表的每一行或与另一个表产生联系。
```

说的很明确，我就不多说什么了。


关系键有主键(primary key), 超键(superkey), 候选键(candidate key),外键(foreign key),代理鍵,自然键。
我们只需要简单的理解有两个主键和外键。

假设我们只会查询主键，那么我们该选择什么样的数据结构才能加快查询呢？
参加过ACM的同胞应该马上可以想到树这个数据结构了。
最常见的是二叉查找树，查找复杂度O(log(n)),即二分查找。

有时候我们可能需要查询的条件不是主键怎么办？
数据库会先对查询的条件建一个索引。

对于索引，可以简单的理解为特殊的主键，或者把主键理解为特殊的索引。
主键的特殊之处是一个主键可以唯一标示一条记录。

建一个索引的意思就是以查询的条件(假设是单一条件)为比较值，建一个查找树，方便快速查找。
新建的这个树会缓存下来。

这也是为什么别人会告诉你，对经常查询的条件加上索引的缘故。


### 数据库用的什么树

数据库的实现没有简单的选择二叉查找树。

原因是我们每查一层，我们需要读取下层的数据，这个读取不是从内存里读取的，是从硬盘里读取的。
这样看来，我们至少需要读取硬盘的次数是O(log(n))次了，这样的代价是很大的，不可接受的。


说起从硬盘起读取数据，不知道大家有没有想起给磁盘文件排序的问题。

对，我们的数据库和磁盘文件排序面临着一样的问题，不能一次性把所有内容读取到内存中，需要多次读取。
这时要考虑的是读取数据的次数了，我们需要尽量减少读取数据的次数。

那我们该如何选取数据结构呢？
这是B树站出来了。

什么是B树呢？

可以理解为多叉查找树，而且每个节点储存多个值。

看看这个图片

![B树样例](http://upload.wikimedia.org/wikipedia/commons/thumb/6/65/B-tree.svg/400px-B-tree.svg.png)

这样，每个节点储存100个值，３层可以储存100W个值了，但是我们只需要读取3次硬盘，换成二叉查找树的话是20层左右,这效率是刚刚的。



## 聊些其他的

其实看了阮一峰的那篇文章，我有种被骗的感觉。
在那篇文章里学到的知识只有一个：数据库的数据结构为什么选择B树。




