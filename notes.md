[TOC]

# Notes

## English

### E1. New words and expressions

| Literary | Meaning |
| --------: |:-------- |
| Literary | `adj.` about writing |
| Taking into account that ... | Mentioned that ... |

## OI

### O1. NOIP2016 Simulation Nr. 49 Problem A

小 W 摆石子
【问题描述】
小 W 得到了一堆石子，要放在 N 条水平线与 M 条竖直线构成的网格的交点上。因为小 M 最喜欢矩形了，小 W 希望知道用 K 个石子最多能找到多少四边平行于坐标轴的长方形，它的四个角上都恰好放着一枚石子。
【输入格式】
第一行三个整数 N,M,K。
【输出格式】
一个非负整数，即最多的满足条件的长方形数量。
【输入输出样例】

rectangle.in
3 3 8
rectangle.out
5
rectangle.in
7 14 86
rectangle.out
1398

【数据规模】
对于 50%的数据：N<=30
对于 100%的数据：N<=30000，保证任意两点不重合，K<=N*M

```
/**************************************************************
    Problem: 1884
    User: yanhuihang
    Language: C++
    Result: Accepted
    Time:1 ms
    Memory:1084 kb
****************************************************************/
 
 
#include <algorithm>
#include <cstdio>
 
using std::max;
using std::swap;
 
int main(int argc, char const *argv[])
{
#ifndef ONLINE_JUDGE
  freopen("rect.in", "r", stdin);
#endif
 
  long long int n, m, k;
  scanf("%lld%lld%lld", &n, &m, &k);
 
  if (k == n*m) {
    printf("%lld", n*(n-1)*m*(m-1)/4); }
  else {
    if (n > m) swap(n, m);
 
    long long s = 0;
 
    for (long long x = 1; x <= n; ++x) {
      long long y = k / x;
      long long r = k - x*y;
      if (y > m || (y == m && r != 0)) continue;
      s = max(s, x*(x-1)*y*(y-1)/4+r*y*(r-1)/2); }
 
    printf("%lld", s); }
 
  return 0;
}
```

　　考虑对于一个布满点的网格(x行，Y列)，所有的矩形总数为组合数C(2,x)*C(2,y)。但k不一定刚好就能布满整个网格，所以我们先找到在网格中最大能形成的长方形矩阵的长y和宽x，剩余石子为r，则有k = x * y + r , 最大能形成的长方形矩阵最多有C(x,2)*C(y,2)个矩形，剩余石子 r 以一排或一列的形式，靠在大矩形短的一边（要注意是否到达边界），则多增加的矩形数为 C(2,r)*y （y为长边的点数）。 枚举x与y，计算出C(x,2)*C(y,2)+ C(r,2)*y的最大值。
　　
```
#include <iostream>
#include <cmath>
#include <algorithm>
using namespace std;

long long Func(long long n)
{
    return n*(n-1)/2;
}
int main()
{
    int T;
    cin >> T;
    for(int cnt = 1; cnt <= T; ++cnt)
    {
        int n, m, k;
        cin >> n >> m >> k;
        if(n > m) swap(n,m);    　　　　//n为短边，m为长边（个人习惯~0~）
        long long ans = 0;
        for(int x = 2; x <= n; ++x)   //对x进行枚举（短边）
        {
            int y = k / x;     　　　　//可能的最大长边y
            int r = k % x;     　　　　//剩余的石子数r
            if( y > m || ( y == m && r) )   //超边界  
                continue;
            ans = max(ans, Func(x)*Func(y) + y*Func(r));
        }
        cout << "Case #" << cnt << ": " << ans << endl;
    }
    return 0;
}
```

-----

### O2. NOIP2016 Simulation Nr. 49 Problem B (Fibonacci Sequence, Minus Modulo, Fast Matrix Exponentiation)

> Fibonacci Sequence is defined by:
$f_1 = f_2 = 1$
$f_n = f_{n-1} + f_{n-2} (n \ge 3)$

> Given $N < M$, calculate
$$\sum_{i=N}^M f_i$$
answer is required to be printed modulo $10^5$

We have
$$\sum_{i=N}^M f_i = \sum_{i=1}^M f_i -\sum_{i=1}^{N-1} f_i$$

Let $s_k$ be $\sum_{i=1}^k f_i$, we have

$$\sum_{i=N}^M f_i = s_M - s_{N-1}$$

so we just need to calculate $s$.

Obviously,

$$s_k = s_{k-1} + f_k = s_{k-1} + f_{k-1} + f_{k-2}$$

Use the matrix exponentiation. I don't know how to do or how it works:

$$\begin{pmatrix}
    s_k \\
    f_k \\
    f_{k-1}
\end{pmatrix}  = \begin{pmatrix}
    1 & 1 & 1 \\
    0 & 1 & 1 \\
    0 & 1 & 0 \\
\end{pmatrix}^{k-2}  \begin{pmatrix}
    s_2 \\
    f_2 \\
    f_1
\end{pmatrix}$$

And the code (also I really don't know) :

```
#include<iostream>
#include<cstdlib>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
#define mod 10000
using namespace std;
struct hh
{
    int a[5][5],n,m;
};
hh a,b,c;

int t,l,r,suma,sumb,ans;
int f[50005];

hh mul(hh,hh);
hh ff(hh,int);
int main()
{
    int i,j;
    freopen("fibonacci.in","r",stdin);
    freopen("fibonacci.out","w",stdout);
    scanf("%d",&t);
    a.m=a.n=b.n=3;
    b.m=1;
    for(j=1;j<=3;j++) a.a[1][j]=1;
    a.a[2][2]=a.a[2][3]=1;
    a.a[3][2]=1;
    b.a[1][1]=2;
    b.a[2][1]=b.a[3][1]=1;
    while(t--)
    {
        scanf("%d%d",&l,&r);
        if(l<=3) suma=l-1;
        else{c=mul(ff(a,l-3),b);suma=c.a[1][1];}
        if(r<=2) sumb=r;
        else{c=mul(ff(a,r-2),b);sumb=c.a[1][1];}
        ans=(sumb-suma+mod)%mod;
        printf("%d\n",ans);
    }
    fclose(stdin);
    fclose(stdout);
    return 0;
}

hh mul(hh a,hh b)
{
    int i,j,k;
    hh c;
    c.n=a.n;
    c.m=b.m;
    memset(c.a,0,sizeof(c.a));
    for(i=1;i<=c.n;i++)
      for(j=1;j<=c.m;j++)
        for(k=1;k<=c.n;k++)
         c.a[i][j]=(c.a[i][j]+a.a[i][k]*b.a[k][j])%mod;
    return c;
}

hh ff(hh a,int k)
{
    int i,j;
    hh c;
    c.m=a.m;
    c.n=a.n;
    for(i=1;i<=min(c.n,c.m);i++) c.a[i][i]=1;
    for(i=1;i<=c.n;i++)
      for(j=1;j<=c.m;j++)
         if(i!=j) c.a[i][j]=0;
    while(k)
    {
        if(k&1) c=mul(a,c);
        a=mul(a,a);
        k>>=1;
    }
    return c;
}
```

Another Code (I really don't know):

```
/**************************************************************
    Problem: 1885
    User: noip201603
    Language: C++
    Result: Accepted
    Time:7 ms
    Memory:1084 kb
****************************************************************/
 
 
#include<cstdio>
#include<cstring>
const int maxn=3,mod=10000,sz=2;
int t,x,y,l,r;
struct Matrix
{
    int num[maxn][maxn];
    Matrix operator*(Matrix b)
    {
        Matrix res;memset(res.num,0,sizeof res.num);
        for(register int i=1;i<=sz;++i)for(register int j=1;j<=sz;++j)for(register int k=1;k<=sz;++k)(res.num[i][j]+=num[i][k]*b.num[k][j])%=mod;
        return res;
    }
    void reset(){num[1][1]=num[1][2]=num[2][1]=1,num[2][2]=0;}
}ele;
Matrix operator^(Matrix a,int n)
{
    Matrix res;res.num[1][1]=res.num[2][2]=1,res.num[2][1]=res.num[1][2]=0;
    for(;n;n>>=1,a=a*a)if(n&1)res=res*a;
    return res;
}
int main()
{
    for(scanf("%d",&t);t--;)
    {
        scanf("%d%d",&x,&y);
        ele.reset();
        ele=ele^(x-1);l=(ele.num[1][1]+ele.num[1][2])%mod;
        ele.reset();
        ele=ele^y;r=(ele.num[1][1]+ele.num[1][2])%mod;
        printf("%d\n",(r-l+mod)%mod);
    }
    return 0;
}
```

Yet another code (I really don't know):

```
/**************************************************************
    Problem: 1885
    User: Owen
    Language: C++
    Result: Accepted
    Time:8 ms
    Memory:1084 kb
****************************************************************/
 
 
#include<cstdio>
#include<cstring>
using namespace std;
const int q=1e4;
struct matrix {
    int a[3][3];
    void eye() {
        a[1][2]=a[2][1]=0;
        a[1][1]=a[2][2]=1;
    }
    void base() {
        a[1][1]=a[1][2]=a[2][1]=1;
        a[2][2]=0;
    }
    int first() {
        return a[1][1];
    }
    void clear() {
        memset(a,0,sizeof a);
    }
} bas,m,ret;
matrix mul(matrix b,matrix c) {
    ret.clear();
    for (int i=1;i<=2;++i) for (int j=1;j<=2;++j) {
        for (int k=1;k<=2;++k) (ret.a[i][j]+=b.a[i][k]*c.a[k][j])%=q;
    }
    return ret;
}
int get(int x) {
    m.eye();
    bas.base();
    while (x) {
        if (x&1) m=mul(m,bas);
        x>>=1;
        bas=mul(bas,bas);
    }
    return m.first();
}
int main() {
    #ifndef ONLINE_JUDGE
    freopen("test.in","r",stdin);
    #endif
    int T;
    scanf("%d",&T);
    while (T--) {
        int x,y;
        scanf("%d%d",&x,&y);
        int a=get(y+1),b=get(x); 
        int ans=(a-b+q)%q;
        printf("%d\n",ans);
    }
}
```

Yet yet another code (I really don't know):

```
/**************************************************************
    Problem: 1885
    User: 131441373
    Language: C++
    Result: Accepted
    Time:9 ms
    Memory:1084 kb
****************************************************************/
 
 
#include <cstdio>
#include <cstdlib>
#include <cstring>
const int Q=10000;
int tcas;
 
inline int plus(int x,int y){return (x+y)%Q;}
inline int mul(int x,int y){return (x*y)%Q;}
 
struct G{
    int a[2][2];
    G(){memset(a,0,sizeof(a));}
}fir,pr;
G operator *(G x,G y)
{
    G res;
    for(int k=0;k<2;k++)
    for(int i=0;i<2;i++)
    for(int j=0;j<2;j++) res.a[i][j]=plus(res.a[i][j],mul(x.a[i][k],y.a[k][j]));
    return res;
}
 
int get(int t)
{
    G res=fir;
    G x=pr;
    for(;t>0;t>>=1)
    {
        if(t&1) res=x*res;
        x=x*x;
    }
    return res.a[0][0];
}
 
int main()
{
    #ifndef ONLINE_JUDGE
        freopen("fibonacci.in","r",stdin);
        freopen("fibonacci.out","w",stdout);
    #endif
    fir.a[0][0]=1;fir.a[1][0]=1;
    pr.a[0][1]=pr.a[1][0]=pr.a[1][1]=1;
    int i,x,y;
    scanf("%d",&tcas);
    while(tcas--)
    {
        scanf("%d%d",&x,&y);
        y++;
        x=get(x),y=get(y);
        printf("%d\n",(y-x%10000+10000)%10000);
    }
    return 0;
}
```

-----------------

We have a simpler and faster way. Just brute-force.

Taking into account that the answer should be moduloed. We can guess that $f_i \mod x$ is cyclical and we can find the cyclical is $15000$ with the $x = 10000$ in this problem. So we can initialize the sequence and the sum like this:

```
MOD     := 10000
CYCLE   := 15000
fib     := EMPTY SET
sum     := EMPTY SET

fib[1] = fib[2] = 1
sum[1] = 1
sum[2] = 2

for i in 3..CYCLE
    fib[i] = (fib[i-1] + fib[i-2])%MOD
    sum[i] = (sum[i-1] + fib[i])  %MOD

sum[CYCLE % CYCLE] = sum[CYCLE]
```

The last line is that, we get $s_i$ by `sum[i % CYCLE]`, so when `i = CYCLE` we get `sum[0]`.

Then:

```
read n, m
output (sum[m%CYCLE] - sum[(n-1)%CYCLE] + MOD)%MOD
```

What is the ` + MOD` for?

Imagine a situation when `m > n-1` however `m%CYCLE < (n-1)%CYCLE`, we will get a negative number by evaluating `sum[m%CYCLE] - sum[(n-1)%CYCLE]`. So we should append a ` + MOD`. But how it works? I don't know.

-------

I thought another way but failed. Here is it:

We know

$$f_n = \frac{\phi^n - (1-\phi)^n}{\sqrt{5}}$$

where $$\phi = \frac{1+\sqrt{5}}{2}$$

so we can calculate out the general term formula of $s_n$.

But $s_n$ includes $\phi^{k}$, $k$ is something. And we can not modulo `double` like $\phi$. But if we do not modulo in the halfway, we must deal with high precision big *float* number. So failed.

$$END$$

---------

### O3. NOIP2016 Simulation Nr. 49 Problem C
小 W 计树
【问题描述】
小 W 千辛万苦做出了数列题，突然发现小 M 被困进了迷宫里。迷宫是一个有 N(2≤N≤1000)个顶点 M(N?1≤M≤N?(N ? 1)/2 ) 条边的无向连通图。设 dist1[i]表示在这个无向连通图中， 顶点 i 到顶点 1 的最短距离。为了解开迷宫，现在要求小 W 在这个图中删除 M ? (N ? 1)条边，使得这个迷宫变成一棵树。设 dist2[i]表示在这棵树中，顶点 i 到顶点 1 的距离。小 W 的任务是求出有多少种删除方案，使得对于任意的 i，满足 dist1[i]=dist2[i]。
快点帮助小 W 救出小 M 吧！
【输入格式】
第一行，两个整数， N， M，表示有 N 个顶点和 M 条边。
接下来有 M 行，每行有 3 个整数 x, y, len(1 ≤ x, y ≤ n, 1 ≤ len ≤ 100)，表示顶点 x 和顶点 y 有一条长度为 len 的边。
数据保证不出现自环、重边。
【输出格式】
一行两个整数，表示满足条件的方案数 mod 2147483647 的答案。
【输入输出样例】
treecount.in 
3 3
1 2 2
1 3 1
2 3 1

treecount.in 
2
【样例解释】
删除第一条边或第三条边都能满足条件，所以方案数是 2。
【数据规模】
对于 30%的数据：2≤N≤5，M≤10
对于 50%的数据：满足条件的方案数不超过 10000
对于 100%的数据：2≤N≤1000

 

题解

首先考虑离 1 点最近的那个点，一定和 1 点只连着一条边，则这条边是必选的；然后考察第二近的点，一种可能是和 1 点直接连的边比较近，一种可能是经过刚才最近的那个点再到 1 点的路比较近，不管是哪一种，选择都是唯一的，而剩下第三种可能是两者距离相同，这样的话两者选且只能选一个。以此类推，假设现在已经构造好了前 k 个点的一棵子树，看剩余点中到 1 点最近的点，这个点到 1 点有 k 种方法（分别是和那 k 个点连边）， 其中有 m 个是可以保持最短距离的，则这一步可选的边数就是 m。一直构造，把方法数累乘，就能得到最后的结果。整个过程可以很好的符合 dijkstra 的过程，而生成树的步骤和 prim 如出一辙。

 
```#include<iostream>
#include<cstdlib>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
#define mod 2147483647
using namespace std;
 
int n,m;
bool v[1005];
int we[1010][1010], dis[1010];
int last[1010];
int tot=0;
struct hh
{
    int next,to,w;
}e[2000005];

long long ans=1;
struct node 
{
    int id,d;
};
node a[1005];
queue<int> q;
 
bool cmp(node a, node b) 
{
    return a.d < b.d;
}
 
void add(int a,int b,int c) 
{
    tot++;
    e[tot].to=b;
    e[tot].w=c;
    we[a][b]=c;
    e[tot].next=last[a];
    last[a]=tot;
}

void spfa()
{
    int i,now;
    for(i=2;i<=n;i++) dis[i]=9999999;
    q.push(1); 
    dis[1]=0;
    while(!q.empty()) 
    {
        now=q.front(); 
        q.pop(); 
        v[now]=false;
        for(i=last[now];i;i=e[i].next) 
            if(dis[e[i].to]>dis[now]+e[i].w) 
            {
                dis[e[i].to]=dis[now]+e[i].w;
                if(!v[e[i].to]) 
                {
                    v[e[i].to]=true;
                    q.push(e[i].to);
                }
            }
    }
}

int main() 
{
    int i,j,x,y,z,now;
    freopen("treecount.in","r",stdin);
    freopen("treecount.out","w",stdout);
    scanf("%d%d",&n,&m);
    for(i=1;i<=n;i++) 
     for(j=1;j<=n;j++) 
      we[i][j]=9999999;
    for(i=1;i<=m;i++) 
    {
        scanf("%d%d%d",&x,&y,&z);
        add(x,y,z);
        add(y,x,z);
    }
    spfa();
    for (i=1;i<=n;i++)
    {
        a[i].id=i;
        a[i].d=dis[i];
     } 
    sort(a+1,a+n+1,cmp);
    for(i=2;i<=n;i++) 
    {
        now=0;
        for(j=1;j<=i-1;j++) 
            if(a[i].d==a[j].d+we[a[i].id][a[j].id]) now++;
        ans=ans*(long long)now%mod;
    }
    printf("%lld",ans);
    fclose(stdin);
    fclose(stdout);
    return 0;
}
```

首先考虑离 1 点最近的那个点，一定和 1 点只连着一条边，则这条边是必选的；然后考察第二近的点，一种可能是和 1 点直接连的边比较
近，一种可能是经过刚才最近的那个点再到 1 点的路比较近，不管是哪一种，选择都是唯一的，
而剩下第三种可能是两者距离相同，这样的话两者选且只能选一个。以此类推，假设现在已经构
造好了前 k 个点的一棵子树，看剩余点中到 1 点最近的点，这个点到 1 点有 k 种方法（分别是和
那 k 个点连边）， 其中有 m 个是可以保持最短距离的，则这一步可选的边数就是 m。一直构造，
把方法数累乘，就能得到最后的结果。整个过程可以很好的符合 dijkstra 的过程，而生成树的
步骤和 prim 如出一辙。

```
/**************************************************************
    Problem: 1886
    User: GEOTCBRL
    Language: C++
    Result: Accepted
    Time:141 ms
    Memory:5664 kb
****************************************************************/
 
 
/*
    I will chase the meteor for you, a thousand times over.
    Please wait for me, until I fade forever.
    Just 'coz GEOTCBRL.
*/
#include <bits/stdc++.h>
using namespace std;
#define fore(i,u)  for (int i = head[u] ; i ; i = nxt[i])
#define rep(i,a,b) for (int i = a , _ = b ; i <= _ ; i ++)
#define per(i,a,b) for (int i = a , _ = b ; i >= _ ; i --)
#define For(i,a,b) for (int i = a , _ = b ; i <  _ ; i ++)
#define Dwn(i,a,b) for (int i = ((int) a) - 1 , _ = (b) ; i >= _ ; i --)
#define fors(s0,s) for (int s0 = (s) , _S = s ; s0 ; s0 = (s0 - 1) & _S)
#define foreach(it,s) for (__typeof(s.begin()) it = s.begin(); it != s.end(); it ++)
 
#define mp make_pair
#define pb push_back
#define pii pair<int , int>
#define fir first
#define sec second
#define MS(x,a) memset(x , (a) , sizeof (x))
 
#define gprintf(...) fprintf(stderr , __VA_ARGS__)
#define gout(x) std::cerr << #x << "=" << x << std::endl
#define gout1(a,i) std::cerr << #a << '[' << i << "]=" << a[i] << std::endl
#define gout2(a,i,j) std::cerr << #a << '[' << i << "][" << j << "]=" << a[i][j] << std::endl
#define garr(a,l,r,tp) rep (__it , l , r) gprintf(tp"%c" , a[__it] , " \n"[__it == _])
 
template <class T> inline void upmax(T&a , T b) { if (a < b) a = b ; }
template <class T> inline void upmin(T&a , T b) { if (a > b) a = b ; }
 
typedef long long ll;
 
const int maxn = 1007;
const int maxm = 200007;
const int mod = 1000000007;
const int inf = 0x7fffffff;
const double eps = 1e-7;
 
typedef int arr[maxn];
typedef int adj[maxm];
 
inline int fcmp(double a , double b) {
    if (fabs(a - b) <= eps) return 0;
    if (a < b - eps) return -1;
    return 1;
}
 
inline int add(int a , int b) { return ((ll) a + b) % mod ; }
inline int mul(int a , int b) { return ((ll) a * b) % mod ; }
inline int dec(int a , int b) { return add(a , mod - b % mod) ; }
inline int Pow(int a , int b) {
    int t = 1;
    while (b) {
        if (b & 1) t = mul(t , a);
        a = mul(a , a) , b >>= 1;
    }
    return t;
}
 
#define gc getchar
#define idg isdigit
#define rd RD<int>
#define rdll RD<long long>
template <typename Type>
inline Type RD() {
    char c = getchar(); Type x = 0 , flag = 1;
    while (!idg(c) && c != '-') c = getchar();
    if (c == '-') flag = -1; else x = c - '0';
    while (idg(c = gc()))x = x * 10 + c - '0';
    return x * flag;
}
inline char rdch() {
    char c = gc();
    while (!isalpha(c)) c = gc();
    return c;
}
#undef idg
#undef gc
 
// beginning
 
int n , m;
arr G[maxn] , dis , vis , pre;
 
void input() {
    n = rd() , m = rd();
    rep (i , 1 , n) rep (j , 1 , n) G[i][j] = n * 200;
    rep (i , 1 , m) {
        int u = rd() , v = rd() , w = rd();
        G[u][v] = G[v][u] = w;
    }
}
 
inline void dijk() {
    rep (i , 1 , n) dis[i] = inf;
    dis[1] = 0;
    rep (x , 1 , n) {
        int u = 0;
        rep (i , 1 , n) if (!vis[i] && (!u || dis[i] < dis[u])) u = i;
        if (!u) break;
        vis[u] = 1;
        rep (v , 1 , n) if (!vis[v] && G[u][v] <= 100) {
            if (dis[v] > dis[u] + G[u][v]) {
                dis[v] = dis[u] + G[u][v];
                pre[v] = 1;
            } else if (dis[v] == dis[u] + G[u][v])
                pre[v] ++;
        }
    }
}
 
void solve() {
    dijk();
//  garr(pre , 1 , n , "%d");
    int ans = 1;
    rep (i , 2 , n) ans = 1ll * ans * pre[i] % 2147483647;
    printf("%d\n" , ans);
}
 
int main() {
    #ifndef ONLINE_JUDGE
        freopen("data.txt" , "r" , stdin);
    #endif
    input();
    solve();
    return 0;
}
```

```
/**************************************************************
    Problem: 1886
    User: noip201603
    Language: C++
    Result: Accepted
    Time:470 ms
    Memory:18672 kb
****************************************************************/
 
 
#include<cstdio>
#include<cstring>
typedef long long ll;
const int maxn=1005,maxe=1000005,maxE=500005,inf=1<<30;
ll mod=2147483647ll;
int head[maxn],to[maxe],next[maxe],len[maxe],dis[maxn],st[maxE],en[maxE],l[maxE],n,m,cnt;
bool inq[maxn];
ll ans=1;
struct Queue
{
    int num[maxn],s,t;
    bool empty(){return s==t;}
    int nxt(int x){return x==n-1?0:x+1;}
    void pop(){s=nxt(s);}
    void push(int x){num[t]=x;t=nxt(t);}
    int front(){return num[s];}
    void reset(){s=t=0;}
}q;
void insert(int a,int b,int c){to[cnt]=b,len[cnt]=c,next[cnt]=head[a];head[a]=cnt++;}
void spfa()
{
    q.reset();q.push(1);memset(inq,0,sizeof inq);inq[1]=1;
    for(register int i=1;i<=n;++i)dis[i]=inf;dis[1]=0;
    int u;
    while(!q.empty())
    {
        inq[u=q.front()]=0;q.pop();
        for(register int i=head[u],v=to[i];~i;v=to[i=next[i]])if(dis[v]>dis[u]+len[i])
        {
            dis[v]=dis[u]+len[i];
            if(!inq[v])q.push(v),inq[v]=1;
        }
    }
}
int main()
{
    memset(head,-1,sizeof head);
    scanf("%d%d",&n,&m);for(register int i=1;i<=m;++i)scanf("%d%d%d",st+i,en+i,l+i),insert(st[i],en[i],l[i]),insert(en[i],st[i],l[i]);
    spfa();
    for(register int s=2;s<=n;++s)
    {
        cnt=0;
        for(register int i=head[s],v=to[i];~i;v=to[i=next[i]])if(dis[v]+len[i]==dis[s])++cnt;
        (ans*=cnt)%=mod;
    }
    printf("%lld\n",ans);
    return 0;
}
```

```
/**************************************************************
    Problem: 1886
    User: ez_myh
    Language: C++
    Result: Accepted
    Time:192 ms
    Memory:5044 kb
****************************************************************/
 
 
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <utility>
#define FR first
#define SE second
 
using namespace std;
 
typedef long long ll;
typedef pair<int,int> pr;
 
int e[1005][1005];
 
int dis[1005];
bool check[1005];
 
pr a[1005];
 
void dijkstra(int n) {
  memset(dis,0x7f,sizeof(dis));
  dis[1]=0;
  for(int i=1;i<n;i++) {
    int p=0;
    for(int j=1;j<=n;j++)
      if (!check[j]&&dis[j]<dis[p]) p=j;
    check[p]=true;
    for(int j=1;j<=n;j++)
      if (!check[j]&&e[p][j]) dis[j]=min(dis[j],dis[p]+e[p][j]);
  }
}
 
int main() {
  int n,m;
  scanf("%d%d",&n,&m);
  for(int i=1;i<=m;i++) {
    int x,y,z;
    scanf("%d%d%d",&x,&y,&z);
    e[x][y]=z;
    e[y][x]=z;
  }
  dijkstra(n);
  for(int i=1;i<=n;i++) a[i]=pr(dis[i],i);
  sort(a+1,a+n+1);
  int ans=1;
  for(int i=2;i<=n;i++) {
    int p=a[i].SE,s=0;
    for(int j=1;j<i;j++) {
        int q=a[j].SE;
        if (e[q][p]&&dis[q]+e[q][p]==dis[p]) s++;
    }
    ans=(ll)ans*s%0x7fffffff;
  }
  printf("%d\n",ans);
  return 0;
}
```

$$END$$

----

## Cmd Markdown

### M1. Slash

We can't type some special symbol like `$#\` in formula, just
append slash `\backslash` in the head.

We can't get slash by this way because slash slash `\\` means line-break. We can use `\backslash`.

### M2. Matrix

We have 3 types of matrix. We use `&` to separate columns, `\\` to separate lines in all types.

Syntax:

```
\begin{matrix type}
    1 & pi^2 \\
    x & e
\end{matrix type}
```

`matrix` for $\begin{matrix}
    1 & pi^2 \\
    x & e
\end{matrix}$

`pmatrix` for $\begin{pmatrix}
    1 & pi^2 \\
    x & e
\end{pmatrix}$

`bmatrix}` for $\begin{bmatrix}
    1 & pi^2 \\
    x & e
\end{bmatrix}$

### M3. Inline-code

Use \`code\` for `code`. You can type some special symbols without slash here.

### M4. Table

12. 表格支持

```
| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |
```

for

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |
