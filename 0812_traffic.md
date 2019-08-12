就暴力减一下？平台关闭，还没提交，记得试一下。最关键解方程b[j]+x!=a[i]，x非负整数，求个最小x
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
typedef long long llt;
llt m,n;
long long const SIZE=10100;
llt a[SIZE];
llt b[SIZE];
bool flag[SIZE];
int main()
{
   while(scanf("%lld%lld",&n,&m)!=EOF)
   {
       memset(flag,0,sizeof(flag));
       for(int i=1;i<=n;++i) scanf("%lld",&a[i]);
       for(int i=1;i<m;++i) scanf("%lld",&b[i]);
       sort(a+1,a+n+1);sort(b+1,b+1+m);
       for(int i=1;i<=n;++i)
       {
           for(int j=1;j<=n;++j)
           {
               if(b[j]-a[i]>=0) flag[b[j]-a[i]]=1;
           }
       }
       for(int i=0;i<SIZE;++i)
       {
           if(!flag[i]) {printf("%lld\n",i);continue;}
       }
   }
    return 0;
}

```cpp
