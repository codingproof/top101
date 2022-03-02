![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295


#### BM43. [包含min函数的栈](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=295&sfm=github&channel=nowcoder

**题目中的要求：**

- 实现栈的push、pop、top、min函数
- **访问每个函数的时间复杂度为O(1)**

我们都知道栈结构的push、pop、top操作都是O(1)，但是min函数做不到，于是想到在push的时候将最小值记录下来，由于栈先进后出的特殊性，只能同样用栈来记录最小值。

**方法：双栈法**
**具体做法：**
使用一个栈记录进入栈的元素，正常进行push、pop、top操作，使用另一个栈记录每次push进入的最小值：每次push元素的时候与第二个栈的栈顶元素比较，若是较小，则进入第二个栈，若是较大，则第二个栈的栈顶元素再次入栈，因为即便加了一个元素，它依然是最小值。于是，每次访问最小值即访问第二个栈的栈顶。
![图片说明](https://uploadfiles.nowcoder.com/images/20210717/397721558_1626504307685/BF24FE790ACB10342DE5628AEC3283ED "图片标题") 

```
class Solution {
public:
    stack<int> s1;  //用于栈的push 与 pop
    stack<int> s2;  //用于存储最小min
    void push(int value) {
        s1.push(value);  
        if(s2.empty() || s2.top() > value)  //空或者新元素较小，则入栈
            s2.push(value);
        else
            s2.push(s2.top());  //重复加入栈顶
    }
    void pop() {
        s1.pop();
        s2.pop();
    }
    int top() {
        return s1.top();
    }
    int min() {
        return s2.top();
    }
};
```

**复杂度分析：**
- 时间复杂度：O(1)，每个函数访问都是直接访问，无循环
- 空间复杂度：O(n)，s1为必要空间，s2为辅助空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)