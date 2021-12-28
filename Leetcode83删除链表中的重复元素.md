# Leetcode83|删除链表中的重复元素

### 背景知识

```python
class ListNode:
    """
    链表节点
    """
    def __init__(self, x):          # 在创建结点时，执行初始化赋值程序
        self.val = x                # 当前结点的值
        self.next = None            # 下一个节点，刚创建时为None
```

## 一、链表简介

链表是一种在存储单元上非连续、非顺序的存储结构。数据元素的逻辑顺序是通过链表中的指针链接次序实现。链表是由一系列的结点组成，结点可以在运行时动态生成。每个结点包含两部分：数据域与指针域。数据域存储数据元素，指针域存储下一结点的指针。

## 二、单向链表

单向链表也叫单链表，是链表中最简单的形式，它的每个节点包含两个域，一个信息域（元素域）和一个链接域。这个链接指向链表中的下一个节点，而最后一个节点的链接域则指向一个空值。

head 保存首地址，item 存储数据，next 指向下一结点地址。

链表失去了序列的随机读取优点，同时链表增加了指针域，空间开销也较大，但它对存储空间的使用要相对灵活。

列如：有一堆数据[1,2,3,5,6,7]，要在3和5之间插入4, 如果用数组，需要将5之后的数据都往后退一位，然后再插入4，这样非常麻烦，但是如果用链表，我就直接在3和5之间插入4就行。

### 1. 定义结点

结点的数据结构为数据元素(item)与 指针(next)

```python3
class Node(object):
    """单链表的结点"""

    def __init__(self, item):
        # item存放数据元素
        self.item = item
        # next是下一个节点的标识
        self.next = None
```

### 2. 定义链表

链表需要具有首地址指针head。

```python
class SingleLinkList(object):
    """单链表"""

    def __init__(self):
        self._head = None
```

```python
if __name__ == '__main__':
    # 创建链表
    link_list = SingleLinkList()
    # 创建结点
    node1 = Node(1)
    node2 = Node(2)

    # 将结点添加到链表
    link_list._head = node1
    # 将第一个结点的next指针指向下一结点
    node1.next = node2

    # 访问链表
    print(link_list._head.item)  # 访问第一个结点数据
    print(link_list._head.next.item)  # 访问第二个结点数据
```

是不是感觉很麻烦，所以我们要在链表中增加操作方法。

- is_empty() 链表是否为空
- length() 链表长度
- items() 获取链表数据迭代器
- add(item) 链表头部添加元素
- append(item) 链表尾部添加元素
- insert(pos, item) 指定位置添加元素
- remove(item) 删除节点
- find(item) 查找节点是否存在

```python
class SingleLinkList(object):
    """单链表"""

    def __init__(self):
        self._head = None

    def is_empty(self):
        """判断链表是否为空"""
        return self._head is None

    def length(self):
        """链表长度"""
        # 初始指针指向head
        cur = self._head
        count = 0
        # 指针指向None 表示到达尾部
        while cur is not None:
            count += 1
            # 指针下移
            cur = cur.next
        return count

    def items(self):
        """遍历链表"""
        # 获取head指针
        cur = self._head
        # 循环遍历
        while cur is not None:
            # 返回生成器
            yield cur.item
            # 指针下移
            cur = cur.next

#yield：到这里你可能就明白yield和return的关系和区别了，带yield的函数是一个生成器，而不是一个函数了，这个生成器有一个函数就是next函数，next就相当于“下一步”生成哪个数，这一次的next开始的地方是接着上一次的next停止的地方执行的，所以调用next的时候，生成器并不会从foo函数的开始执行，只是接着上一步停止的地方开始，然后遇到yield后，return出要生成的数，此步就结束。
            
            
    def add(self, item):
        """向链表头部添加元素"""
        node = Node(item)
        # 新结点指针指向原头部结点
        node.next = self._head
        # 头部结点指针修改为新结点
        self._head = node

    def append(self, item):
        """尾部添加元素"""
        node = Node(item)
        # 先判断是否为空链表
        if self.is_empty():
            # 空链表，_head 指向新结点
            self._head = node
        else:
            # 不是空链表，则找到尾部，将尾部next结点指向新结点
            cur = self._head
            while cur.next is not None:
                cur = cur.next
            cur.next = node

    def insert(self, index, item):
        """指定位置插入元素"""
        # 指定位置在第一个元素之前，在头部插入
        if index <= 0:
            self.add(item)
        # 指定位置超过尾部，在尾部插入
        elif index > (self.length() - 1):
            self.append(item)
        else:
            # 创建元素结点
            node = Node(item)
            cur = self._head
            # 循环到需要插入的位置
            for i in range(index - 1):
                cur = cur.next
            node.next = cur.next
            cur.next = node

    def remove(self, item):
        """删除节点"""
        cur = self._head
        pre = None
        while cur is not None:
            # 找到指定元素
            if cur.item == item:
                # 如果第一个就是删除的节点
                if not pre:
                    # 将头指针指向头节点的后一个节点
                    self._head = cur.next
                else:
                    # 将删除位置前一个节点的next指向删除位置的后一个节点
                    pre.next = cur.next
                return True
            else:
                # 继续按链表后移节点
                pre = cur
                cur = cur.next

    def find(self, item):
        """查找元素是否存在"""
        return item in self.items()
```

```python
if __name__ == '__main__':
    link_list = SingleLinkList()
    # 向链表尾部添加数据
    for i in range(5):
        link_list.append(i)
    # 向头部添加数据
    link_list.add(6)
    # 遍历链表数据
    for i in link_list.items():
        print(i, end='\t')
    # 链表数据插入数据
    link_list.insert(3, 9)
    print('\n', list(link_list.items()))
    # 删除链表数据
    link_list.remove(0)
    # 查找链表数据
    print(link_list.find(4))
```

### 3.环形链表

![img](https://pic1.zhimg.com/80/v2-1e670ebd005b3d00f248aec08e450cc4_720w.jpg)

单向循环链表为单向链表的变种，链表的最后一个next指向链表头，新增一个循环。

```python
class SingleCycleLinkList(object):

    def __init__(self):
        self._head = None

    def is_empty(self):
        """判断链表是否为空"""
        return self._head is None

    def length(self):
        """链表长度"""
        # 链表为空
        if self.is_empty():
            return 0
        # 链表不为空
        count = 1
        cur = self._head
        while cur.next != self._head:
            count += 1
            # 指针下移
            cur = cur.next
        return count

    def items(self):
        """ 遍历链表 """
        # 链表为空
        if self.is_empty():
            return
        # 链表不为空
        cur = self._head
        while cur.next != self._head:
            yield cur.item
            cur = cur.next
        yield cur.item

    def add(self, item):
        """ 头部添加结点"""
        node = Node(item)
        if self.is_empty():  # 为空
            self._head = node
            node.next = self._head
        else:
            # 添加结点指向head
            node.next = self._head
            cur = self._head
            # 移动结点，将末尾的结点指向node
            while cur.next != self._head:
                cur = cur.next
            cur.next = node
        # 修改 head 指向新结点
        self._head = node

    def append(self, item):
        """尾部添加结点"""
        node = Node(item)
        if self.is_empty():  # 为空
            self._head = node
            node.next = self._head
        else:
            # 寻找尾部
            cur = self._head
            while cur.next != self._head:
                cur = cur.next
            # 尾部指针指向新结点
            cur.next = node
            # 新结点指针指向head
            node.next = self._head

    def insert(self, index, item):
        """ 指定位置添加结点"""
        if index <= 0:  # 指定位置小于等于0，头部添加
            self.add(item)
        # 指定位置大于链表长度，尾部添加
        elif index > self.length() - 1:
            self.append(item)
        else:
            node = Node(item)
            cur = self._head
            # 移动到添加结点位置
            for i in range(index - 1):
                cur = cur.next
            # 新结点指针指向旧结点
            node.next = cur.next
            # 旧结点指针 指向 新结点
            cur.next = node

    def remove(self, item):
        """ 删除一个结点 """
        if self.is_empty():
            return
        cur = self._head
        pre = Node
        # 第一个元素为需要删除的元素
        if cur.item == item:
            # 链表不止一个元素
            if cur.next != self._head:
                while cur.next != self._head:
                    cur = cur.next
                # 尾结点指向 头部结点的下一结点
                cur.next = self._head.next
                # 调整头部结点
                self._head = self._head.next
            else:
                # 只有一个元素
                self._head = None
        else:
            # 不是第一个元素
            pre = self._head
            while cur.next != self._head:
                if cur.item == item:
                    # 删除
                    pre.next = cur.next
                    return True
                else:

                    pre = cur  # 记录前一个指针
                    cur = cur.next  # 调整指针位置
        # 当删除元素在末尾
        if cur.item == item:
            pre.next = self._head
            return True

    def find(self, item):
        """ 查找元素是否存在"""
        return item in self.items()


if __name__ == '__main__':
    link_list = SingleCycleLinkList()
    print(link_list.is_empty())
    # 头部添加元素
    for i in range(5):
        link_list.add(i)
    print(list(link_list.items()))
    # 尾部添加元素
    for i in range(6):
        link_list.append(i)
    print(list(link_list.items()))
    # 添加元素
    link_list.insert(3, 45)
    print(list(link_list.items()))
    # 删除元素
    link_list.remove(5)
    print(list(link_list.items()))
    # 元素是否存在
    print(4 in link_list.items())
```

### Solution

```python
 cur = head                          # 定义临时结点
        while cur and cur.next:             # 当前结点和下一个结点有效时
            if cur.val == cur.next.val:     # 如果当前结点的值等于下一个结点的值
                cur.next = cur.next.next    # 去掉下一个结点
            else:
                cur = cur.next              # 否则当前结点后移
        return head                         # 返回头结点
```

