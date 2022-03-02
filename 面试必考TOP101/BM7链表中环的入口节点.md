![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295


#### BM7. [链表中环的入口节点](https://www.nowcoder.com/practice/6e630519bf86480296d0f1c868d425ad?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/6e630519bf86480296d0f1c868d425ad?tpId=295&sfm=github&channel=nowcoder


**题目分析**

上面已经说了快慢指针如何设置那么现在就要引申出，一个相遇公式的推到过程了，这道题目除了使用推导数学公式当然可以使用set存储，遍历过程中存在set里面，那么第一次在set里面重复的元素就是链表中的环入口了


方法一
判断链表中是否有环升级
```java
  public ListNode detectCycle(ListNode head) {
    Set<ListNode> set = new HashSet<>();
    while (head != null) {
      if (set.contains(head))
        return head;
      set.add(head);
      head = head.next;
    }
    return null;
  }
```



方法二
先告诉你结论
环外链表的长度 = 第一次相遇点 + (n-1) * 环的长度
![alt](https://uploadfiles.nowcoder.com/images/20220220/588579017_1645341216726/22921E2E3FAE8F189D658E3C8F442C1E)

   
公式推到过程，c为相遇点，b为入口点

$Lab + Lbc = t$   公式1                       
ab距离加上bc的距离就是一倍速移动的距离 
$Lab + Lbc + n*R = 2t$   公式2      
ab距离加上bc距离在加上
2-1推导出      
$n*R = t$       
$R$ 环长度等于 $Lbc+ Lcb$
​    将1  $ t=Lab + Lbc  R = Lbc+ Lcb $带入           
得到如下         
$n(Lbc+ Lcb) = Lab + Lbc$  
$nLbc + nLcb - Lbc  =  Lab$ 
$Lbc(n-1) + bLcb = Lab$
$Lbc(n-1) + (n-1)Lcb + Lcb = Lab$
$(n-1)(Lcb+Lcb) + Lcb = Lab$
    总结成为一句话就是   环外链表的长度 = 第一次相遇点 + $(n-1)$ * 环的长度

环外链表的长度 = 第一次相遇点 + (n-1) * 环的长度
就是说你将一个头结点放在a点，另一头结点放在b点，按照相同速度遍历，如果两个节点相等，那么就证明该点是环的入口

寻找相遇点，复用判断链表中是否有环，将相遇的节点命名为meetNode，如果meetNode不为空证明存在环的入口

```java
    public ListNode EntryNodeOfLoop(ListNode pHead) {
        if(pHead == null || pHead.next == null)
            return null;
        ListNode fast = pHead;
        ListNode slow = pHead;
        ListNode meetNode = null;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast){
                meetNode = slow;
                break;
            }
        }
        if(meetNode != null){
            
        }
        return null;
    }
```

找到环的相遇点之后，头结点p1放在a点，另一头结点p2放在b点，这块需要注意的是需要先判断是或相等，因为存在只有一个节点的环。

```java
if(meetNode != null){
            ListNode p1= pHead;
            ListNode p2= meetNode;
            while(p1 != null){
                if(p1 == p2)
                    return p1;
                p1 = p1.next;
                p2 = p2.next;
            }
        }

```

**完整代码**
```java
  public ListNode detectCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    ListNode meetNode = null;
    while (fast != null && fast.next != null) {
      slow = slow.next;
      fast = fast.next.next;
      if (fast == slow) {
        meetNode = fast;
        break;
      }
    }
    if (meetNode == null)
      return meetNode;
    while (head != null && meetNode != null) {
      if (meetNode == head)
        return meetNode;
      head = head.next;
      meetNode = meetNode.next;
    }
    return null;
  }
```


**复杂度分析**

时间复杂度：$O(n)$， slow指针走过的距离不会超过链表的总长度；fast指针是slow指针的二倍速

空间复杂度：没有使用额外的空间


![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)