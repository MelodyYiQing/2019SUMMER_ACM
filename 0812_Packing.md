暴力模拟，就是模拟的时候感觉稍微要转换一下
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
typedef long long llt;
llt m,n;
long long const SIZE=1e6+10;
struct goods
{
    llt t,x;
}good[SIZE];
llt xi[SIZE];
llt xt[SIZE];
bool cmp(goods a,goods b){return a.t<=b.t;}
int main()
{
    scanf("%lld%lld",&n,&m);
    for(int i=0;i<n;++i)
    {
        scanf("%d%d",&good[i].t,&good[i].x);
    }
    for(int i=1;i<=m;++i) scanf("%d",&xi[i]);
    sort(good,good+n,cmp);
    llt now=0;//good安排到第几个了
    llt possess=0;//拥有的物品数目
    llt need=0;//特定某一秒要付的费用
    llt op=0;//可操作数目
    llt ans=0;
    for(int t=good[0].t;t<=good[n-1].t;++t)//按秒操作 一般need<=possess(因为只要不超标need就不用加）op一般应该不是1就是0
    {
        if(good[now].t==t||possess>op) op++;//有三种情况1.这一秒有物品来了，肯定会有操作2.这一秒没有东西来，但是outlet里有超量的东西要处理3.既没有东西来，又没有东西运，就什么都不要操作
        while(need&&op) need--,possess--,op--;//知道这一秒要操作，就从超量的东西当中拿出来一个，相应的这一秒我要付的钱就少了1
        while(good[now].t==t&&now<n)//操作同一秒的物品
        {
            int xnow=good[now++].x;
            xt[xnow]++;possess++;
            if(xt[xnow]>xi[xnow]) //没读满就不付费
            {
                if(op) op--;
                else need++;
                xt[xnow]--;//记录以后还原
            }
        }
        ans+=need;//ans每秒同步计算要支付的费用
    }
    if(need) ans+=need*(need-1)/2;//最后处理一下遗留问题
    printf("%lld\n",ans);
    return 0;
}

```cpp
