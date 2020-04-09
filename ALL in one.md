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
    for(int i=h[u];i;ne[i])if(p[i]!=father){
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
    if(dis[u]>rt){ rt=u; }
    for(int i=h[u];i;ne[i])if(p[i]!=father){
        dis[v]=dis[u]+w[i];
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
## dfs序
见云笔记图解  
