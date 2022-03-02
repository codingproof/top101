![alt](https://uploadfiles.nowcoder.com/bm/top101-head.jpg)

https://www.nowcoder.com/exam/oj?tab=%E7%AE%97%E6%B3%95%E7%AF%87&topicId=295

#### BM100. [设计LRU缓存结构](https://www.nowcoder.com/practice/e3769a5f49894d49b871c09cadd13a61?tpId=295&sfm=github&channel=nowcoder)

https://www.nowcoder.com/practice/e3769a5f49894d49b871c09cadd13a61?tpId=295&sfm=github&channel=nowcoder

**题目的主要信息：**

- 实现LRU缓存的模拟结构，包括加入函数set，访问函数get
- 结构有长度限制，加入新数时，超出长度则需要删除最不常访问的，其中set与get都访问
- 两个函数都是O(1)

**方法一：构建双向链表**

插入与访问值都是O(1)，没有任何一种数据结构可以做到。
于是我们可以想到数据结构的组合。访问O(1)很容易想到了哈希表，插入O(1)有很多，但是如果访问到了再插入，且超出长度要在O(1)之内删除，我们可以想到用链表。因为要在O(1)之内删除最不常访问的，所以是双向链表。于是我们的方法就是哈希表+双向链表。

**具体做法：**

用哈希表存储链表结点和key值，能够做到O(1)访问链表任意结点，每次调用函数后将该结点放到链表最前方表示权重最大，最常访问，每次删除链表最后一个结点。要实现这个操作，我们需要的是有头结点和尾结点的双向链表。
![图片说明](https://uploadfiles.nowcoder.com/images/20210718/397721558_1626595271243/4FCCB58C63F2E066A9D67D0BCEAB99B0 "图片标题") 

```c++
class Solution {
public:
      struct node {//设置双向链表结构
          node* next;
          node* pre;
          int key;
          int val;
          node(int k, int v) : key(k), val(v), next(NULL), pre(NULL) {}//可以直接输入数字初始化
       };
      node* head = new node(-1, -1);//设置一个头
      node* tail = new node(-1, -1);//设置一个尾
      int length = 0;//记录链表中有效结点（除去头尾）的数量
      map<int, node* > mp; //哈希表
      void update(node* p) { //每次访问后，更新优先度，即将访问的节点放在第一个位置
          //将p节点从原位置删除
          node* q = p->pre;
          q->next = p->next;
          p->next->pre = q; 
          //将p节点插入到第一个位置
          p->next = head->next;
          head->next->pre = p;
          head->next = p;
          p->pre = head;
      }
      void set(int key, int val, int k) { //插入数据
          if (mp.count(key)) //插入的数据已经存在，更新p节点的位置
              update(mp[key]);
          else {     //否则加入数据
              node* p = new node(key, val);  //创建新节点加入哈希表
              mp[key] = p;
              length++;
              //将p节点插入到第一个位置
              p->next = head->next;
              p->next->pre = p;
              head->next = p;
              p->pre = head;
              if (length > k) {  //超出LRU缓存空间
                  node* q = tail->pre->pre;
                  node* t = q->next;
                  q->next = tail;
                  tail->pre = q;
                  mp.erase(t->key);//删除map里面的数据
                  delete t;
              }
          }
      }
      int get(int key) {  //访问数据
          if (mp.count(key)) {  //哈希表找到数据更新节点，并返回
              node* p = mp[key];
              update(p);
              return p->val;
          }
          else {//返回-1
              node* p = new node(-1, -1);
              return p->val;
          }
      }
      vector<int> LRU(vector<vector<int> >& operators, int k) {
         head->next = tail;
         tail->next = head; //先将链表首位相接,便于插入与删除
         vector<int> res;  //记录输出
         for (int i = 0; i < operators.size(); i++) {
             if (operators[i][0] == 1)
                 set(operators[i][1], operators[i][2], k);
             else if (operators[i][0] == 2) {
                 res.push_back(get(operators[i][1]));
             }
         }
       return res;        
    }
};
```

**复杂度分析：**
- 时间复杂度：O(nlgn)，map为哈希表
- 空间复杂度：O(k)，链表和哈希表都是O(k)的辅助空间

**方法二：利用LST中的list类**


**具体方法：**

用list代替新建链表，每次访问过后将其取出加到链表最前方，要去掉一位时，直接去掉最后一位。
```c++
class Solution {
public:
      
    list <pair<int, int> >  plist;  //用于list列表对模拟双向链表
    unordered_map<int, list<pair<int, int> >::iterator> mp;  //用于set 方法
    int capacity; //每次存放的最大容量
    void set(int key, int  value)
    {
        auto iter = mp.find(key);
        if(iter != mp.end()){//找到将其取出放到第一位
            plist.erase(mp[key]);
            plist.push_back({key, value});  
            mp[key] = plist.begin();
        }else{
            if(capacity == plist.size()){ //容量满则去掉最后一位
                mp.erase(plist.back().first);
                plist.pop_back();
            }
            plist.push_front({key, value});
            mp[key] = plist.begin();
        }
    }
    int get(int key)//获取那个经常使用的键值对
    {
      auto  iter = mp.find(key);
      if(iter != mp.end()){ //找到将其取出放到第一位
          plist.erase(mp[key]); 
          plist.push_front({key,iter->second->second});  
          mp[key]=plist.begin();
          return iter->second->second;  //返回值
      }else
          return -1;
    }
    vector<int> LRU(vector<vector<int> >& operators, int k) {
       vector<int>  res;
       if(k == 0)
           return res;
        capacity = k;
        int n = operators.size();
        for(int i = 0; i < n; i++){  
            if(operators[i][0] == 1){
                set(operators[i][1], operators[i][2]);
            }
            else if(operators[i][0] == 2){
                res.push_back(get(operators[i][1]));
            }
        }
        return  res;
    }     
};
```

**复杂度分析：**
- 时间复杂度：O(n)，所有函数访问都是O(1)，n取决于operators数组的大小
- 空间复杂度：O(k)，链表和哈希表都是O(k)的辅助空间

![alt](https://uploadfiles.nowcoder.com/bm/top101-tail.jpg)