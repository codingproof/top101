![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295


#### BM59. [N皇后问题](https://www.nowcoder.com/practice/c76408782512486d91eea181107293b6?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/c76408782512486d91eea181107293b6?tpId=295&sfm=github&channel=nowcoder

题目主要信息:
- 在一个$n*n$的棋盘上要摆放$n$个皇后，求摆的方案数，不同位置就是不同方案数
- 摆放要求：任何两个皇后不同行，不同列也不在同一条斜线上

具体思路:

**n个皇后，不同行不同列，那么肯定棋盘每行都会有一个皇后，每列都会有一个皇后。**

对于第一行，皇后可能出现在该行的任意一列，我们用一个数组pos记录皇后出现的位置，下标为行号，元素值为列号。如果皇后出现在第一列，那么第一行的皇后位置就确定了，相当于在剩余的n-1行中找n-1个皇后的位置，这就是一个子问题，因此使用递归。

- **终止条件：** 当最后一行都被选择了位置，说明n个皇后位置齐了，增加一种方案数返回。
- **返回值：** 每一级要将选中的位置及方案数返回。
- **本级任务：** 每一级其实就是在该行选择一列作为该行皇后的位置，遍历所有的列选择一个符合条件的位置加入数组，然后进入下一级。

检查是否符合条件，我们可以比那里所有已经记录的行，对其记录的列号查看与当前行列号的关系：即是否同行、同列或是同一对角线。

具体递归过程可以参考如下图示：

![alt](https://uploadfiles.nowcoder.com/images/20220220/397721558_1645324836478/CAAF9B6E5081EEAD4FFF714ED2F8CBA5)

代码实现:
```cpp
class Solution {
public:
    bool isValid(vector<int> &pos, int row, int col){ //判断皇后是否符合条件
        for(int i = 0; i < row; i++){ //遍历所有已经记录的行
            if(row == i || col == pos[i] || abs(row - i) == abs(col - pos[i])) //不能同行同列同一斜线
                return false;
        }
        return true;
    }
    
    void recursion(int n, int row, vector<int> & pos, int &res){ //递归查找皇后种类
        if(row == n){ //完成全部行都选择了位置
            res++; 
            return;
        }
        for(int i = 0; i < n; i++){ //遍历所有列
            if(isValid(pos, row, i)){ //检查该位置是否符合条件
                pos[row] = i; //加入位置
                recursion(n, row + 1, pos, res); //递归继续查找
            }
        }
    }
    
    int Nqueen(int n) {
        int res = 0;
        vector<int> pos(n, 0); //下标为行号，元素为列号，记录皇后位置
        recursion(n, 0, pos, res); //递归
        return res;
    }
};
```

复杂度分析：
- 时间复杂度：$O(n*n!)$，isValid函数每次检查复杂度为$O(n)$，递归过程相当于对长度为$n$的数组求全排列，复杂度为$O(n!)$
- 空间复杂度：$O(n)$，辅助数组和栈空间最大为$O(n)$


![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)