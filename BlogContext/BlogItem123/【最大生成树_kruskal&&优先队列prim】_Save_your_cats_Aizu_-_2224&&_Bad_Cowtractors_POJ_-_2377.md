##
【最大生成树_kruskal&&优先队列prim】_Save_your_cats_Aizu_-_2224&&_Bad_Cowtractors_POJ_-_2377

Save your cats Aizu - 2224

Nicholas Y. Alford was a cat lover. He had a garden in a village and kept many
cats in his garden. The cats were so cute that people in the village also
loved them.  
One day, an evil witch visited the village. She envied the cats for being
loved by everyone. She drove magical piles in his garden and enclosed the cats
with magical fences running between the piles. She said “Your cats are shut
away in the fences until they become ugly old cats.” like a curse and went
away.  
Nicholas tried to break the fences with a hummer, but the fences are
impregnable against his effort. He went to a church and asked a priest help.
The priest looked for how to destroy the magical fences in books and found
they could be destroyed by holy water. The Required amount of the holy water
to destroy a fence was proportional to the length of the fence. The holy water
was, however, fairly expensive. So he decided to buy exactly the minimum
amount of the holy water required to save all his cats. How much holy water
would be required?  
Input  
The input has the following format:  
N M  
x1 y1  
.  
.  
.  
xN yN  
p1 q1  
.  
.  
.  
pM qM  
The first line of the input contains two integers N (2 ≤ N ≤ 10000) and M (1 ≤
M). N indicates the number of magical piles and M indicates the number of
magical fences. The following N lines describe the coordinates of the piles.
Each line contains two integers xi and yi (-10000 ≤ xi, yi ≤ 10000). The
following M lines describe the both ends of the fences. Each line contains two
integers pj and qj (1 ≤ pj, qj ≤ N). It indicates a fence runs between the pj-
th pile and the qj-th pile.  
You can assume the following:  
No Piles have the same coordinates.  
A pile doesn’t lie on the middle of fence.  
No Fences cross each other.  
There is at least one cat in each enclosed area.  
It is impossible to destroy a fence partially.  
A unit of holy water is required to destroy a unit length of magical fence.  
Output  
Output a line containing the minimum amount of the holy water required to save
all his cats. Your program may output an arbitrary number of digits after the
decimal point. However, the absolute error should be 0.001 or less.  
Sample Input 1  
3 3  
0 0  
3 0  
0 4  
1 2  
2 3  
3 1  
Output for the Sample Input 1  
3.000  
Sample Input 2  
4 3  
0 0  
-100 0   
100 0  
0 100  
1 2  
1 3  
1 4  
Output for the Sample Input 2  
0.000  
Sample Input 3  
6 7  
2 0  
6 0  
8 2  
6 3  
0 5  
1 7  
1 2  
2 3  
3 4  
4 1  
5 1  
5 4  
5 6  
Output for the Sample Input 3  
7.236  
Sample Input 4  
6 6  
0 0  
0 1  
1 0  
30 0  
0 40  
30 40  
1 2  
2 3  
3 1  
4 5  
5 6  
6 4  
Output for the Sample Input 4  
31.000  
简单翻译过来 是给n个坐标 后面m个 是联通情况

问你破坏最少是多少

其实就是破话掉环上最小的一个边 直到没有环

考虑最（小）大生成树 和边的和 相减

    
    
    #include <iostream>
    #include <stack>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    using namespace std;
    typedef long long ll;
    
    const int maxn=10000+10;
    int pos,n,m;
    
    struct egde{
        int u,v;
        double dis;
        egde(){};
        egde(int a,int b,double c){u=a,v=b,dis=c;}
        bool operator<(const egde &a) const {return dis > a.dis; }
    
    };
    egde es[maxn*10],e;// 太小过不去。。。 边也没有个数 m x2吧 得写下面 所以试了几次 x10过了
    
    struct node{
        int x,y;
    }p[maxn];
    
    int pre[maxn];
    
    void init(){
        for(int i=0;i<n;i++){
            pre[i]=i;
        }
    }
    
    int finds(int x){
        return pre[x]!=x?pre[x]=finds(pre[x]):x;
    }
    
    void unions(int x,int y){
        x=finds(x),y=finds(y);
        if(x!=y){
            pre[x]=y;
        }
    }
    
    double Kruskal(){
        sort(es,es+pos);
        init();
        double res=0;
        for(int i=0;i<pos;i++){
             e = es[i];
            if(finds(e.u)!=finds(e.v)){
                unions(e.u,e.v);
                res+=e.dis;
            }
        }
        return res;
    }
    
    int main(){
        while(cin>>n>>m){
            int x,y;
            for(int i=0;i<n;i++){
                cin>>p[i].x>>p[i].y;    
            }
            pos=0;
            double sum=0;
            for(int i=0;i<m;i++){
                cin>>x>>y;
                x--;y--;
                double t=sqrt((p[x].x-p[y].x)*(p[x].x-p[y].x)+(p[x].y-p[y].y)*(p[x].y-p[y].y));
                es[pos++]=egde(x,y,t);
                es[pos++]=egde(y,x,t);
                sum+=t;
            }
            printf("%.3lf\n",sum-Kruskal());
        }
        return 0;
    }

这里 我补一个 用堆 优先队列 prim 写的最大生成树  
Bad Cowtractors POJ - 2377  
<http://poj.org/problem?id=2377>

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <queue>
    #include <stack>
    #include <map>
    using namespace std;
    typedef unsigned long long ull;
    typedef long long ll;
    const int maxn=1e3+5;
    const int mod=1e9+7;
    const int INF=0x3f3f3f;
    
    typedef pair<int,int> P;
    vector<P> G[maxn];
    bool vis[maxn];
    int n,m;
    int d[maxn];
    
    
    int prim_p(int s){
        int res=0;
        priority_queue<P,vector<P>,less<P> > que;
        for(int i=0;i<=n;i++) {
            d[i]=-INF;
            vis[i]=0;   
        }
        d[s]=0;
        que.push(P(0,s));
        while(!que.empty()){
            P p=que.top();que.pop();
            int v=p.second;
            if(vis[v]||d[v]>p.first) continue;
            vis[v]=1;
            res+=d[v];
            for(int i=0;i<G[v].size();i++){
                P e=G[v][i];
                if(d[e.second]<e.first){
                    d[e.second]=e.first;
                    que.push(P(d[e.second],e.second));
                }
            }
        }  
        return res; 
    }
    
    int main(){
        int st,ed,dis;
        while(cin>>n>>m&&n){
            for(int i=0;i<m;i++){
                cin>>st>>ed>>dis;
                G[st].push_back(P(dis,ed));
                G[ed].push_back(P(dis,st));
            }
    
            int ans=prim_p(1);
            for(int i=1;i<=n;i++){//注意孤立点判断
                if(d[i]<0){
                     ans=-1;
                     break;
                }
            }
            if(ans<=0) cout<<-1<<endl;
            else cout<<ans<<endl;
        }
        return 0;
    }

480ms 的确是我没有写好

例外附一个 Kruskal写的 68ms

    
    
    #include<set>
    #include<map>
    #include<queue>
    #include<stack>
    #include<bitset>
    #include<math.h>
    #include<string>
    #include<vector>
    #include<cstdio>
    #include<cstring>
    #include<iostream>
    #include<algorithm>
    #define pi acos(-1)
    
    using namespace std;
    typedef long long ll;
    const int maxn = 1e6+5;
    const int INF = 0x3f3f3f3f;
    const double EPS = 1e-10;
    const ll INF_ll = 0x7fffffffffffffff;
    ll mod = 1e9+7;
    
    int n,m;
    struct node{
        int u,v;
        int cost;
    }T[maxn];
    int pre[maxn];
    bool cmp(node a, node b){
        return a.cost> b.cost;
    }
    int finds(int x)
    {
        return pre[x] == x?x:pre[x]=finds(pre[x]);
    }
    int unions(int x,int y){
        int xx1 = finds(x);
        int yy1 = finds(y);
    
        if(xx1 == yy1) return 0;
        else{
            if(xx1 < yy1){
                pre[yy1] =xx1;
    
            }
            else pre[xx1] =yy1;
        }
        return 1;
    }
    int main()
    {
        scanf("%d%d",&n,&m);
        for(int i = 0; i <= n; i++) pre[i] = i;
        for(int i = 0; i < m ;i++){
            scanf("%d%d%d",&T[i].u,&T[i].v,&T[i].cost);
        }
        sort(T, T + m ,cmp);
        ll ans1 = 0;
        int k = 0;
        for(int i = 0; i < m ;i++){
            if(unions(T[i].u,T[i].v)){
                ans1 += T[i].cost;
                k++;
            }
            if(k == n - 1) break;
        }
        int i;
        for(i=1;i<=n;i++) {
        //  cout<<finds(i)<<"    "<<finds(1)<<endl;
            if(finds(i)==finds(1))
                {
                    if(i==n) printf("%lld\n",ans1);
                }
            else break;
        }
        if(i!=n+1) cout<<-1<<endl;
    }

