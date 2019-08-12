就暴力减一下？平台关闭，还没提交，记得试一下。最关键解方程b[j]+x!=a[i]，x非负整数，求个最小x
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
int m,n;
const int SIZE=11010;
int b[SIZE];
bool flag[SIZE];
int main()
{
   while(scanf("%d%d",&n,&m)!=EOF)
   {
       memset(flag,0,sizeof(flag));
       for(int i=1;i<=n;++i)
       {
           int tmp;
           scanf("%d",&tmp);
           flag[tmp]=1;
       }
       for(int i=1;i<=m;++i) scanf("%d",&b[i]);
       for(int d=0;;d++)
       {
           int i;
           for(i=1;i<=m;++i)
            {
                if(flag[b[i]+d])
                    break;
            }
           if(i==m+1)
           {
               printf("%d\n",d);
               break;
           }
       }
   }
    return 0;
}


```cpp
