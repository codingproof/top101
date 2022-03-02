![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM74. [数字字符串转化成IP地址](https://www.nowcoder.com/practice/ce73540d47374dbe85b3125f57727e1e?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/ce73540d47374dbe85b3125f57727e1e?tpId=295&sfm=github&channel=nowcoder

题目的主要信息：

- 有一个只包含数字的字符串，将该字符串转化成IP地址的形式
- 需要返回所有情况，顺序没有问题

方法一：暴力枚举
**具体做法：**

对于IP字符串，如果只有数字，则相当于需要我们将IP地址的三个点插入字符串中，而第一个点的位置只能在第一个字符、第二个字符、第三个字符之后，而第二个点只能在第一个点后1-3个位置之内，第三个点只能在第二个点后1-3个位置之内，且要要求第三个点后的数字数量不能超过3，因为IP地址每位最多3位数字。

暴力枚举这三个点的位置，使用substr截取出四段数字，比较数字不能大于255，且除了0以外不能有前导0，然后才能组装成IP地址加入答案中。

```cpp
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> res;
        int n = s.length();
        for(int i = 1; i < 4 && i < n - 2; i++){ //遍历IP的点可能的位置（第一个点）
            for(int j = i + 1; j < i + 4 && j < n - 1; j++){ //第二个点的位置
                for(int k = j + 1; k < j + 4 && k < n; k++){ //第三个点的位置
                    if(n - k >= 4) //最后一段剩余数字不能超过3
                        continue; 
                    //从点的位置分段截取
                    string a = s.substr(0, i);
                    string b = s.substr(i, j - i);
                    string c = s.substr(j, k - j);
                    string d = s.substr(k);
                    if(stoi(a) > 255 || stoi(b) > 255 || stoi(c) > 255 || stoi(d) > 255) //IP每个数字不大于255
                        continue;
                    //排除前导0的情况
                    if((a.length() != 1 && a[0] == '0') || (b.length() != 1 && b[0] == '0') ||  (c.length() != 1 && c[0] == '0') || (d.length() != 1 && d[0] == '0'))
                        continue;
                    string temp = a + "." + b + "." + c + "." + d; //组装IP地址
                    res.push_back(temp);
                }
            }
        }
        return res;
    }
};
```

**复杂度分析：**
- 时间复杂度：如果将3看成常数，则复杂度为$O(1)$，如果将3看成字符串长度的1/4，则复杂度为$O(n^3)$，三次嵌套循环
- 空间复杂度：如果将3看成常数，则复杂度为$O(1)$，如果将3看成字符串长度的1/4，则复杂度为$O(n)$，4个记录截取字符串的临时变量。res属于返回必要空间。


方法二：递归+回溯
**具体做法：**

使用step记录分割出的数字个数，index记录递归的下标，结束递归是指step已经为4，且下标到达字符串末尾。

在主体递归中，每次加入一个字符当数字，最多可以加入三个数字，剩余字符串进入递归构造下一个数字，同时要检查每次的数字是否合法（不超过255且没有前导0）.

合法IP需要将其连接，同时递归完这一轮后需要回溯。

![alt](https://uploadfiles.nowcoder.com/images/20211206/397721558_1638758030591/D203C4C85B04AE9F5014A40D38F0534E)

```cpp
class Solution {
private: 
    vector<string> res; //返回答案
    string s; //记录输入字符串
    string nums; //记录分段IP数字字符串
public:
    void dfs(int step, int index){ //step表示第几个数字，index表示字符串下标
        string cur = ""; //当前分割出的字符串
        if(step == 4){ //分割出了四个数字
            if(index != s.length()) //下标必须走到末尾
                return;
            res.push_back(nums);
        }else{
            for(int i = index; i < index + 3 && i < s.length(); i++){ //最长遍历3位
                cur += s[i];
                int num = stoi(cur); //转数字比较
                string temp = nums;
                if(num <= 255 && (cur.length() == 1 || cur[0] != '0')){ //不能超过255且不能有前导0
                    if(step - 3 != 0) //添加点
                        nums += cur + ".";
                    else
                        nums += cur;
                    dfs(step + 1, i + 1); //递归查找下一个数字
                    nums = temp; //回溯
                }
            }
        }
    }
    vector<string> restoreIpAddresses(string s) {
        this->s = s;
        dfs(0, 0);
        return res;
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(3^n)$，3个分枝的树型递归
- 空间复杂度：$O(n)$，递归栈深度为$n$

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)