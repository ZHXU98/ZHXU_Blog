##
【链式前向星+树的直径】_2018年小白月赛6_C题_桃花_&&_Roads_in_the_North_POJ_-_2631_&&_Cow_Marathon_POJ_-_1985

    
    
    #define MAXM 500010
    #define MAXN 10010
    
    /*
    1 结构体数组edge存边，edge[i]表示第i条边,
    2 head[i]存以i为起点的第一条边(在edge中的下标)
    */
    struct EDGE{
        int next;   //下一条边的存储下标 
        int to;     //这条边的终点 
        int w;      //权值 
    }; 
    
    EDGE edge[MAXM];
    
    int n, m, cnt;
    int head[MAXN];  //head[i]表示以i为起点的第一条边 
    
    void Add(int u, int v, int w) {  //起点u, 终点v, 权值w 
        edge[++cnt].next = head[u];
        edge[cnt].w = w;
        edge[cnt].to = v;
        head[u] = cnt;    //第一条边为当前边 
    }
    /*
    2.增边：若以点i为起点的边新增了一条，在edge中的下标为j.
    那么edge[j].next=head[i];然后head[i]=j.
    即每次新加的边作为第一条边，最后倒序遍历
    ps 每次更新 都把上次存的边 结构体里next指向他 然后更新他 把j边更新到head里
    head里存的 可以说是 最后那个边出现的位置 二结构体每次存的都是上一次的 所以 就有办法遍历了
    */

以下是牛客  
链接：<https://www.nowcoder.com/acm/contest/136/C>  
来源：牛客网

时间限制：C/C++ 1秒，其他语言2秒  
空间限制：C/C++ 262144K，其他语言524288K  
64bit IO Format: %lld

题目描述  
桃花一簇开无主，可爱深红映浅红。 ——《题百叶桃花》
桃花长在桃树上，树的每个节点有一个桃花，调皮的HtBest想摘尽可能多的桃花。HtBest有一个魔法棒，摘到树上任意一条链上的所有桃花，由于HtBest法力有限，只能使用一次魔法棒，请求出Htbest最多可以摘到多少个桃花。  
输入描述:  
第一行有一个正整数n，表示桃树的节点个数。接下来n-1行，第i行两个正整数ai,bi ，表示桃树上的节点ai,bi之间有一条边。  
输出描述:  
第一行一个整数，表示HtBest使用一次魔法棒最多可以摘到多少桃花。  
对于100%的测试数据：  
1 ≤ n ≤ 1000000  
数据量较大，注意使用更快的输入输出方式。  
输入  
3  
1 2  
2 3  
输出  
3

以下两种实现  
分别 BFS和dfs 遍历方式 都是for 一直到I==-1 停止  
算是求某点到其他点距离的更快方法。。。  
刚做这题 用 dijkstra2次找最远点 炸了

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <queue>
    #include <stack>
    #include <set>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    const int maxn=1000000+5;
    int head[maxn];
    bool vis[maxn];
    int cnt,n,u,v;
    int d[maxn];
    
    
    struct node{
        int next,to,cost;
        node(int t,int n,int c){next=n;to=t;cost=c;}
        node(){};
    }edge[maxn<<1];
    
    void addedge(int u,int v,int c){
        edge[cnt]=node(u,head[v],c);
        head[v]=cnt++;
        edge[cnt]=node(v,head[u],c);
        head[u]=cnt++;
    }
    
    int pos,dis;
    
    void dfs(int u){
        vis[u]=1;
        for(int i=head[u];i!=-1;i=edge[i].next){
            if(!vis[edge[i].to]){
                d[edge[i].to]=d[u]+1;
                dfs(edge[i].to);
            }
        }
    }
    /*
    void bfs(int u){
        memset(vis,0,sizeof(vis));
        memset(d,0,sizeof(d));
        queue<int> q;
        q.push(u);
        d[u]=1;
        vis[u]=1;
        while(!q.empty()){
            int t=q.front();q.pop();
            for(int i=head[t];i!=-1;i=edge[i].next){
                if(!vis[edge[i].to]){
                    vis[edge[i].to]=1;
                    dis=d[edge[i].to]=d[t]+1;
                    pos=edge[i].to;
                    q.push(edge[i].to);
                }
            }
        }
    }
    */
    int main(){
        while(cin>>n){
        cnt=0;
        memset(head,-1,sizeof(head));
            for(int i=0;i<n-1;i++){
                scanf("%d %d",&u,&v);
                addedge(u,v,1);
            }
            /*
            bfs(1);
            bfs(pos);
            cout<<dis<<endl;
            */
            memset(vis,0,sizeof(vis));
            memset(d,0,sizeof(d));
            d[1]=1;
            dfs(1);
            int pos=1;
            for(int i=1;i<=n;i++){
                if(d[i]>d[pos])
                    pos=i;
            }
            memset(vis,0,sizeof(vis)); 
            d[pos]=1;
            dfs(pos);
            dis=0;
            for(int i=1;i<=n;i++){
                if(d[i]>dis) dis=d[i];
            }
            printf("%d\n",dis);
        }
        return 0;
    }

Roads in the North POJ - 2631  
<http://poj.org/problem?id=2631>  
这题用的 前项星 我也写不出向上面水题 那样好写的DFS了。。。orz

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <queue>
    #include <stack>
    #include <set>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    const int maxn=1000000+5;
    int head[maxn];
    bool vis[maxn];
    int cnt,n,u,v,c;
    int d[maxn];
    
    struct node{
        int next,to,cost;
        node(int n,int t,int c){next=n;to=t;cost=c;}
        node(){};
    }edge[maxn<<1];
    
    void addedge(int u,int v,int c){
        edge[cnt]=node(head[u],v,c);
        head[u]=cnt++;
        edge[cnt]=node(head[v],u,c);
        head[v]=cnt++;
    }
    
    int pos,dis;
    int ans; 
    
    void bfs(int u){
        memset(vis,0,sizeof(vis));
        memset(d,0,sizeof(d));
        queue<int> q;
        q.push(u);
        d[u]=0;
        vis[u]=1;
        while(!q.empty()){
            int t=q.front();q.pop();
            for(int i=head[t];i!=-1;i=edge[i].next){
                if(!vis[edge[i].to]){
                    vis[edge[i].to]=1;
                    dis=d[edge[i].to]=d[t]+edge[i].cost;
                    if(dis>ans){//多个判断更新
                        ans=max(dis,ans);
                        pos=edge[i].to;
                    }
                    q.push(edge[i].to);
                }
            }
        }
    }
    
    int main(){ 
    memset(head,-1,sizeof(head));
    cnt=0;
        while(~scanf("%d %d %d",&u,&v,&c)){
            addedge(u,v,c); 
        }
        dis=0;
        bfs(1);
        ans=0;
        bfs(pos);
        printf("%d\n",ans);
        return 0;
    }

Cow Marathon POJ - 1985  
<http://poj.org/problem?id=1985>  
板子题了 成。。。。orz

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <queue>
    #include <stack>
    #include <set>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    const int maxn=1000000+5;
    int head[maxn];
    bool vis[maxn];
    int cnt,n,u,v,c;
    int d[maxn];
    
    struct node{
        int next,to,cost;
        node(int n,int t,int c){next=n;to=t;cost=c;}
        node(){};
    }edge[maxn<<1];
    
    void addedge(int u,int v,int c){
        edge[cnt]=node(head[u],v,c);
        head[u]=cnt++;
        edge[cnt]=node(head[v],u,c);
        head[v]=cnt++;
    }
    
    int pos,dis;
    int ans; 
    
    void bfs(int u){
        memset(vis,0,sizeof(vis));
        memset(d,0,sizeof(d));
        queue<int> q;
        q.push(u);
        d[u]=0;
        vis[u]=1;
        while(!q.empty()){
            int t=q.front();q.pop();
            for(int i=head[t];i!=-1;i=edge[i].next){
                if(!vis[edge[i].to]){
                    vis[edge[i].to]=1;
                    dis=d[edge[i].to]=d[t]+edge[i].cost;
                    if(dis>ans){//多个判断更新
                        ans=max(dis,ans);
                        pos=edge[i].to;
                    }
                    q.push(edge[i].to);
                }
            }
        }
    }
    
    int main(){ 
    memset(head,-1,sizeof(head));
    cnt=0;
    int n,m;
    char cmd[10];
        while(~scanf("%d %d ",&n,&m)){
            for(int i=0;i<m;i++){
                scanf("%d %d %d %s",&u,&v,&c,cmd);
                addedge(u,v,c); 
            }
            dis=0;
            bfs(1);
            ans=0;
            bfs(pos);
            printf("%d\n",ans);
        }
    
        return 0;
    }

附同学写的 感觉写起来方便点。。orz

    
    
    #include<vector>
    #include<string.h>
    using namespace std;
    const int maxn = 1e5;
    const int INF =1e9;
    vector<int> G[maxn + 5];
    vector<int> V[maxn + 5];
    int vis[maxn + 5];
    int d[maxn + 5];
    void dfs(int u){
        vis[u] = 1;
        for(int i = 0; i < G[u].size() ;i++){
            int v = G[u][i];
            if(!vis[v]){
                d[v] = d[u] + V[u][i];
                dfs(v);
            }
        }
    }
    
    int main(){
        int n,m;
        scanf("%d%d",&n,&m);
    
        int u,v,w;
        char c;
        for(int i = 0; i < m; i++){
            scanf("%d%d%d",&u,&v,&w);
            scanf(" %c",&c);
            G[u].push_back(v );
            V[u].push_back(w);
            G[v].push_back(u);
            V[v].push_back(w);
        }
        memset(vis,0,sizeof(vis));
       // memset(d, INF, sizeof(d));
    
        for(int i = 1; i <= n ;i++) d[i] = i == 1? 0:INF;
    
        dfs(1);
    
        int maxn = -1;
        int flag;
        for(int i = 1; i <= n ;i++){
            if(d[i] > maxn && d[i] != INF){
                maxn = d[i];
                flag = i;
            }
        }
    
        memset(vis, 0, sizeof(vis));
        for(int i = 1; i <= n ; i++) d[i] = i== flag?0:INF;
        dfs(flag);
    
        maxn = -1;
        for(int i = 1; i <= n; i++){
            if(d[i] > maxn && d[i] != INF){
                maxn = d[i];
            }
        }
        printf("%d\n",maxn);
    }
    
    

