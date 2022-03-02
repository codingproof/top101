![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM101. [设计LFU缓存结构](https://www.nowcoder.com/practice/93aacb4a887b46d897b00823f30bfea1?tpId=295&sfm=github&channel=nowcoder)


https://www.nowcoder.com/practice/93aacb4a887b46d897b00823f30bfea1?tpId=295&sfm=github&channel=nowcoder

**题目的主要信息：**

- 实现LFU的set与get函数，且复杂度为O(1)
- 每次调用这两个函数会给一个频率赋值，超出长度则移除频率最少的，若有频率相同，则移除访问时间最早的

**方法一：平衡二叉树+哈希表**

哈希表有非常好的之间访问，可以达成O(1)，而经过算术符号重载后的平衡二叉树，能够找到最近最少用的放在树根节点。

**具体做法：**

使用STL自带的map作为哈希表，set作为平衡二叉树。
对于 get函数，只需要查看一下哈希表mp是否有key这个键即可，有的话需要同时更新哈希表和集合中该缓存的使用频率cnt以及使用时间time，否则返回 -1。（时间最大等于操作数n，所以不用担心很大）
而对于put函数，首先需要查看mp中是否已有对应的键值。如果有的话不用其他操作，仅需要更新缓存的 频率和时间值。如果没有的话相当于是新插入一个缓存，这时候需要先查看是否达到缓存容量最大的capacity，如果达到了的话，需要删除最近最少使用的缓存，即平衡二叉树中最左边的结点，同时删除 mp中对应的索引，最后向mp和二叉树中插入新的缓存信息即可。
```c++
class Solution {
public:
      struct Node {
          int cnt; //频率
          int time; //访问时间
          int key;
          int value;
          Node(int _cnt, int _time, int _key, int _value):cnt(_cnt), time(_time), key(_key), value(_value){}
          //将 cnt（使用频率）作为第一关键字，time（最近一次使用的时间）作为第二关键字进行比较
          bool operator< (const Node& rhs) const { //重载比较符号<
              return cnt == rhs.cnt ? time < rhs.time : cnt < rhs.cnt;
          }
      };
      int capacity; //全局变量表示当前LFU容量
      int time; //表示当前的次数
      unordered_map<int, Node> mp; //哈希表
      set<Node> S; //平衡二叉树
      int get(int key) {
        if (capacity == 0) 
            return -1;
        auto it = mp.find(key);
        // 如果哈希表中没有键 key，返回 -1
        if (it == mp.end()) 
            return -1;
        // 从哈希表中得到旧的缓存
        Node cache = it->second;
        // 从平衡二叉树中删除旧的缓存
        S.erase(cache);
        // 将旧缓存更新
        cache.cnt += 1;
        cache.time = ++time;
        // 将新缓存重新放入哈希表和平衡二叉树中
        S.insert(cache);
        it -> second = cache;
        return cache.value;
    }    
    void set(int key, int value) {
        if (capacity == 0) 
            return;
        auto it = mp.find(key);
        if (it == mp.end()) {
            // 如果到达缓存容量上限
            if (mp.size() == capacity) {
                // 从哈希表和平衡二叉树中删除最近最少使用的缓存
                mp.erase(S.begin() -> key);
                S.erase(S.begin());
            }
            // 创建新的缓存
            Node cache = Node(1, ++time, key, value);
            // 将新缓存放入哈希表和平衡二叉树中
            mp.insert(make_pair(key, cache));
            S.insert(cache);
        }
        else {
            // 这里和 get() 函数类似
            Node cache = it -> second;
            S.erase(cache);
            cache.cnt += 1;
            cache.time = ++time;
            cache.value = value;
            S.insert(cache);
            it -> second = cache;
        }
    }
      vector<int> LFU(vector<vector<int> >& operators, int k) {
         vector<int> res;  //记录输出
         capacity = k; 
         for (int i = 0; i < operators.size(); i++) {
             if (operators[i][0] == 1)
                 set(operators[i][1], operators[i][2]);
             else if (operators[i][0] == 2) {
                 res.push_back(get(operators[i][1]));
             }
         }
       return res;       
    }
};
```

**复杂度分析：**
- 时间复杂度：O(nlgk)，get时间复杂度O(logk)，put 时间复杂度O(logk)，一共n步操作，k的容量
- 空间复杂度：O(k)，哈希表和平衡二叉树不会存放超过缓存容量的键值对

**方法二：双哈希表**

**具体做法：**

需要建立一个双向量表及两个哈希表，链表结点存储频率、key及value。第一个哈希表建立链表与频率的映射，旨在能O(1)找到最小频率；第二个哈希表建立键值key到第一个哈希表的映射，旨在能O(1)找到key对应的那组数据。
对于get函数，直接访问哈希表即可，但是访问后要更新频率；
对于set函数，需要容量未满，则直接加入，若是满了则通过第一个哈希表剔出频率最低的结点，最后要更新结点频率为1。
![图片说明](https://uploadfiles.nowcoder.com/images/20210718/397721558_1626600963354/B3D97C7EC83825350468EBD04E3E111E "图片标题") 
以下代码使用了C++ STL自带的list模仿双向链表。

```c++
class Solution {
public:
    //用list模拟双向链表
    unordered_map<int, list<vector<int> > > freq_mp; //频率到双向链表的哈希表
    unordered_map<int, list<vector<int> > ::iterator> mp; //key到双向链表迭代器的哈希表
    int min_fre = 0; //记录当前最小频次
    int capacity; //记录缓存剩余容量
    void update(list<vector<int> >::iterator iter, int key, int value) { //调用函数时更新频率
        int num = (*iter)[0];   //找到key
        freq_mp[num].erase(iter); 
        if(freq_mp[num].empty()) { 
            freq_mp.erase(num); 
            if(min_fre == num) min_fre++; //若当前频率为最小，最小频率加一
        }
        freq_mp[num + 1].push_front({num + 1, key, value}); //插入频率加一的双向链表表头，链表中对应：频率 key value
        mp[key] = freq_mp[num + 1].begin(); 
    }
    void set(int key, int value) {
        auto it = mp.find(key); 
        if(it != mp.end())
            update(it->second, key, value); //更新频率
        else { 
            if(capacity == 0) { //满容量则取频率最低的键
                int oldnum = freq_mp[min_fre].back()[1]; 
                freq_mp[min_fre].pop_back(); 
                if(freq_mp[min_fre].empty()) freq_mp.erase(min_fre); 
                mp.erase(oldnum); 
            }
            else capacity--; //若有空闲则直接加入，容量减1
            min_fre = 1; //最小频率置为1
            freq_mp[1].push_front({1, key, value}); //在频率为1的双向链表表头插入该键
            mp[key] = freq_mp[1].begin(); 
        }
    }
    int get(int key) {
        auto it = mp.find(key);
        if(it != mp.end()) { 
            auto iter = it->second; 
            int value = (*iter)[2]; 
            update(iter, key, value); //更新频率
            return value; 
        }
        else 
            return -1; //找不到返回-1
    }
    vector<int> LFU(vector<vector<int> >& operators, int k) {
         vector<int> res;  //记录输出
         capacity = k; 
         for (int i = 0; i < operators.size(); i++) {
             if (operators[i][0] == 1)
                 set(operators[i][1], operators[i][2]);
             else if (operators[i][0] == 2) {
                 res.push_back(get(operators[i][1]));
             }
         }
       return res;       
    }
};
```

**复杂度分析：**
- 时间复杂度：O(n)，取决于操作数n
- 空间复杂度：O(k)，取决于缓存容量k

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)