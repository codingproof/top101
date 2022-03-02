![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM8. [链表中倒数最后k个结点](https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/886370fe658f41b498d40fb34ae76ff9?tpId=295&sfm=github&channel=nowcoder



**题目分析**

这个题目也是一个快慢指针问题，就是先让快指针走k步，然后快指针拉着慢指针一起走，需要判断k是否会超出数组的长度。

![alt](https://uploadfiles.nowcoder.com/images/20211026/397721558_1635226738569/411BAC4824C77353B52BB1434925F792)



**做法分析**

定义快指针fast，和慢指针slow，快指针先走K步

```java
ListNode fast = pHead;
ListNode slow = pHead;
while(k > 0 && fast != null){
    fast =  fast.next;
    k--;
}
```

额外判断一种情况，就是给出的k是大于链表长度，这样fast已经是null，但是k不为0，所以直接返回null即可

```java
if(fast == null && k != 0)
    return null;
```

最后快慢指针同时向后遍历，那么最后的慢指针就应该是链表中倒数第K个节点

```java
while(fast != null){
    fast = fast.next;
    slow = slow.next;
}
return slow;
```


**完整题解**
```java
  public ListNode FindKthToTail(ListNode pHead, int k) {
    ListNode fast = pHead;
    ListNode slow = pHead;
    while (k > 0 && fast != null) {
      fast = fast.next;
      k--;
    }
    if (fast == null && k != 0)
      return null;
    while (fast != null) {
      fast = fast.next;
      slow = slow.next;
    }
    return slow;
  }

```
**复杂度分析**
时间复杂度：$O(n)$， slow指针走过的距离不会超过链表的总长度，所以时间复杂度是

空间复杂度：$O(1)$没有使用额外的空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)
