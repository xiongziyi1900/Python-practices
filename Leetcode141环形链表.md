# Leetcode141环形链表

### 背景知识

快慢指针：

我们先来简单介绍一下何为快慢指针。快慢指针就是定义两根指针，移动的速度一快一慢，以此来制造出自己想要的差值。**这个差值可以让我们找到链表上相应的节点。**

如果现在对这里的简介不是很理解，没有关系，我们一起来看看如下的几个例子。相信看完后，就会明白了。

##### 判断链表中的环：

还是把链表比作一条跑道，链表中有环，那么这条跑道就是一条圆环跑道，在一条圆环跑道中，两个人有速度差，那么迟早两个人会相遇，只要相遇那么就说明有环。

为了不失一般性，我们在环上加了额外的两个节点，我们可以想象一下，只要两个指针跑进了环里，那么因为存在速度差，他们之间的距离总会由远及近，然后相遇，在远离。像极了我们人世间某些人在你生命中匆匆而过的感觉。

![img](https://upload-images.jianshu.io/upload_images/2527373-5be26cd5e91eca40.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

快慢指针中，因为每一次移动后，快指针都会比慢指针多走一个节点，所以他们之间在进入环状链表后，不论相隔多少个节点，慢指针总会被快指针赶上并且重合，此时就可以判断必定有环。

代码如下：

```python
public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }

        ListNode fast = head;
        ListNode low = head;

        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            low = low.next;

            if (low.equals(fast)) {
                return true;
            }
        }

        return false;
    }

```

