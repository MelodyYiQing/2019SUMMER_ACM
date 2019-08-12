三遍DFS，最关键树的重心就是到各个节点距离和最短的节点，所以题目就可以转化为已知两棵树，根据提示构造两棵树，然后分别求出树的重心，然后连接两棵树的重心后
<br>再深搜一下，求出加工过的树的任意两条边间距离的和就行了（这个是有公式的:任意一条边对距离和的贡献 = 左端结点个数 * 右端结点个数 * 边权）
<br>https://blog.csdn.net/Ratina/article/details/95967532
<br>https://blog.csdn.net/Ratina/article/details/96753811
<br>补的代码基本是照着人家打的 我再加了一点旁注
```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
#define LL long long
using namespace std;
const int INF=0x3f3f3f3f;
const int maxn=1e5+50;//点的数目
const int maxm=2e5+50;//边的数目
struct edge{
    int u;
    int v;
    int next;
}e[maxm];
int head[maxn],cnt;
void adde(int u,int v)
{
    e[cnt].u=u;
    e[cnt].v=v;
    e[cnt].next=head[u];
    head[u]=cnt++;
}
int n;
bool vis[maxn];
int tot;
void DFS0(int u)//分开两棵树
{
    if(vis[u]) return;
    vis[u]=true;
    tot++;//u是子节点，节点数加一
    for(int i=head[u];i!=-1;i=e[i].next)
        DFS0(e[i].v);
}
int p,minimum;
int DFS1(int u,int pre,int N)// 求树的重心
{
    int sum=0,max_sub=0;//max_sub是节点u向下的所有子树中节点最多的子树的个数。sum是节点u向下的所有节点，因为如果u为根的话，向上还有一棵子树，那棵树的节点数就是树的总节点树-sum-1
    for(int i=head[u];i!=-1;i=e[i].next)
    {
        int v=e[i].v;
        if(v==pre) continue; //双向边防止绕回来
        int t=DFS1(v,u,N);
        max_sub=max(max_sub,t);//求拥有最多节点的子树的节点个数
        sum+=t;
    }
    max_sub=max(max_sub,N-sum-1);//考虑u向上回溯的那个子树的节点数
    if(max_sub<minimum)
    {
        minimum=max_sub;
        p=u;//p是记录的重心
    }
    return sum+1;//返回的子节点数要算上自己
}
LL ans;
int DFS2(int u,int pre)//算一边的节点数，减一下就得到了另外一边的节点数
{
    int sum=0;
    for(int i=head[u];i!=-1;i=e[i].next)
    {
        int v=e[i].v;
        if(v==pre) continue;
        int t=DFS2(v,u);
        ans+=(1LL*t)*(1LL*(n-t));//算任意两节点间的距离和：任意一条边对距离和的贡献 = 左端结点个数 * 右端结点个数 * 边权（这里设置为1）
        sum+=t;
    }
    return sum+1;
}
int main()
{
    memset(head,-1,sizeof(head));
    cnt=0;
    scanf("%d",&n);//两棵树 n个节点 所以有n-2条边 （所以才是连一条边，变成一棵树
    for(int i=1;i<=n-2;i++)
    {
        int u,v;
        scanf("%d%d",&u,&v);
        adde(u,v);
        adde(v,u);
    }
    int n1,n2,s1,s2;//s1是树1的节点n1是树1的节点数，s2是树2的节点，n2是树2的节点数
    for(int i=1;i<=n;++i)
    {
        if(!vis[i])
        {
            tot=0;
            DFS0(i);
            if(i==1)
            {
            s1=i;
            n1=tot;
            }
            else
            {
            s2=i;
            n2=tot;
            }
        }

    }
    int g1,g2;//树1和树2的重心
    minimum=INF;
    DFS1(s1,-1,n1);
    g1=p;
    minimum=INF;
    DFS1(s2,-1,n2);
    g2=p;
    adde(g1,g2);
    adde(g2,g1);//重心加边
    ans=0;
    DFS2(1,-1);
    printf("%lld\n",ans);
    return 0;
}
```cpp
