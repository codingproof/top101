![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM3.[链表中的节点每k个一组翻转](https://www.nowcoder.com/practice/b49c3dc907814e9bbfa8437c251b028e?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/b49c3dc907814e9bbfa8437c251b028e?tpId=295&sfm=github&channel=nowcoder

题目分析
刚才我们已经完全掌握了如何翻转指定区间的链表，现在每K个节点进行翻转，那么是不是可用上我们上面已经写好的函数呢，每K个一组翻转转化为数学的通用公式也就是翻转`[(n-1)*k+1, n*k]`，n代表了是第几组，那么一共能有几组需要求出整个链表的长度。

**这道题目其实是一道hard的题目，但是分拆之后，非常的容易就做出来。**

**分解步骤**

复用链表内指定区间翻转

```java
public ListNode reverseBetween (ListNode head, int m, int n) {
        ListNode dummy = new ListNode(-1);
        int len = n - m;
        dummy.next = head;
        ListNode frontTmp = dummy;
        while(--m > 0){
            frontTmp = frontTmp.next;
        }
        ListNode pre = frontTmp.next;
        ListNode lastTmp = pre;
        ListNode cur = pre.next;
        while(len-- > 0){
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        frontTmp.next = pre;
        lastTmp.next = cur;
        return dummy.next;
    }
```

求链表长度count

```java
public ListNode reverseKGroup (ListNode head, int k) {
        ListNode tail = head;
        int count = 0;
        while(tail != null){
            tail = tail.next;
            count++;
        }
        //分组翻转
        return head;
    }
```

计算有多少分组n，遍历分组n，翻转指定区间`(n-1)*k+1, n*k`，其次注意边界条件可以到达第n组

```java
int n = count / k;
for(int i = 1; i <= n; i++){
    head = reverseBetween(head, (i-1) * k + 1, i * k);
    }
```

完整题解

```java
import java.util.*;
public class Solution {
    public ListNode reverseKGroup (ListNode head, int k) {
        ListNode tail = head;
        int count = 0;
        while(tail != null){
            tail = tail.next;
            count++;
        }
        int n = count / k;
        for(int i = 1; i <=  n; i++){
            //这里面的i代表的是第几个组所以不能
            head = reverseBetween(head, (i-1) * k + 1, i * k);
        }
        return head;
    }
    //翻转指定区间的链表
    public ListNode reverseBetween (ListNode head, int m, int n) {
        ListNode dummy = new ListNode(-1);
        int len = n - m;
        dummy.next = head;
        ListNode frontTmp = dummy;
        while(--m > 0){
            frontTmp = frontTmp.next;
        }
        ListNode pre = frontTmp.next;
        ListNode lastTmp = pre;
        ListNode cur = pre.next;
        while(len-- > 0){
            ListNode next = cur.next;
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

- 时间复杂度：$O(2n)$ - $O(n(n+1)/2)$，n为链表长度，最好翻转一次k=n，最坏1个就翻转n(n+1)/2 + n
- 空间复杂度：$O(1)$，没有使用新的额外空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)