![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM46. [最小的K个数](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=295&sfm=github&channel=nowcoder

题目的主要信息：

- 对于一个给定无序数组，返回最小的k个元素，顺序由小到大
- k和数组有特殊情况需要单独讨论

**方法一：sort排序法**

**具体做法：**

这是最能想到，也是最简单的方法。利用sort函数对数组进行由小到大排序，然后取前k个值入vector即可。
```
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        if(k == 0 || input.size() == 0) //排除特殊情况
            return res;
        sort(input.begin(), input.end()); //排序
        for(int i = 0; i < k; i++){ //因为k<=input.length,取前k小
            res.push_back(input[i]);
        }
        return res;
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(nlog_2n)$，sort函数属于快排
- 空间复杂度：$O(1)$，无额外空间使用


**方法二：堆排序**

**具体做法：**

利用input数组中前k个元素，构建一个大小为k的大顶堆，对于后续的元素，依次比较其与堆顶的大小，若是比堆顶小，则堆顶弹出，再将新数加入堆中，直至数组结束。最后将堆顶依次弹出即是最小的k个数。
![图片说明](https://uploadfiles.nowcoder.com/images/20210722/397721558_1626945012109/6A105C4B5BE11C9FE59934C5B4E772BF "图片标题") 
```
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        if(k == 0 || input.size() == 0) //排除特殊情况
            return res;
        priority_queue<int> q;  
        for(int i = 0; i < k; i++)//构建一个k个大小的堆
            q.push(input[i]);
        for(int i = k; i < input.size(); i++){
            if(q.top() > input[i]){  //较小元素入堆
                q.pop();
                q.push(input[i]);
            }
        }
        for(int i = 0; i < k; i++){ //堆中元素取出入vector
            res.push_back(q.top());
            q.pop();
        }
        return res;
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(nlog_2k)$，构建和维护大小为k的堆，需要$log_2k$，加上遍历整个数组
- 空间复杂度：$O(k)$，堆空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)