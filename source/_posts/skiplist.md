---
title: 跳表 skiplist 简介
date: 2023-03-16 22:10:33
tags: skiplist
categories: Algorithm
description: 跳表 (Skip List) 是由 William Pugh 在 1990 年发表的文章 Skip Lists: A Probabilistic Alternative toBalanced Trees 中描述的一种查找数据结构，支持对数据的快速查找，插入和删除。
---

## 跳表 skiplist

跳表 (Skip List) 是由 William Pugh 在 1990 年发表的文章 Skip Lists: A Probabilistic Alternative toBalanced Trees 中描述的一种查找数据结构，支持对数据的快速查找，插入和删除。

对于 AVL 树、红黑树等平衡树，在插入过程中需要做多次旋转操作，实现复杂，而跳表实现更简单、开销更低，应用广泛。

![skip list](/img/skiplist/skip_list.png)

跳表是一个包含随机算法的数据结构，跳表的查找、插入、删除的==平均时间复杂度==都是<font color="#2DC26B"> O(logn)</font>，而且可以按照范围区间查找元素，==空间复杂度==是 <font color="#00b050">O(n)</font>。

跳表的最底层通常是一个==有序链表==，上层是下层的一个==索引==，下层元素出现在上层的概率为 p (通常 p=1/2)。

### 跳表的创建过程

首先，对于一个有序链表，如果我们需要查找元素，则需要从头开始遍历链表，查找时间复杂度为 O(n)。如果我们每相邻两个节点增加一个指针，让指针指向下下个节点，那么上层形成了一个减缩版的链表，当我查找元素时，先在上层链表查找，当待查元素在两个节点之间时，再回到下层链表查找，这样我们就可以跳过一些节点，提高查找速度。以此类推，我们可以在第二层链表上建立第三层链表……查找一个元素时，从最高层开始，逐层向下，直到找到为止。

![linklist](/img/skiplist/linklist.png)

上述我们建立的数据结构就是一个跳表，我们可以将上层链表看作是下层链表的一个索引，每相邻两个、三个……建立一个索引，这是一种确定性策略，如果只是查找操作，这么建立跳表没有问题。但是当我们需要频繁插入和删除元素时，这种确定性策略会使维护跳表变得复杂。

为了解决上述问题，我们引入随机策略，不要求上下相邻两层链表之间的节点个数有严格的对应关系，而是为每个节点随机产生一个层数(level)。

```cpp
// 该 randomLevel 方法会随机生成 1~MAX_LEVEL 之间的数，且 ：
//        1/2 的概率返回 1
//        1/4 的概率返回 2
//        1/8 的概率返回 3 以此类推
private int randomLevel() {
  int level = 1;
  // 当 level < MAX_LEVEL，且随机数小于设定的晋升概率时，level + 1
  while (Math.random() < SKIPLIST_P && level < MAX_LEVEL)
    level += 1;
  return level;
}
```

创建跳表我们首先需要一个返回层数的随机函数 randomlevel()，在有序链表的每一个元素上运行该随机函数，当返回 level=1 时，不创建索引；当返回 level=2 时，对该元素创建一级索引；当返回 level=3 时，对该元素创建一级和二级索引……

如果 SKIPLIST_P=1/2 时，一级索引有 n/2 个元素，二级索引有 n/4 个元素……空间复杂度为 O(n)。如果底层链表中的元素存储的是对象，索引只需存储对象排序的键值 key 即可。

### 查找元素

![](/img/skiplist/skip_list_search.png)

查找元素时，从最高层索引开始查找，逐层向下，直到找到为止。根据概率可以证明查找的平均时间复杂度为 O(logn)。

```cpp
V& find(const K& key) {
  SkipListNode<K, V>* p = head;
  // 找到该层最后一个键值小于 key 的节点，然后走向下一层
  for (int i = level; i >= 0; --i) {
    while (p->forward[i]->key < key) {
      p = p->forward[i];
    }
  }
  // 现在是小于，所以还需要再往后走一步
  p = p->forward[0];
  // 成功找到节点
  if (p->key == key) return p->value;
  // 节点不存在，返回 INVALID
  return tail->value;
}
```

### 插入元素

![](/img/skiplist/Skip_list_add_element-en.png)

插入元素的关键是查找元素的合适插入位置，将元素插入链表中之后，运行 randomlevel 函数，确定在该元素上建立几层索引。

```cpp
void insert(const K &key, const V &value) {
  // 用于记录需要修改的节点
  SkipListNode<K, V> *update[MAXL + 1];
  SkipListNode<K, V> *p = head;
  for (int i = level; i >= 0; --i) {
    while (p->forward[i]->key < key) {
      p = p->forward[i];
    }
    // 第 i 层需要修改的节点为 p
    update[i] = p;
  }
  p = p->forward[0];
  // 若已存在则修改
  if (p->key == key) {
    p->value = value;
    return;
  }
  // 获取新节点的最大层数
  int lv = randomLevel();
  if (lv > level) {
    lv = ++level;
    update[lv] = head;
  }
  // 新建节点
  SkipListNode<K, V> *newNode = new SkipListNode<K, V>(key, value, lv);
  // 在第 0~lv 层插入新节点
  for (int i = lv; i >= 0; --i) {
    p = update[i];
    newNode->forward[i] = p->forward[i];
    p->forward[i] = newNode;
  }
  ++length;
}
```

### 删除元素

![](/img/skiplist/skip_list_delete.png)

删除元素的关键同样是查找操作，先找到待删除元素的位置，然后删除对应元素及其索引，删除操作调整对应指针即可。

```cpp
bool erase(const K &key) {
  // 用于记录需要修改的节点
  SkipListNode<K, V> *update[MAXL + 1];
  SkipListNode<K, V> *p = head;
  for (int i = level; i >= 0; --i) {
    while (p->forward[i]->key < key) {
      p = p->forward[i];
    }
    // 第 i 层需要修改的节点为 p
    update[i] = p;
  }
  p = p->forward[0];
  // 节点不存在
  if (p->key != key) return false;
  // 从最底层开始删除
  for (int i = 0; i <= level; ++i) {
    // 如果这层没有 p 删除就完成了
    if (update[i]->forward[i] != p) {
      break;
    }
    // 断开 p 的连接
    update[i]->forward[i] = p->forward[i];
  }
  // 回收空间
  delete p;
  // 删除节点可能导致最大层数减少
  while (level > 0 && head->forward[level] == tail) --level;
  // 跳表长度
  --length;
  return true;
}
```

## 复杂度证明

### 空间复杂度

对于一个节点而言，节点的层数为 $i$ 的概率为 $p^{i-1}(1-p)$，则该节点的期望层数为：
$$
\sum_{i\ge 1,p<1}ip^{i-1}(1-p)=\frac{1}{1-p}
$$
则跳表的期望空间为 $\frac{n}{1-p}$，且因为 $p$ 为常数，所以跳表的**期望空间复杂度**为 O(n)。在最坏的情况下，每一层有序链表等于初始有序链表，即跳表的**最差空间复杂度**为 O(nlogn)。

### 时间复杂度

从后向前分析查找路径，这个过程可以分为从最底层爬到第 $L(n)$ 层和后续操作两个部分。在分析时，假设一个节点的具体信息在它被访问之前是未知的。

假设当前我们处于一个第 $i$ 层的节点 $x$，我们并不知道 $x$ 的最大层数和 $x$ 左侧节点的最大层数，只知道 $x$ 的最大层数至少为 $i$。如果 $x$ 的最大层数大于 $i$，那么下一步应该是向上走，这种情况的概率为 $p$；如果 $x$ 的最大层数等于 $i$，那么下一步应该是向左走，这种情况概率为 $1-p$。

令 $C(i)$ 为在一个无限长度的跳表中向上爬 $i$ 层的期望代价，定义 $C(0)=0$，那么有：
$$
C(i)=(1-p)(1+C(i))+p(1+C(i-1))
$$
解得 $C(i)=\frac{i}{p}$。

由此可以得出：在长度为 $n$ 的跳表中，从最底层爬到第 $L(n)$ 层的期望步数存在上界 $\frac{L(n)-1}{p}$。

现在只需要分析爬到第 $L(n)$ 层后还要再走多少步。易得，到了第 $L(n)$ 层后，向左走的步数不会超过第 $L(n)$ 层及更高层的节点数总和，而这个总和的期望为 $\frac{1}{p}$。所以到了第 $L(n)$ 层后向左走的期望步数存在上界 $\frac{1}{p}$。同理，到了第 $L(n)$ 层后向上走的期望步数存在上界 $\frac{1}{p}$。

所以，跳表查询的期望查找步数为 $\frac{L(n)-1}{p}+\frac{2}{p}$，又因为 $L(n)=log_{\frac{1}{p}}n$，所以跳表查询的**期望时间复杂度**为 O(logn)。

在最坏的情况下，每一层有序链表等于初始有序链表，查找过程相当于对最高层的有序链表进行查询，即跳表查询操作的**最差时间复杂度**为 O(n)。

插入操作和删除操作就是进行一遍查询的过程，途中记录需要修改的节点，最后完成修改。易得每一层至多只需要修改一个节点，又因为跳表期望层数为 $log_{\frac{1}{p}}n$，所以插入和修改的**期望时间复杂度**也为 O(logn)。

## skiplist 与平衡树、哈希表的比较

- skiplist 和各种平衡树（如 AVL、红黑树等）的元素是有序排列的，而哈希表不是有序的。因此，在哈希表上只能做单个 key 的查找，不适宜做范围查找。所谓范围查找，指的是查找那些大小在指定的两个值之间的所有节点。
- 在做范围查找的时候，平衡树比 skiplist 操作要复杂。在平衡树上，我们找到指定范围的小值之后，还需要以中序遍历的顺序继续寻找其它不超过大值的节点。如果不对平衡树进行一定的改造，这里的中序遍历并不容易实现。而在 skiplist 上进行范围查找就非常简单，只需要在找到小值之后，对第 1 层链表进行若干步的遍历就可以实现。
- 平衡树的插入和删除操作可能引发子树的调整，逻辑复杂，而 skiplist 的插入和删除只需要修改相邻节点的指针，操作简单又快速。
- 从内存占用上来说，skiplist 比平衡树更灵活一些。一般来说，平衡树每个节点包含2个指针（分别指向左右子树），而 skiplist 每个节点包含的指针数目平均为 $1/(1-p)$，具体取决于参数 $p$ 的大小。如果像 Redis 里的实现一样，取 $p=1/4$，那么平均每个节点包含 1.33 个指针，比平衡树更有优势。
- 查找单个 key，skiplist 和平衡树的时间复杂度都为 O(logn)，大体相当；而哈希表在保持较低的哈希值冲突概率的前提下，查找时间复杂度接近 O(1)，性能更高一些。所以我们平常使用的各种 Map 或 dictionary 结构，大都是基于哈希表实现的。
- 从算法实现难度上来比较，skiplist 比平衡树要简单得多。

## 参考文献

[1]  Skip Lists: A Probabilistic Alternative toBalanced Trees
[2]  [Skip list - Wikipedia](https://en.wikipedia.org/wiki/Skip_list)
[3]  [Redis内部数据结构详解(6)——skiplist](http://zhangtielei.com/posts/blog-redis-skiplist.html)
[4]  [跳表 - OI Wiki](https://oi-wiki.org/ds/skiplist/)
