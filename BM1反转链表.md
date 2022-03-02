## 1.链表系列

#### BM1.[反转链表](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=296&sfm=top101)

**题目分析**

翻转链表这个题目，我建议第一个刷，最基本的链表写得滚瓜烂熟，因为我们后面要使用这个翻转链表函数。其实很多题目拆解开来之后你会发现都是在解决局域小问题。

![alt](https://uploadfiles.nowcoder.com/images/20211001/397721558_1633084777359/E53A90674EDC6B8D31549D8DF4E7B38E)

**做法分解**

翻转链表肯定会有一个pre遍历连接节点，最后pre节点被赋值到最后一个不为空的cur节点。作为翻转值

```java
public ListNode ReverseList(ListNode head) {
    if(head == null)
    return head;
    ListNode pre = null;
    ListNode cur = head;
    
    return pre;
}
```

循环过程中因为要进行翻转，改变cur.next所以要提前记录cur.next，然后进行翻转，然后移动赋值指针。其实有点像交换函数swap。不妨先写一个交换值的swap函数。

```java
while(cur != null){
    ListNode next = cur.next;
    cur.next = pre;
    pre = cur;
    cur = next;
}
```


完整题解
```java
 public ListNode ReverseList(ListNode head) {
      if(head == null)
    return head;
  ListNode pre = null;
  ListNode cur = head;
  while(cur != null){
    ListNode next = cur.next;
    cur.next = pre;
    pre = cur;
    cur = next;
}
  return pre;
    }
```


**复杂度分析：**

- 时间复杂度：$O(n)$，$n$为链表的长度，一次遍历
- 空间复杂度：$O(1)$，没有使用新的额外空间