![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM15. [删除有序链表中重复的元素-I](https://www.nowcoder.com/practice/c087914fae584da886a0091e877f2c79?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/c087914fae584da886a0091e877f2c79?tpId=295&sfm=github&channel=nowcoder

**题目分析**

给出的链表为1→1→2,返回1→2

给出的链表为1→1→2→3→3,返回1→2→3.

首先这个题目的意思就是保留一个重复的元素。
动图解析
![alt](https://uploadfiles.nowcoder.com/images/20220220/588579017_1645362070886/CDC2738FBC40BC145056518810388352)


**做法分析**

首先遍历当前节点

```java
public ListNode deleteDuplicates (ListNode head) {
    if(head == null)
    return head;
    ListNode cur = head;
    while(cur != null){
    
    }
}  
```

找到第一个不为空的节点next，然后连接不为空的节点，向右移动遍历节点

```java
ListNode next = cur.next;
while(next != null && next.val == cur.val){
    next = next.next;
}
cur.next = next;
cur = cur.next;
```



完整题解
```java
public ListNode deleteDuplicates (ListNode head) {
    if(head == null)
        return head;
    ListNode cur = head;
    while(cur != null){
        ListNode next = cur.next;
        while(next != null && next.val == cur.val){
            next = next.next;
        }
        cur.next = next;
        cur = cur.next;
    }
    return head;
}
```

**复杂度分析**

- 时间复杂度：$O(n$，$n$为链表的长度
- 空间复杂度：$O(1)$，没有使用新的额外空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)