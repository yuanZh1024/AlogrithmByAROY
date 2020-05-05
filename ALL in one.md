```
#pragma GCC optimize("O2")
#pragma GCC optimize("O3")
#pragma GCC optimize("Ofast")
#pragma GCC optimize("inline")
#pragma GCC optimize("-fgcse")
#pragma GCC optimize("-fgcse-lm")
#pragma GCC optimize("-fipa-sra")
#pragma GCC optimize("-ftree-pre")
#pragma GCC optimize("-ftree-vrp")
#pragma GCC optimize("-fpeephole2")
#pragma GCC optimize("-ffast-math")
#pragma GCC optimize("-fsched-spec")
#pragma GCC optimize("unroll-loops")
#pragma GCC optimize("-falign-jumps")
#pragma GCC optimize("-falign-loops")
#pragma GCC optimize("-falign-labels")
#pragma GCC optimize("-fdevirtualize")
#pragma GCC optimize("-fcaller-saves")
#pragma GCC optimize("-fcrossjumping")
#pragma GCC optimize("-fthread-jumps")
#pragma GCC optimize("-funroll-loops")
#pragma GCC optimize("-fwhole-program")
#pragma GCC optimize("-freorder-blocks")
#pragma GCC optimize("-fschedule-insns")
#pragma GCC optimize("inline-functions")
#pragma GCC optimize("-ftree-tail-merge")
#pragma GCC optimize("-fschedule-insns2")
#pragma GCC optimize("-fstrict-aliasing")
#pragma GCC optimize("-fstrict-overflow")
#pragma GCC optimize("-falign-functions")
#pragma GCC optimize("-fcse-skip-blocks")
#pragma GCC optimize("-fcse-follow-jumps")
#pragma GCC optimize("-fsched-interblock")
#pragma GCC optimize("-fpartial-inlining")
#pragma GCC optimize("no-stack-protector")
#pragma GCC optimize("-freorder-functions")
#pragma GCC optimize("-findirect-inlining")
#pragma GCC optimize("-fhoist-adjacent-loads")
#pragma GCC optimize("-frerun-cse-after-loop")
#pragma GCC optimize("inline-small-functions")
#pragma GCC optimize("-finline-small-functions")
#pragma GCC optimize("-ftree-switch-conversion")
#pragma GCC optimize("-foptimize-sibling-calls")
#pragma GCC optimize("-fexpensive-optimizations")
#pragma GCC optimize("-funsafe-loop-optimizations")
#pragma GCC optimize("inline-functions-called-once")
#pragma GCC optimize("-fdelete-null-pointer-checks")
#define rint register int //OI专用
```
```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define MYINTMAX 0x3f3f3f3f
#define N 100005
ll n, k[N];
ll ans, f[N];
ll n,tot,rt,dis[N],fa[N],h[N],ne[N<<1],p[N<<1],wi[N<<1];
void addEdge(int u,int v,int w){p[++tot]=v;ne[tot]=h[u];h[u]=tot;wi[tot]=w;}
void add(int u,int v){p[++tot]=v;ne[tot]=h[u];h[u]=tot;}
void addOrEdge(int u,int v){ne[++tot]=h[u];h[v]=tot;}
```
## C++ STL使用
### map
默认按key升序排序。set同样也是默认升序
map<int,map<string,int>> mp中
插入: 
- mp.insert(pair<int,map<string,int>>())
- 数组方式直接插入 mp[1]["abc"]=2;
访问：
- for(auto [id,s]:mp) cout<<id<<s["abc"];
- 如果访问一个不存在在mp中的键值，比如map[1]["d"] 则返回的是默认值(s基本类型都是0)
查找：
- if(mp.find(1)==mp.end()) 

### vector
vector<vector<string>> ans
ans.push_abck({}) ——push一个空的string向量
ans.back().push_back("abc")——向最后一个string向量push一个string
## unique函数
”删除”序列中所有相邻的重复元素(只保留一个)；返回值是一个迭代器，它指向的是去重后容器中不重复序列的最后一个元素的下一个元素。
https://www.cnblogs.com/wangkundentisy/p/9033782.html
iterator unique(iterator it_1,iterator it_2);
## 树状数组
//简洁树状数组模板  
```cpp
for(int pos = nums[i]; pos > 0; pos -= pos&-pos) tmp+=tree[pos];
for(int pos = nums[i]; pos <= maxNum; pos += pos&-pos) tree[pos]++;
////
/*****************树状数组模板********/
int tree[51][MAXN]={0};
int MAXNTREE;
#define sumMod(x) ((x)>=(MOD)?(x)-(MOD):(x))
inline int query(int k,int pos)
{
    //printf("pos = %d\n",pos);
    int ans = 0;
    while (pos > 0) {
        ans = sumMod(tree[k][pos]+ans);
        pos -= pos & (-pos);
    }
    return ans;
}
inline void update(int k,int pos, int val)
{
    int ans = 0;
    while (pos <= MAXNTREE) {
        tree[k][pos] = sumMod(tree[k][pos]+val);
        pos += pos & (-pos);
    }
}
//最常用
int tree[MAXN]={0};
int MAXNTREE;
inline long long query(int pos)
{
    long long  ans = 0;
    while (pos > 0) {
        ans+=tree[pos];
        pos -= pos & (-pos);
    }
    return ans;
}
inline void update(int pos, int val)
{
    int ans = 0;
    while (pos <= MAXNTREE) {
        tree[pos]+=val;
        pos += pos & (-pos);
    }
}
```
## 树的直径
定义：树的直径，即 树上最远的两点的距离（即 树上最大距离），若树的边权全为1，则树的直径即是**树上的最长链**。  
通常有两种树的直径的求法，时间复杂度均为O(n)  
方法1：**可求得树的直径大小，但无法求得直径的具体路径。**  
选取**任意结点**为根遍历树,设$d[i]$为以节点$i$为根的子树中**叶子到根i的最远距离**，则  
$d[u]=max(d[v]+w[u][v]),v∈u的子结点$  
解释：dfs遍历过程中d[u]相当于保存了(假设以u为根的树的叶子节点为$v_1$,$v_2$……)遍历过的每个叶子节点到u的最远距离
模板：
```cpp
int dis[MAXN],maxDia=0;
int dfs(int u,int father){
    dis[u]=0;
    for(int i=h[u];i;i=ne[i])if(p[i]!=father){
        dfs(p[i],u);
        maxDia=max(maxDia,dis[u]+dis[p[i]]+w[i]);
        dis[u]=max(dis[u],dis[p[i]]+w[i]);
    }
}
```
方法2：该方法要遍历两次，第一次任选结点为根进行DFS，找到距离最远的结点；第二次以该距离最远的结点为根进行DFS，找到距离最远的结点，两点距离即为直径大小，两点简单路径即为直径。    
$dist[i]$：表示i结点到原树根结点的距离  
```cpp
int dis[MAXN],rt;
void dfs(int u,int father)
{
    if(dis[u]>dis[rt]){ rt=u; }
    for(int i=h[u];i;ne[i])if(p[i]!=father){
        dis[p[i]]=dis[u]+w[i];
        dfs(p[i],u);
    }
}
int get_Diameter()
{
    dis[1]=0;
    rt=1;
    dfs(1,0);//找到距离最远的结点rt
    dis[rt]=0;
    dfs(rt,0);//从rt开始dfs
    return dis[rt];
}

```
## dfs序，应用tarjan算法
见云笔记图解  
## tarjan算法

tarjan算法：通过递归和栈操作，找强连通子图，并进行缩点


设每个点的DFS序为dfn[u]，当递归到第u个点，发现下一个点v已经被遍历过，且$dfn[u]>dfn[v]$, 这两个点一定在一个强连通子图中（构成环）


low[u]记录u所在强连通子图中最小的DFS序，对于每个强连通子图，把点都缩为DFS序最小的那个点（即low[u]）  

或者说强连通子图中能到达u的最小编号/最小dfs序


缩点方法：进行递归，每次进入函数时都将当前点入栈，每次退出函数时判断当前点u的DFS序是否为u所在强连通子图中的最小DFS序，即dfn[u]==low[u]。如果是，那么就可以将栈中u之前的点全部缩为点u，即以点u为根的强连通子图（树）


解释2：如果遍历完整个搜索树后某个点的dfn值等于low值，则它是该搜索子树的根。这时，它以上（包括它自己）一直到栈顶的所有元素组成一个强连通分量。  

//图解： https://www.bilibili.com/video/av7330663/

```cpp

#define ll long long
#define N 100005
ll n, k[N];
ll ans, f[N];
ll n, tot, rt, dis[N], fa[N], h[N], ne[N << 1], p[N << 1], wi[N << 1];
void addEdge(int u, int v, int w){
    p[++tot] = v;
    ne[tot] = h[u];
    h[u] = tot;
    wi[tot] = w;
}
void add(int u, int v)
{
    p[++tot] = v;
    ne[tot] = h[u];
    h[u] = tot;
}
ll dfn[N], low[N];
int rankin;
int sta[N], vis[N];
int top = -1;
void tarjan(int u, int father)
{
    dfn[u] = low[u] = ++rankin;
    sta[++top] = u; //入栈
    vis[u] = 1;
    for (int i = h[u]; i; i = ne[i])if (p[i] != father)
        {
            int v = p[i];
            if (dfn[v] == 0){ //未访问
                tarjan(v, u);
                low[u] = min(low[u], low[v]);
            }
            else if (vis[v]){
                low[u] = min(low[u], dfn[v]);
            }
        }
    if (low[u] == dfn[u]){ // 自己到达自己
        do{
            int x = top--;
            vis[x] = 0;
            //ans.push_back(sta[top]);这就是一个强连通分量
        } while (x != u);
    }
}

```

> 典型题目：https://www.luogu.com.cn/problem/P2661

## 并查集模板
```cpp
int parent[1005];
int find(int i)
{  // path compression
    if (parent[i] != i)
        parent[i] = find(parent[i]);
    return parent[i];
}

void Union(int x, int y)
{  // union with rank
    int rootx = find(x);
    int rooty = find(y);
    if (rootx != rooty) {
        parent[rooty] = rootx;
    }
}
```
//计算n的k次方，素数筛，计算组合数C（n，m）组合数4种方案参考https://blog.csdn.net/dadaguai001/article/details/81559554
```cpp
#include <bits/stdc++.h>//题目：https://ac.nowcoder.com/acm/contest/4853/A
using namespace std;
#define ll long long
const int maxn2 = 1e5 + 10;
char s[22][100100];
int nums[maxn2];
int sum[30];
priority_queue<int, vector<int>, greater<int>> pq; //小顶堆

const int cnmmaxn = 100000;
//素数筛
bool arr[cnmmaxn + 1] = {false};
vector<int> produce_prim_number()
{
    vector<int> prim;
    prim.push_back(2);
    int i, j;
    for (i = 3; i * i <= cnmmaxn; i += 2)
    {
        if (!arr[i])
        {
            prim.push_back(i);
            for (j = i * i; j <= cnmmaxn; j += i)
                arr[j] = true;
        }
    }
    while (i < cnmmaxn)
    {
        if (!arr[i])
            prim.push_back(i);
        i += 2;
    }
    return prim;
}
//计算n!中素数因子p的指数
int cal(int x, int p)
{
    int ans = 0;
    long long rec = p;
    while (x >= rec)
    {
        ans += x / rec;
        rec *= p;
    }
    return ans;
}
//计算n的k次方，二分法
int pow(long long n, int k)
{
    long long ans = 1;
    while (k)
    {
        if (k & 1)
        {
            ans = (ans * n);
        }
        n = (n * n);
        k >>= 1;
    }
    return ans;
}
//计算C（n，m）
ll combination(int n, int m)
{
    vector<int> prim = produce_prim_number();
    long long ans = 1;
    int num;
    for (int i = 0; i < prim.size() && prim[i] <= n; ++i)
    {
        num = cal(n, prim[i]) - cal(m, prim[i]) - cal(n - m, prim[i]);
        ans = (ans * (ll)pow(prim[i], num));
    }
    return ans;
}
int main(void)
{
    ll ans = 0;
    int n;
    cin >> n;
    ll tmpsum = 0;
    for (int i = 0; i < n; ++i)
    {
        cin >> nums[i];
        tmpsum += nums[i];
    }
    for (int i = 0; i < n; ++i)
        for (int k = 0; k <= 29; ++k)
        {
            if ((nums[i] >> k) & 1)
                sum[k]++;
        }
    //for(auto x:sum) cout<<x<<" ";
    for (int k = 0; k <= 29; ++k)
    {
        if (sum[k] >= 2)
            ans += (1 << k) * combination(sum[k], 2);
    }
    cout << (ll)ans * 2 + (ll)tmpsum << endl;
    return 0;
}
```
///LCA模板
例题：https://blog.nowcoder.net/n/6ba3155a4b0f4c519b39774dbe3ceabb
```cpp
#define MAXN 200010
int p[MAXN], h[MAXN], ne[MAXN]; 
int num = 0;
int dep[MAXN / 2], f[MAXN / 2][21];
int MAXDEPTH;
//加边
void addEdge(int from, int to)
{
    p[++num] = to; 
    ne[num] = h[from];
    h[from] = num;
 
    p[++num] = from;
    ne[num] = h[to];
    h[to] = num;
}

// dfs预处理深度和倍增
void dfs(int u, int father)
{
    dep[u] = dep[father] + 1;
    for (int i = 1; (1 << i) <= dep[u]; ++i) {
        f[u][i] = f[f[u][i - 1]][i - 1];
    }
    for (int i = h[u]; i; i = ne[i])if (p[i] != father) {
            f[p[i]][0] = u;
            dfs(p[i], u);
        }
}
// bfs预处理深度和倍增
void bfs(int root){
    f[root][0] = root;
    dep[root] = 0;
    queue<int> que;
    que.push(root);
    while(!que.empty()){
        int u = que.front();
        que.pop();
        for(int i = 1; i < MAXDEPTH; i++){
            f[u][i] = f[f[u][i - 1]][i - 1];
        }
        for(int i = h[u]; i; i = ne[i]) if(p[i]!=f[u][0]){
            int v = p[i];
            dep[v] = dep[u] + 1;
            f[v][0] = u;
            que.push(v);
        }
    }
}
// 求LCA
int LCA(int u, int v){
    if(dep[u] > dep[v]) swap(u, v);
    int hu = dep[u], hv = dep[v];
    for(int det = hv - hu, i = 0; det; det >>= 1, i++){
        if(det & 1){ // 深度深的往上跳 直到和深度低的相同深度
            v = f[v][i];
        }
    }
    if(u == v){
        return u;
    }
    for(int i = MAXDEPTH - 1; i >= 0; i--){
        if(f[u][i] == f[v][i]) continue;// 同一深度后继续往上跳，但是两者不能相遇（可能是最远祖先，而不是最近祖先）
        u = f[u][i];
        v = f[v][i];
    }
    return f[u][0];// 无法继续上跳 则为此时x或y的祖先 f[y][0]=f[x][0]
}
void init()
{
    MAXDEPTH = 20;//2的20次方
    num = 0;
    memset(f, 0, sizeof(f));
    memset(h, 0, sizeof(h));
}
```
// LCA性质参考：https://oi-wiki.org/graph/lca/#_5
```cpp
int main()
{
    int t;
    cin >> t;
    while (t--) {
        init();
        int n, q;
        cin >> n >> q;
        for (int i = 0; i < n - 1; i++) {
            int u, v;
            cin >> u >> v;
            addEdge(u, v);
        }
        dep[0]=-1;
        dfs(1, 0);
        //bfs(1);
        while (q--) {
            int a, b, c;
            cin >> a >> b >> c;
            if (b == c && b == 1) {
                cout << "NO\n";
                continue;
            }
            int dis1 = dep[b] + dep[c] - 2 * dep[LCA(b, c)] + dep[c];  // B -> C的距离+ C -> 1的距离dep[c]
            int dis2 = dep[a];
           // cout<<dis1<<" " <<dis2<<" "<<LCA(b, c)<<endl;
            if (dis1 < dis2) {
                cout << "NO\n";
            } else if (dis1 > dis2) {
                cout << "YES\n";
            } else {
                if (LCA(c, a) == 1)
                    cout << "NO\n";
                else
                    cout << "YES\n";
            }
        }
    }
    return 0;
}
```

```cpp
//离散化模板
    sort(d.begin(), d.end());
    d.erase(unique(d.begin(), d.end()), d.end());
    for (int i = 1; i <= n; ++i)
    a[i] = lower_bound(d.begin(), d.end(), a[i]) - d.begin() + 1,
    b[i] = lower_bound(d.begin(), d.end(), b[i]) - d.begin() + 1;
        /*************离散化end******/
```

