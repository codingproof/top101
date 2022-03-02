![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM6. [判断链表中是否有环](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=295&sfm=github&channel=nowcoder)

题目分析

这个题目其实就是快慢指针问题，一听快慢指针然后就很开心，然后一写就会出问题，不知道循环的开始条件以及终止条件，所以这个题目我建议仔细想想为什么要这样去写循环条件呢？还有一件非常重要的事情就是你要画出一个环，感受一下什么叫有环？

有环就是遍历链表会产生循环遍历


![alt](https://uploadfiles.nowcoder.com/images/20220110/423483716_1641800950920/0710DD5D9C4D4B11A8FA0C06189F9E9C)



方法一
新建一个set存储出现的链表节点，如果set存在重复的节点，证明该节点存在

```java
    public boolean hasCycle(ListNode head) {
        HashSet<ListNode> set = new HashSet<ListNode>();
        while(head != null){
            if(set.contains(head)){
                return true;
            }
            set.add(head);
            head = head.next; 
        }
        return false;
    }
```


方法二

大家经常上来就会写一个快慢指针的问题，那么快指针为什么是2倍速，3倍速可以吗？任意倍速可以吗？

定义快慢指针，记住快慢指针的开始位置都是一样的，慢指针slow是1倍速，快指针fast是2倍速

```java
public boolean hasCycle(ListNode head) {
    //提前判断终止条件，方便后续的判断
        if(head == null || head.next == null)
            return false;
        ListNode slow = head;
        ListNode fast = head;
    }
```

由于fast快指针的速度更快，那么对应的终止条件肯定由fast控制，每次fast指针要向后移动两位，所以fast = fast.next.next，那么fast不能为null，fast.next也是不能为null的

```java
while(fast!=null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
```

那么有环为true的终止条件是什么，快慢指针相等，可以想象有一个人在操场上跑圈无论他什么时候开始跑的，但是他的速度是你的两倍，肯定会追上你的。同样这个同学如果是三倍速度是不依然可以超过你。这个时候我们需要数学抽象出来一个公式，当fast和slow都进入环的时候，那么fast和slow的具体的d就是固定的，fast是2倍速，slow是1倍速，抽象出如下公式，所以d无论是任何距离，都可以通过x变量得到。想一下3倍速可以吗？n倍速可以吗？因为d的最小值=1，

$d = nx - 1x$                  d是间隔节点数，x是移动次数。  代表了倍速

```java
while(fast!=null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            //是否覆写了对应equals方法
            if(slow == fast)
                return true;
        }
```

n倍速的一个写法，n倍速的方式也是可以通过的，证明我们抽象的数学公式还是非常没有问题的，所以可以引出下面的问题，链表中环的入口。

```java
public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null)
            return false;
        ListNode slow = head;
        ListNode fast = head;
        while(fast!=null && fast.next != null && fast.next.next != null && fast.next.next.next != null){
            slow = slow.next;
            fast = fast.next.next.next.next;
            //是否覆写了对应equals方法
            if(slow == fast)
                return true;
        }
        return false;
    }
```


**完整题解**
```java
  public boolean hasCycle(ListNode head) {
    if(head == null || head.next == null)
      return false;
    ListNode slow = head;
    ListNode fast = head;
    while(fast!=null && fast.next != null){
      slow = slow.next;
      fast = fast.next.next;
      if(slow == fast)
        return true;
    }
    return false;
  }
```

**复杂度分析**

时间复杂度：$O(n)$，在最初判断快慢指针是否相遇时，slow 指针走过的距离不会超过链表的总长度；

空间复杂度：$O(1)$，需要额外两个快慢指针来遍历结点。

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)