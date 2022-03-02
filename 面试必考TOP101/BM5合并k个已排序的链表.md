![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM5. [合并k个已排序的链表](https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&sfm=github&channel=nowcoder)


https://www.nowcoder.com/practice/65cfde9e5b9b4cf2b6bafa5f3ef33fa6?tpId=295&sfm=github&channel=nowcoder

**题目分析**

上一道题目你已经掌握地非常清楚了，在复用合并两个有序链表函数的基础上，给这个题目引入一个归并算法就能轻松解决这道题目。所以这道题目值得多刷几遍，感受一下归并分治的思想

**做法分析**

复用合并两个排序链表函数

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

写分解K个链表的函数，确定区间left,right，确定何时(left==right)返回归并的对象。归并函数写起来非常的简单

```java
public ListNode mergeInternal(ArrayList<ListNode> lists, int left, int right){
        //如果区间已经闭合，返回最小的区间值
        if(left == right){
            return lists.get(left);
        }
        if(left > right){
            return null;
        }
        int mid = left + (right - left) / 2;
        // 合并两个小区间
        return mergeTwoLists(mergeInternal(lists, left, mid), mergeInternal(lists, mid + 1, right));
    }

```

连接主干函数，这种相对比较复杂的题目不要害怕写得函数比较多，只要确保每个函数职责明确就没有问题

```java
public ListNode mergeKLists(ArrayList<ListNode> lists) {
    int left = 0;
    int right = lists.size() - 1;
    return mergeInternal(lists, left, right);
}
```



**完整题解**
```java

 public ListNode mergeKLists(ArrayList<ListNode> lists) {
    int left = 0;
    int right = lists.size() - 1;
    return mergeInternal(lists, left, right);
  }

  public ListNode mergeInternal(ArrayList<ListNode> lists, int left, int right){
    if(left == right){
      return lists.get(left);
    }
    if(left > right){
      return null;
    }
    int mid = left + (right - left) / 2;
    return mergeTwoLists(mergeInternal(lists, left, mid), mergeInternal(lists, mid + 1, right));
  }

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

时间复杂度：$O(n*log(n))$，归并排序的时间复杂度

空间复杂度：$O(n)$需要额外开辟空间返回链表

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)