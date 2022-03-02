![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

####  BM16. [删除有序链表中重复的元素-II](https://www.nowcoder.com/practice/71cef9f8b5564579bf7ed93fbe0b2024?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/71cef9f8b5564579bf7ed93fbe0b2024?tpId=295&sfm=github&channel=nowcoder

**题目分析**
首先根据这个题目中所有重复的元素都不保留，所以必须要有一个pre节点存在。然后问题分解，逐个击破。本题就会分解成为5个步骤。

![alt](https://uploadfiles.nowcoder.com/images/20211202/397721558_1638444061470/0A01E83A481A4919FAE203E7BB77FDD3)

建立伪节点dummy，所以直接就能确定结果的返回

```java
public ListNode deleteDuplicates (ListNode head) {
    if(head == null)
    return head;
    ListNode dummy = new ListNode(-1);
    
    return dummy.next;
}  
```

然后伪节点的“遍历连接节点”pre用来构建连接结果，另外记住pre的next节点就是head节点，还要一个“遍历当前节点”cur

```java
ListNode pre = dummy;
pre.next = head;
ListNode cur = head;
```

遍历当前节点

```java
while(cur != null){
    
}
```

那么当前的节点就可能是重复的值，判断当前节点是不是重复的值，那么就需要知道当前节点的下一个节点。知道下一个节点之后就可以判断重复，对重复的节点进行一网打尽。打尽之后就进行节点连接，跳过那些重复的值；

```java
int mayRepeatValue = cur.val;
ListNode next = cur.next;
if(next ！= null && next.val == mayRepeatValue){
    while(next != null && next.val == mayRepeatValue){
    next = next.next;
    }
    cur = next;
    pre.next = cur;
}
   ```

不需要一网打尽的，就要对你的遍历指针赋值右移动。这样也是2步骤中为什么要构造pre.next是head的原因

```java
else{
    pre = cur;
    cur = next;
}
```



完整题解 
```java
 public ListNode deleteDuplicates (ListNode head) {
        if(head == null)
            return head;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode cur = head;
        while(cur != null){
            //重复的元素
            int mayRepeatValue = cur.val;
            ListNode next = cur.next;
            if(next != null && next.val == mayRepeatValue){
                //找到一个不重复的next节点
                while(next != null && next.val == mayRepeatValue){
                    next = next.next;
                }
                cur = next;
                pre.next = cur;
            }else{
                pre = cur;
                cur = next;
            }
        }
        return dummy.next;
    }

```


**复杂度分析**

- 时间复杂度：$O(n$，$n$为链表的长度
- 空间复杂度：$O(1)$，没有使用新的额外空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)
