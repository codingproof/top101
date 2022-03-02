![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM47. [寻找第K大](https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/e016ad9b7f0b45048c58a9f27ba618bf?tpId=295&sfm=github&channel=nowcoder

题目中给到的信息：
- 利用快速排序的思想
- 有重复数字，不用去重，也不用管稳定性与否

**方法一：重载sort**
因为sort使用的是快速排序，因此这种方法**勉强**算是利用了快排思想。
这次要寻找第K大，sort函数默认递增，因此需要将其重载为递减，然后遍历到第k个。
```c++
class Solution {
public:
    static bool comp(int a, int b){  //重载为递减
        if(a > b)
            return true;
        else
            return false;
    }
    int findKth(vector<int> a, int n, int K) {
        sort(a.begin(), a.end(), comp);//sort函数为快排
        return a[K - 1];
    }
};
```

**复杂度分析：**
- 时间复杂度：O(nlgn)，比较排序最低复杂度为O(nlgn)
- 空间复杂度：O(n)，递归栈的最大深度

**方法二：快排+二分法**
快速排序，每次移动，可以找到一个标杆元素，然后将大于它的移到左边，小于它的移到右边，如果它左边刚好有K-1个比它大的，那么该元素就是第K大，如果它左边的元素比K - 1多，说明第K大在其左边，直接二分不用管标杆元素右边，同理如果它左边的元素比K-1少，那第K大在其右边，左边不用管。
**具体做法：**
- 进行一次快排，大元素在左，小元素在右，得到的中轴p点
- 如果 p - low + 1 = k ，那么p点就是第K大
- 如果 p - low + 1 > k，则第k大的元素在左半段，更新high = p - 1，执行第一步
- 如果 p - low + 1 < k，则第k大的元素在右半段，更新low = p + 1, 且 k = k - (p - low + 1)，排除掉前面部分更大的元素，再执行第一步
![图片说明](https://uploadfiles.nowcoder.com/images/20210716/397721558_1626437471524/586E508F161F26CE94633729AC56C602 "图片标题") 



```c++
class Solution {
public:
    int partion(vector<int>& a, int low, int high){ //常规的快排划分，但这次是大数在左
        int temp = a[low];
        while(low < high){
            while(low < high && a[high] <= temp)
                high--;
            if(low == high)
                break;
            else
                a[low] = a[high];
            while(low < high && a[low] >= temp)
                low++;
            if(low == high)
                break;
            else
                a[high] = a[low];
        }
        a[low] = temp;
        return low;
    }
    int quickSort(vector<int>& a, int low, int high, int K){
        int p = partion(a, low, high); //先进行一轮划分，p下标左边的都比它大，下标右边都比它小
        if(K == p - low + 1)  //若p刚好是第K个点，则找到
            return a[p];
        else if(p - low + 1 > K)  //从头到p超过k个数组，则目标在左边
            return quickSort(a, low, p - 1, K);  //递归左边
        else  
            return quickSort(a, p + 1, high, K - (p - low + 1));  //否则，在右边,递归右边,但是需要减去左边更大的数字的数量
    }
    int findKth(vector<int> a, int n, int K) {
        return quickSort(a, 0, n - 1, K);
    }
};
```

**复杂度分析：**
- 时间复杂度：O(n)，利用二分法缩短了时间——T(2/N)+T(N/4)+T(N/8)+……=T(N)
- 空间复杂度：O(n)，递归栈最大深度

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)