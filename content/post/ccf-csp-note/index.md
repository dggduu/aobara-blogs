+++
author = "aobara"
title = "CCF CSP 笔记"
date = "2025-05-10"
categories = [
    "笔记",
]
+++
# 基础知识
## misc
```cpp
fgets(sep, sizeof(sep), stdin);
```
### 素数
```cpp
int isPrime(int n) {
    if (n <= 1) return 0;
    if (n == 2) return 1;
    if (n % 2 == 0) return 0;

    for (int i = 3; i <= sqrt(n); i += 2) {
        if (n % i == 0)
            return 0;
    }
    return 1;
}
```
### 最大公因数
```cpp
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```
### 最小公倍数
```cpp
int lcm(int a, int b) {
    return a * b / gcd(a, b);
}
```
## 字符串处理函数
### string 转换为char
[^1]
```cpp
    string s;
    char str[200];
    memset(str, 0, sizeof(str)); // 清空数组
    strncpy(str, s.c_str(), sizeof(str) - 1); // 安全地复制
```
## cctype
```c
int tolower(int c);
int toupper(int c);
```
### C-style
```c
sscanf(const char *__source, const char *__format, ...)
从字符串 __source 里读取变量，比如 sscanf(str,"%d",&a)。

sprintf(char *__stream, const char *__format, ...)
将 __format 字符串里的内容输出到 __stream 中，比如 sprintf(str,"%d",i)。

strlen(const char *str)
返回从 str[0] 开始直到 '\0' 的字符数。注意，未开启 O2 优化时，该操作写在循环条件中复杂度是 \Theta(N) 的。

strcmp(const char *str1, const char *str2)
按照字典序比较 str1 str2 若 str1 字典序小返回负值，两者一样返回 0，str1 字典序更大则返回正值。请注意，不要简单的认为返回值只有 0、1、-1 三种，在不同平台下的返回值都遵循正负，但并非都是 0、1、-1。

strcpy(char *str, const char *src)
把 src 中的字符复制到 str 中，str src 均为字符数组头指针，返回值为 str 包含空终止符号 '\0'。

strncpy(char *str, const char *src, int cnt)
复制至多 cnt 个字符到 str 中，若 src 终止而数量未达 cnt 则写入空字符到 str 直至写入总共 cnt 个字符。

strcat(char *str1, const char *str2)
将 str2 接到 str1 的结尾，用 *str2 替换 str1 末尾的 '\0' 返回 str1。

strstr(char *str1, const char *str2)
若 str2 是 str1 的子串，则返回 str2 在 str1 的首次出现的地址；如果 str2 不是 str1 的子串，则返回 NULL。

strchr(const char *str, int c)
找到在字符串 str 中第一次出现字符 c 的位置(char指针)，并返回这个位置的地址。如果未找到该字符则返回 NULL。

strrchr(const char *str, int c)
找到在字符串 str 中最后一次出现字符 c 的位置，并返回这个位置的地址。如果未找到该字符则返回 NULL。
```
----
```c
const char* str = "Try not. Do, or do not. There is no try.";
    char target = 'T';
    const char* result = str;
 
    while ((result = std::strchr(result, target)) != nullptr)
    {
        std::cout << "Found '" << target
                  << "' starting at '" << result << "'\n";

        ++result;
    }
//Found 'T' starting at 'Try not. Do, or do not. There is no try.'
//Found 'T' starting at 'There is no try.'
```
### CPP Style
```cpp
s.find(ch, start = 0)
查找并返回从 start 开始的字符 ch 的位置；rfind(ch) 从末尾开始，查找并返回第一个找到的字符 ch 的位置（皆从 0 开始）（如果查找不到，返回 -1）。
s.substr(start, len) 
可以从字符串的 start（从 0 开始）截取一个长度为 len 的字符串（缺省 len 时代码截取到字符串末尾）。
s.append(s) 
将 s 添加到字符串末尾。
s.append(s, pos, n) 
将字符串 s 中，从 pos 开始的 n 个字符连接到当前字符串结尾。
s.replace(pos, n, s) 
删除从 pos 开始的 n 个字符，然后在 pos 处插入串 s。
s.erase(pos, n) 
删除从 pos 开始的 n 个字符。
s.insert(pos, s) 
在 pos 位置插入字符串 s。

string::npos（无符号数的-1,就是size_t最大值） 的用法
auto found = s1.find(s2);//返回的是size_t类型，
    //不等说明找到了，相等说明没找到
    if (found != string::npos) {
        cout << "first " << s2 << " found at: " << (found)
             << endl;
    }
```

## 文件 IO
### freopen()
```cpp
#include <stdio.h> 
#include <iostream> 
using namespace std;

int main() 
{ 
    int a,b; 
    freopen("in.txt","r",stdin); //输入重定向，输入数据将从in.txt文件中读取
    freopen("out.txt","w",stdout); //输出重定向，输出数据将保存在out.txt文件中
    while(cin>> a >> b) 
        cout<< a+b <<endl; // 注意使用endl
    fclose(stdin);//关闭文件
    fclose(stdout);//关闭文件
    return 0; 
}
```

## 数据结构
### 链表
## STL 库
### 共有函数
```cpp
=
有赋值运算符以及复制构造函数。

begin()
返回指向开头元素的迭代器。

end()
返回指向末尾的下一个元素的迭代器。end() 不指向某个元素，但它是末尾元素的后继。

size()
返回容器内的元素个数。

max_size()
返回容器 理论上 能存储的最大元素个数。依容器类型和所存储变量的类型而变。

empty()
返回容器是否为空。

swap()
交换两个容器。

clear()
清空容器。

==/!=/</>/<=/>=
按 字典序 比较两个容器的大小。（比较元素大小时 map 的每个元素相当于 set<pair<key, value>>，无序容器不支持 </>/<=/>=。）
```
### array
```cpp
// 1. 创建空array，长度为3; 常数复杂度
std::array<int, 3> v0;
// 2. 用指定常数创建array; 常数复杂度
std::array<int, 3> v1{1, 2, 3};

v0.fill(1);  // 填充数组
v0.at(1); //访问指定的元素，同时进行越界检查
operator[] //访问指定的元素，不 进行越界检查
// 访问数组
for (int i = 0; i != arr.size(); ++i) cout << arr[i] << " ";
```
### queue
```cpp
pair<int,int> now = arr.front();
arr.pop();
arr.push(make_pair(dy,dx));

int dx = now.second + tx[i] , dy = now.first + ty[i];
```
## map
```cpp
map<string, int> mp;

# 插入与删除操作
可以直接通过下标访问来进行查询或插入操作。例如 mp["Alan"]=100。

通过向 map 中插入一个类型为 pair<Key, T> 的值可以达到插入元素的目的，例如 mp.insert(pair<string,int>("Alan",100));；

erase(key) 函数会删除键为 key 的 所有 元素。返回值为删除元素的数量。
erase(pos): 删除迭代器为 pos 的元素，要求迭代器必须合法。

erase(first,last): 删除迭代器在 [first,last) 范围内的所有元素。

clear() 函数会清空整个容器。

# 查询操作
count(x)
返回容器内键为 x 的元素数量。复杂度为 O(\log(size)+ans)（关于容器大小对数复杂度，加上匹配个数）。

find(x)
若容器内存在键为 x 的元素，会返回该元素的迭代器；否则返回 end()。

lower_bound(x)
返回指向首个不小于给定键的元素的迭代器。

upper_bound(x)
返回指向首个大于给定键的元素的迭代器。若容器内所有元素均小于或等于给定键，返回 end()。

empty()
返回容器是否为空。

size()
返回容器内元素个数。

for(auto& pair : wordA) {
    if(wordB.find(pair.first) != wordB.end()) {
        ++con;
    }
}
```
## 堆
```cpp
/* 初始化堆 */
// 初始化小顶堆
priority_queue<int, vector<int>, greater<int>> minHeap;
// 初始化大顶堆
priority_queue<int, vector<int>, less<int>> maxHeap;

/* 元素入堆 */
maxHeap.push(1);
maxHeap.push(3);
maxHeap.push(2);
maxHeap.push(5);
maxHeap.push(4);

/* 获取堆顶元素 */
int peek = maxHeap.top(); // 5

/* 堆顶元素出堆 */
// 出堆元素会形成一个从大到小的序列
maxHeap.pop(); // 5
maxHeap.pop(); // 4
maxHeap.pop(); // 3
maxHeap.pop(); // 2
maxHeap.pop(); // 1

/* 获取堆大小 */
int size = maxHeap.size();

/* 判断堆是否为空 */
bool isEmpty = maxHeap.empty();

/* 输入列表并建堆 */
vector<int> input{1, 3, 2, 5, 4};
priority_queue<int, vector<int>, greater<int>> minHeap(input.begin(), input.end());
```
## misc
### 最小堆/最大堆
```cpp
priority_queue<Node*, vector<Node*>, CompareNode> pq;//优先队列，有优先级的先出队
//加上后是最小堆
struct CompareNode {
    bool operator()(Node* a, Node* b) {
        return a->frequency > b->frequency; // 小顶堆
    }
};
```
### 构建哈夫曼树
```cpp
struct Node {
    char symbol;      // 字符
    int frequency;    // 频率
    Node* left;
    Node* right;

    Node(char symbol, int frequency)
        : symbol(symbol), frequency(frequency), left(nullptr), right(nullptr) {}
};

// 比较函数，用于优先队列
struct CompareNode {
    bool operator()(Node* a, Node* b) {
        return a->frequency > b->frequency; // 小顶堆
    }
};

// 递归生成哈夫曼编码表
void encode(Node* root, string str, unordered_map<char, string>& huffmanCode) {
    if (!root) return;

    if (!root->left && !root->right) {
        huffmanCode[root->symbol] = str;
    }

    encode(root->left, str + "0", huffmanCode);
    encode(root->right, str + "1", huffmanCode);
}

// 构建哈夫曼树
Node* buildHuffmanTree(const unordered_map<char, int>& freqMap) {
    priority_queue<Node*, vector<Node*>, CompareNode> pq;

    for (auto& pair : freqMap) {
        pq.push(new Node(pair.first, pair.second));
    }

    while (pq.size() > 1) {
        Node* left = pq.top(); pq.pop();
        Node* right = pq.top(); pq.pop();

        Node* parent = new Node('\0', left->frequency + right->frequency);
        parent->left = left;
        parent->right = right;

        pq.push(parent);
    }

    return pq.empty() ? nullptr : pq.top();
}
```
# 算法
## 排序
### sort()
#### cmp
false 交换；true不交换；  
```cpp
bool compare(const stu &a, const stu &b) {
    // 情况1: a 的年龄 >= 60，而 b 的年龄 < 60
    // 把 a 放在 b 前面（优先排年长者）
    if (a.year >= 60 && b.year < 60)
        return true;
    // 情况2: a 的年龄 < 60，而 b 的年龄 >= 60
    // 把 b 放在 a 前面，所以 a 不应排在前面，返回 false
    if (a.year < 60 && b.year >= 60)
        return false;
    // 情况3: a 和 b 的年龄都 >= 60
    // 先按年龄从大到小排序，如果年龄相同，则按索引从小到大排序
    if (a.year >= 60 && b.year >= 60) {
        if (a.year == b.year)
            return a.index < b.index; // 年龄相同时，索引小的排前面
        return a.year > b.year;       // 年龄大的排前面
    }
    // 情况4: a 和 b 的年龄都 < 60
    // 按索引从小到大排序
    return a.index < b.index;
}
```
#### 字符串按字母进行排序
```cpp
sort(str,str+cnt);
```
### 归并排序
```cpp
void merge(int start, int mid, int end) {
    int i = start, j = mid + 1, k = start;
    while (i <= mid && j <= end) {
        if (arr[i] <= arr[j]) {
            tmp[k++] = arr[i++];
        } else {
            tmp[k++] = arr[j++];
            cnt += (mid - i + 1);
        }
    }
    while (i <= mid) tmp[k++] = arr[i++];
    while (j <= end) tmp[k++] = arr[j++]; 
    for (i = start; i <= end; ++i) arr[i] = tmp[i]; 
}

void merge_sort(int start, int end) {
    if (start >= end) return;
    int mid = (start + end) / 2;
    merge_sort(start, mid);
    merge_sort(mid + 1, end);
    merge(start, mid, end);
}
```
## 模拟
### 大数运算
#### 加法
```cpp
string str1,str2;
int a[220]={0},b[220]={0},c[220]={0};
int main() {
    cin >> str1 >> str2;
    a[0] =str1.size();b[0] = str2.size();
    c[0] = max(a[0],b[0]);
    for(int i = 1;i<=a[0];i++){
        a[i] = str1[a[0]-i] -'0';
    }
    for(int i = 1;i<=b[0];i++){
        b[i] = str2[b[0]-i] -'0';
    }

    int carry = 0,sum =0;
    for(int i =1;i<=c[0];i++) {
        sum = carry + a[i] + b[i];
        if(sum >0){
            carry = sum / 10;
            c[i] += sum%10;
        }else {
            c[i] =sum;
            carry = 0;
        }
    }
    if(carry > 0) {
        c[0]++;
        c[c[0]] = carry;
    }

    while(c[c[0]] == 0&&c[0] > 1){
        c[0]--;
    }
    for(int i =c[0];i>=1;i--){
        cout << c[i];
    }
    return 0;
}
```
#### 减法
```cpp
    int flag=1;//0- 1+
    if(a[0]<b[0]||(a[0]==b[0]&&a[a[0]]<b[b[0]])){
        flag=0;
    }
    for(int i=1;i<=c[0];i++){
        if(flag){
            if(a[i]-b[i]<0){
                a[i+1]--;
                a[i]+=10;
            }
            c[i]=a[i]-b[i];
        }else{
            if(b[i]-a[i]<0){
                b[i+1]--;
                b[i]+=10;
            }
            c[i]=b[i]-a[i];
        }
    }

    if(!flag) cout << '-';
```
#### 乘法
```cpp
    int x;
    for(int i=1;i<=a[0];i++){
        x=0;
        for(int j=1;j<=b[0];j++){
            c[i+j-1]=c[i+j-1]+a[i]*b[j]+x;
                x=c[i+j-1]/10;
                c[i+j-1]%=10;
        }
        c[b[0]+i]=x;
    }
```
## 搜索
### DFS
```cpp
void dfs(int step,int sx,int sy){
    if(step == n*m){
        cnt++;
        return ;
    }
    for(int i=0;i<8;i++){
        int dx= sx+tx[i],dy = sy+ty[i];
        if(dx>=0&&dx<n&&dy>=0&&dy<m&&flag[dx][dy]!=1){
            flag[dx][dy]=1;
            dfs(step+1,dx,dy);
            flag[dx][dy]=0;
        }
    }
}
```
### BFS
```cpp
void bfs(int x,int y) {
	queue<pair<int,int> > arr;
	flag[y][x] = 1; 
	arr.push(make_pair(y,x));
	
	while(!arr.empty()){
		pair<int,int> now = arr.front();
		arr.pop();
		for(int i=0;i<8;i++) {
			int dx = now.second + tx[i] , dy = now.first + ty[i];
			if(map[dy][dx] == 'W' && !flag[dy][dx] && judge(dy,dx)) {
				arr.push(make_pair(dy,dx));
				flag[dy][dx] = true;
			}
		}
	}
}
```
## DP
## 记忆化搜索
```cpp
long long memo[1001]={0};
long long f(int n) {
    if(memo[n] != 0) {
        return memo[n];
    }
    int res = 1;
    if (n == 1) {
        return res;
    } else {
        for (int i = 1; i <= n / 2; i++) {
            res += f(i);
        }
    }
    memo[n] = res;
    return res;
}
```
### 动态规划
#### 思路
1.构造问题
2.拆分成子问题
3.构造状态转移方程
tip:末态最好为单一  
## 图论
### 最小生成树
###  Kruskal
```cpp
bool cmp(const node& a,const node& b){
    return a.val < b.val;
}
int find(int x){
    if(f[x] == x) return x;
    return f[x] = find(f[x]);
}
void func(){
    for(int i =1 ;i<=n;i++) f[i] = i;
    sort(arr+1,arr+1+m,cmp);
    for(int i =1;i<=m;i++){
        int u = find(arr[i].from);
        int v = find(arr[i].to);
        if(u == v) continue;
        ans += arr[i].val;
        f[u] = v;
        total++;
        if(total==n-1) break;
    }
}
```
### 最短路径
```cpp
const int N = 200005;
const int INF = 0x3f3f3f3f;
struct edge
{
    int to, dis, next;
};
edge e[N];
int head[N] = {0}, dis[N]={0}, cnt = 0;
bool vis[N] ={0};
int n, m, s;
void add_edge(int s,int d,int val){
    e[++cnt].dis =val;
    e[cnt].to = d;
    e[cnt].next = head[s];
    head[s] = cnt;
}
void bfs(){
    priority_queue< pair<int,int> > q;
    dis[s] = 0;
    q.push(make_pair(0,s));
    while(!q.empty()) {
        int x = q.top().second;
        q.pop();
        if(vis[x]) continue;
        vis[x] = 1;
        for(int i = head[x];i;i=e[i].next){
            int y = e[i].to,l=e[i].dis;
            if(dis[y] > dis[x] + l){
                dis[y] = dis[x] +l;
                q.push(make_pair(-dis[y],y));
            }
        }
    }
}
int main() {
    cin >> n >> m >> s;
    int a,b,c;
    for(int i =0;i<m;i++){
        cin >> a >> b >> c;
        add_edge(a,b,c);
    }
    for(int i =1;i<=n;i++) dis[i] = INF;
    bfs();
    for(int i =1;i<=n;i++) cout << dis[i] << " ";
    return 0;
}
```
# instance
## 统计单词数
```cpp
int main(){
    char str[1000001] = {0};
    char sep[10] = {0};

    fgets(sep, sizeof(sep), stdin);
    fgets(str, sizeof(str), stdin);

    int sep_len = strlen(sep);
    int str_len = strlen(str);

    if (sep[sep_len - 1] == '\n') sep[sep_len - 1] = '\0';
    if (str[str_len - 1] == '\n') str[str_len - 1] = '\0';

    sep_len = strlen(sep);
    str_len = strlen(str);

    for(int i = 0; i < sep_len; i++)
        sep[i] = tolower(sep[i]);
    for(int i = 0; i < str_len; i++) 
        str[i] = tolower(str[i]);

    int count = 0, index = -1, ch_index = 0;

    for(int i = 0; i < str_len;) {
        while(i < str_len && str[i] == ' ') i++;
        if(i >= str_len) break;
        int start = i;
        while(i < str_len && str[i] != ' ') i++;
        int word_len = i - start;
        if(word_len == sep_len &&
           strncmp(&str[start], sep, word_len) == 0) {
            if(index == -1)
                index = start;
            count ++;
        }
        ch_index++;
    }

    if(count == 0) {
        printf("-1\n");
    } else {
        printf("%d %d\n", count, index);
    }
}
```
# 正则表达式
# 模板
## toLower
```cpp
string toLower(const string& s) {
    string res;
    for (char c : s) {
        res += static_cast<char>(tolower(static_cast<unsigned char>(c)));
    }
    return res;
}
```
## 找字符串
```cpp
    for (const auto& pair : wordsA) {
        if (wordsB.find(pair.first) != wordsB.end()) {
            ++intersectionSize;
        }
    }
```
## KMP
```cpp
void genNext(int m) {
    int j = 0;
    kmp_next[0] = 0;
    for(int i = 1;i < m;i++) {
        while(j > 0 && b[i] != b[j]) j = kmp_next[j - 1];
        if(b[i] == b[j]) j++;
        kmp_next[i] = j;
    }
}
int kmp(int n,int m) {
    int j = 0;
    int p = -1;
    genNext(m);
    for(int i = 0;i < n;i++) {
        while(j > 0 && a[i] != b[j]) j = kmp_next[j - 1];
        if(a[i] == b[j]) j++;
        if(j == m) {
            p = i - m + 1;
            break;
        }
    }
    return p;
}
```
## 进制转换
### 十进制转二进制/八进制/十六进制
```txt
将35.25转化为二进制数
整数部分：
35/2=17 ......1
17/2=8  ......1
8/2=4   ......0
4/2=2   ......0
2/2=1   ......0
1/2=0   ......1
小数部分：
0.25*2=0.5  0
0.5*2=1     1
```
### 二进制/八进制/十六进制转十进制（递推）
```txt
将11010.01(2)转换为十进制数
11010.01(2)=1*2^4+1*2^3+0*2^2+1*2^1+0*2^0+0*2^(-1)+1*2(-2)
        =26.25
```
# ref
[^1]: (基本都来自oiwiki,后面懒得标了)https://oi-wiki.org/string/lib-func/