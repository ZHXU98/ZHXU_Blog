## (最短路小变形_dijkstra+priority_q)_poj2253_Frogger

题目连接：<http://poj.org/problem?id=2253>

题意：给出一个无向图,求一条1~2的路径使得路径上的最大边权最小.  
分析：dij将距离更新改成取最大值即可，即d[i]表示到达i点过程中的最大边权，更新后可能多个，再靠优先队列取出最小的最大边权。  
精度问题 C++过

这里改变了松弛条件

    
    
    if(d[e.second]>max(e.first,d[v])){
        d[e.second]=max(d[v],e.first);//这样就可以保证 d存的是每次的最长边了 
        //que.push(P(d[e.second],e.second));
    }

0ms飘过 实验了一下其他没有用堆去维护的 大部分都是16ms 和 32 ms

    
    
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
    const int maxn=200+5;
    const int mod=1e9+7;
    const int INF=0x3f3f3f3f;
    
    typedef pair<double,int> P;
    vector<P> G[maxn];
    bool vis[maxn];
    int n,m;
    double d[maxn];
    int cas;
    
    double jl(double x1,double y1,double x2,double y2){
        return sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));
    }
    
    void dijkstra(int s){
        priority_queue<P,vector<P>,greater<P> > que;
        for(int i=0;i<n;i++) {
            d[i]=1e9;
            vis[i]=0;   
        }
        d[s]=0;
        que.push(P(0,s));
        while(!que.empty()){
            P p=que.top();que.pop();
            int v=p.second;
            if(vis[v]) continue;//我们每次弹出的已经是最短路中要的边 所以已经确保了最短路
            vis[v]=1; //所以在之后 松弛时 直接存每次 最长边就好
            for(int i=0;i<G[v].size();i++){
                P e=G[v][i];
                if(d[e.second]>max(e.first,d[v])){
                    d[e.second]=max(d[v],e.first);
                    que.push(P(d[e.second],e.second));
                }
            }
        }   
        printf("Scenario #%d\n",cas++);
        printf("Frog Distance = %.3lf\n\n",d[1]);
    }
    
    double a[maxn],b[maxn];
    
    int main(){
        cas=1;
        while(cin>>n&&n){
            for(int i=0;i<n;i++) G[i].clear();
            for(int i=0;i<n;i++){
                scanf("%lf %lf",&a[i],&b[i]);
            }
            for(int i=0;i<n;i++){
                for(int j=0;j<n;j++){
                    if(i==j)  continue;
                    else{
                        G[i].push_back(P(jl(a[i],b[i],a[j],b[j]),j));
                    }
                }
            }
            dijkstra(0);
        }
        return 0;
    }

