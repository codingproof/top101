![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM4. [合并有序链表](https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/a479a3f0c4554867b35356e0d57cf03d?tpId=295&sfm=github&channel=nowcoder

**题目分析**

如果你会合并有序链表，多个链表合并你已经掌握了80%了，所以完全写熟这个题目，你将会轻松解决合并另一个进阶题目合并k个有序链表

![alt](https://uploadfiles.nowcoder.com/images/20211001/397721558_1633086591658/82953D04639BD2356F6032F90DAF845F)

**做法分析**


方法一

建一个新的伪节点，以及设计一个遍历节点cur

```java
public ListNode mergeTwoLists (ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    
    return dummy.next;
}
```

合并三重奏，1，2全部不空，1不空直接连接1，2不空直接连接2。这块不妨拓展一下合并两个有序的数组

```java
while(l1 != null && l2 != null){
    
}
if(l1 != null){
    cur.next = l1;
}
if(l2 != null){
    cur.next = l2;
}
```

三重奏的内部细节进行填充

```java
while (l1 != null && l2 != null) {
    if (l1.val < l2.val) {
    cur.next = l1;
    l1 = l1.next;
    } else {
    cur.next = l2;
    l2 = l2.next;
    }
    cur = cur.next;
}
if (l1 != null) {
    cur.next = l1;
}
if (l2 != null) {
    cur.next = l2;
}
    
   ```

完整题解

```java
 public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(-1);
    ListNode cur = dummy;
    while (l1 != null && l2 != null) {
      if (l1.val < l2.val) {
        cur.next = l1;
        l1 = l1.next;
      } else {
        cur.next = l2;
        l2 = l2.next;
      }
      cur = cur.next;
    }
    if (l1 != null) {
      cur.next = l1;
    }
    if (l2 != null) {
      cur.next = l2;
    }
    return dummy.next;
  }
```


**复杂度分析**

- 时间复杂度：$O(n)$，$n$为链表1的长度,$m$为链表2的长度
- 空间复杂度：$O(1)$，没有使用新的额外空间



方法二

1. 递归方式写出来合并有序链表，写法不常规，但是算法思想运用的非常的好

   ```java
       public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
           //先判断两者是否为空的情况
        if(l1 == null)
               return l2;
           if(l2 == null)
               return l1;
           ListNode mergeNode = null;
          // 两个需要合并的队列都不为空的情况那需要确定头结点
          if(l1.val < l2.val){
              mergeNode = l1;
              mergeNode.next = mergeTwoLists(l1.next, l2);
          }else{
              mergeNode = l2;
              mergeNode.next = mergeTwoLists(l1, l2.next);
          }
           return mergeNode;
       }
   ```

**复杂度分析**

- 时间复杂度：$O(n)$，$n$为链表1的长度,$m$为链表1的长度，$O(n+m)$所以时间复杂度$O(n)$
- 空间复杂度：$O(n)$，新建一个链表进行返回新的链表长度是$(n+m)

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)