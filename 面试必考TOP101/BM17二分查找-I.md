![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM17. [二分查找-I](https://www.nowcoder.com/practice/d3df40bd23594118b57554129cadf47b?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/d3df40bd23594118b57554129cadf47b?tpId=295&sfm=github&channel=nowcoder

题目的主要信息：

- 给定一个元素升序的、无重复数字的整型数组 nums 和一个目标值 target 
- 找到目标值的下标
- 如果找不到返回-1
- 进阶要求：时间复杂度$O(log_2n)$ ，空间复杂度$O(1)$

方法一：遍历查找
**具体做法：**

遍历数组，查找到与目标值相等的数组元素，返回其下标。

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size(); i++) //遍历数组
            if(nums[i] == target) //比较找到目标值
                return i;
        return -1; //没找到
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(n)$，遍历一次数组
- 空间复杂度：$O(1)$，无额外空间

方法二：二分法
**具体做法：**

既然数组是升序且无重复的，可以使用二分法，从数组首尾开始，每次比较中点值，如果等于目标即可返回下标，如果中点值大于目标，说明中点以后的都大于目标，因此目标在中点左半区间，如果中点值小于目标，则相反。

![alt](https://uploadfiles.nowcoder.com/images/20211206/397721558_1638791662895/BD9B94E79756B5639D9B9F3F09A66631)
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0;
        int r = nums.size() - 1;
        while(l <= r){ //从数组首尾开始，直到二者相遇
            int m = (l + r) / 2; //每次检查中点的值
            if(nums[m] == target)
                return m;
            if(nums[m] > target) //进入左的区间
                r = m - 1;
            else //进入右区间
                l = m + 1;
        }
        return -1; //未找到
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(log_2n)$，对长度为$n$的数组进行二分，最坏情况就是取2的对数
- 空间复杂度：$O(1)$，无额外空间


方法三：STL二分查找
**具体做法：**

可以直接调用STL的库函数lower_bound进行二分查找，不过查找前要检查数组是否为空。

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) //空数组找不到
            return -1;
        auto iter = lower_bound(nums.begin(), nums.end(), target); //二分查找函数
        if(*iter != target)
            return -1; //没找到
        return iter - nums.begin(); //返回下标
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(log_2n)$， lower_bound的时间复杂度
- 空间复杂度：$O(1)$，无额外空间


![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)