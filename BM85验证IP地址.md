![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM85. [验证IP地址](https://www.nowcoder.com/practice/55fb3c68d08d46119f76ae2df7566880?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/55fb3c68d08d46119f76ae2df7566880?tpId=295&sfm=github&channel=nowcoder


**题目的主要信息：**

- IPv4只有十进制数和分割点，其中数字在0-255之间，共4组，且不能有零开头的非零数，不能缺省
- IPv6由8组16进制数组成，会出现a-fA-F，通过冒号分割，不可缺省，可以零开头，或者为一个单独零，每组最多4位。

**方法一：分割字符串比较法**

我们可以先对IP字符串进行分割，然后依次判断每个分割是否符合要求。

**具体做法：**

写一个split函数，遍历IP字符串，遇到.或者:将其分开储存在一个vector中，遍历vector，判断其中每个字符串是否符合上述要求。
![图片说明](https://uploadfiles.nowcoder.com/images/20210721/397721558_1626858977926/738B9E9B9C66144EE1BF52477AB94768 "图片标题") 
```c++
class Solution {
public:
    vector<string> split(string s, string spliter) {  //将字符串从.或者:分割开
        vector<string> res;
        int i;
        while ((i = s.find(spliter)) && i != s.npos) {  //遍历字符串查找spliter
            res.push_back(s.substr(0, i));  //将分割的部分加入vector中
            s = s.substr(i + 1);
        }
        res.push_back(s);
        return res;
    }
    bool isIPv4 (string IP) {
        vector<string> s = split(IP, ".");  
        if (s.size() != 4)  //IPv4必定为4组
            return false;
        for (int i = 0; i < s.size(); i++) {
            if (s[i].size() == 0)  //不可缺省，有一个分割为零，说明两个点相连
                return false;
            //比较数字位数及不为零时不能有前缀零
            if (s[i].size() < 0 || s[i].size() > 3 || (s[i][0]=='0' && s[i].size() != 1))  
                return false;
            for (int j = 0; j < s[i].size(); j++)  //遍历每个分割字符串，必须为数字
                if (!isdigit(s[i][j])) 
                    return false;
            int num = stoi(s[i]);  //转化为数字比较，0-255之间
            if (num < 0 || num > 255) 
                return false;
        }    
        return true;
    }
    bool isIPv6 (string IP) {
        vector<string> s = split(IP, ":");
        if (s.size() != 8)  //IPv6必定为8组
            return false;
        for (int i = 0; i < s.size(); i++) { 
            if (s[i].size() == 0 || s[i].size() > 4)  //每个分割不能缺省，不能超过4位
                return false; 
            for (int j = 0; j < s[i].size(); j++){
                //不能出现a-fA-F以外的大小写字符
                if (!(isdigit(s[i][j]) || (s[i][j] >= 'a' && s[i][j] <= 'f') || (s[i][j] >= 'A' && s[i][j] <= 'F')))
                    return false;
            }
        }
        return true;
    }
    string solve(string IP) {
        if(IP.size() == 0)
            return "Neither";
        if(isIPv4(IP))
            return "IPv4";
        else if(isIPv6(IP))
            return "IPv6";
        return "Neither";
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(n)$，n为字符串IP的长度，判断部分只遍历4组或者8组，但是分割字符串需要遍历全部
- 空间复杂度：$O(1)$，储存分割字符串的临时空间为常数4或者8


**方法二：正则表达式**

**具体做法：**

IP地址是有规律可言的，我们可以根据正则表达式来判断
```c++
#include <regex>  //必须加正则表达式的库
class Solution {
public:
    string solve(string IP) {
        //正则表达式限制0-255 且没有前缀0 四组齐全
        regex ipv4("(([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])\\.){3}([0-9]|[1-9][0-9]|1[0-9][0-9]|2[0-4][0-9]|25[0-5])");
        //正则表达式限制出现8组，0-9a-fA-F的数，个数必须是1-4个
        regex ipv6("([0-9a-fA-F]{1,4}\\:){7}[0-9a-fA-F]{1,4}");
        if (regex_match(IP, ipv4)) //调用正则匹配函数
            return "IPv4";
        else if (regex_match(IP, ipv6)) 
            return "IPv6";
        else return "Neither";
    }
};
```

**复杂度分析：**
- 时间复杂度：$O(n)$，regex_match函数默认$O(n)$
- 空间复杂度：$O(1)$，没有使用额外空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)