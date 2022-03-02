![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

####  BM2.[链表内指定区间反转](https://www.nowcoder.com/practice/b58434e200a648c589ca2063f1faf58c?tpId=295&sfm=github&channel=nowcoder)


https://www.nowcoder.com/practice/b58434e200a648c589ca2063f1faf58c?tpId=295&sfm=github&channel=nowcoder



**题目分析**

指定区间翻转链表这个题目是链表中操作比较复杂的那种，主要考察的点就是链表翻转和指针之间的引用。依然拆分为局域问题来进行解答，另外非常推荐大家多刷几遍这道题目，非常有助于大家养成自己的做题思路。

另外这道题目一定要知道自己要做的两件事情是什么，第一件事就是翻转指定区间的链表，第二件事情就是把已经翻转的链表和被切开的链表链接起来即可，想到切开并且连接上，那么一定涉及到四个节点，所以正确的定义和找到这四个节点就非常的重要了。大家可以开始的时候取几个非常好听的名字 。另外不妨先在纸上画一个这个东西

 dummy------frontTmp    pre(记录lastTmp)-cur-------     ----------pre---cur     

另外还要注意链表的题目有一个技巧就是虚拟头结点，比如这道题目如果需要翻转是从头节点开始的话是不是就需要建立一个伪节点，当你有了虚拟节点和涉及的四个节点，你会发现可以非常容易的解决以下题目


![alt](https://uploadfiles.nowcoder.com/images/20211206/397721558_1638760529718/56E0F0741EC36DE1005A0BF1028ADBF3)


**做法分析**

明确返回结果就是第一个节点，但是第一个节点是什么不确定，但是可以用一个伪节点的下一个节点来进行代替，常用的一种处理方式，另外记录需要翻转的次数

```java
public ListNode reverseBetween (ListNode head, int m, int n) {
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    // 需要翻转的次数就是对应的饭庄长度
    int len = n - m;
    //步骤1 找要翻转的开始节点 以及记住要翻转的节点的前置节点
    
    return dummy.next;
}

```

找到要翻转的节点以及记录翻转的前置节点，另外需要注意是从1开始，终止条件为 --m > 0

```java
ListNode frontTmp = dummy;
//为什么要用--m？
while(--m > 0){
    frontTmp = frontTmp.next;
}
```

已经找到了翻转的链表的前置节点，那么要翻转的链表的节点就是frontTmp.next，此时回顾经典翻转链设置pre和cur即可，另外由于翻转完成之后还要连接，所以要记住当前的需要翻转的节点，也就是我们被记为pre的节点，取名lastTmp

```java
ListNode pre = frontTmp.next;
ListNode cur = pre.next;
ListNode lastTmp = pre;       //当前的pre节点会变成翻转之后的末尾节点，也就是连接断开节点的节点
while(len-- > 0){
    ListNode next = cur.next;
    cur.next = pre;
    pre = cur;
    cur = next;
}
```

将翻转好的链表和断开的节点进行连接，frontTmp->pre            lastTmp->cur

```java
fontTmp.next = pre;
lastTmp.next =cur;
```


完整题解

```java
import java.util.*;
public class Solution {
    public ListNode reverseBetween (ListNode head, int m, int n) {
        //构造一个伪装的节点
        ListNode dummy = new ListNode(-1);
        int len = n - m;
        dummy.next = head;
        ListNode frontTmp = dummy;
        //到达M-1就会结束, 也就是要去少一次循环
        while(--m > 0){
            frontTmp = frontTmp.next;
        }
        // pre 和 cur 是反转链表的标准节点 但是pre又要用于和 n+1进行项链所以要保存 临时节点
        ListNode pre = frontTmp.next;
        ListNode lastTmp = pre;
        ListNode cur = pre.next;
        while(len-- > 0){
            //为什么突然也就是看不到对应的链接的内容了，你可以想  2 《----- 3   4 ------5
            // 注意保证一个临时节点     pre ----> cur------>
            ListNode next = cur.next;
            //第一步是反转，然后两步就是赋值
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        frontTmp.next = pre;
        lastTmp.next = cur;
        return dummy.next;
    }
}

```


**复杂度分析：**

- 时间复杂度：$O(n)$，$n$为链表的长度，一次遍历
- 空间复杂度：$O(1)$，没有使用新的额外空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)