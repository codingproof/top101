![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM9. [删除链表的倒数第n个节点](https://www.nowcoder.com/practice/f95dcdafbde44b22a6d741baf71653f6?tpId=295&sfm=github&channel=nowcoder)


**题目分析**

快慢指针+伪节点
题目由链表中的倒数第K个节点找到倒数第K个节点，删除倒数第K个节点，那么就需要知道K-1个节点，这里面是有一个技巧的，在快慢指针遍历的时候，在赋值之前记录一下指针的位置，那么就是pre指针。这块有一个需要花点时间想的问题就是这个pre的节点初始的位置应该在哪里？简历一个伪节点

![alt](https://uploadfiles.nowcoder.com/images/20220220/588579017_1645343328594/9FAE336608E8DA1595A7DFFC08849469)


**做法分析**

方法一

定义快指针fast，和慢指针slow，记录结果的前一个节点也就是pre节点，以及一个伪节点dummy，将伪节点赋值给前置节点pre，这样就可以处理如果倒数第K个节点是第一个节点的情况。然后快指针先走K步

```java
ListNode fast = head;
ListNode slow = head;
ListNode dummy = new ListNode(-1);
dummy.next = head;
ListNode pre = dummy;
while(n > 0 && fast != null){
    fast = fast.next;
    n--;
}
return  dummy.next;
```

~~额外判断一种情况，就是给出的k是大于链表长度，这样fast已经是null，但是k不为0，所以直接返回null即可~~，由于这块题目给了对应的限制就是k肯定是有效的，所以可以暂且废弃这块的逻辑

最后快慢指针同时向后遍历，同时记录slow节点的前置节点也就是pre节点

```java
while(fast != null){
    pre = slow;
    fast = fast.next;
    slow = slow.next;
}
```

删除倒数第K个节点也就是将第K-1个节点和K+1节点直接进行连接即可

```java
pre.next = pre.next.next;
```


**完整题解**

```java
  public ListNode removeNthFromEnd (ListNode head, int n) {
    ListNode fast = head;
    ListNode slow = head;
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode pre = dummy;
    while(n > 0 && fast != null){
      fast = fast.next;
      n--;
    }
    while(fast != null){
      pre = slow;
      fast = fast.next;
      slow = slow.next;
    }
    pre.next = pre.next.next;
    return  dummy.next;
  }
```



方法二

上面的代码其实不是很清晰，尤其是pre节点的设置，所以这块我们不妨换一个更简单的思路，新建一个链表，然后先链接[0,k-1]，然后再链接[k+1, n]

```java
public ListNode removeNthFromEnd (ListNode head, int n) {
    ListNode fast = head;
    ListNode slow = head;
    //新开一个链表
    ListNode tail = new ListNode(-1);
    ListNode dummy = tail;
    for(int i = 0; i < n; i++){
    fast = fast.next;
    }
    while(fast != null){
    tail.next = slow;
    tail = tail.next;
    fast = fast.next;
    slow = slow.next;
    }
    //连接[k+1, n]
    tail.next = slow.next;
    return dummy.next;
}
```


**复杂度分析**
时间复杂度：$O(n)$， slow指针走过的距离不会超过链表的总长度，所以时间复杂度是
空间复杂度：$O(1)$，没有使用额外的空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)