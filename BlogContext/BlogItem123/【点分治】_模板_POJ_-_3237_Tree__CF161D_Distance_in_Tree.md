## 【点分治】_模板_POJ_-_3237_Tree__CF161D_Distance_in_Tree

首先理解模板

一.概念  
​ 是处理树上路径的一个极好的方法。如果你需要大规模的处理一些树上路径的问题时，点分治是一个离线的方法

一般而言 对于一棵树 我们能选取一个点 将其分割为几个棵子树 如果想要dfs每次进行的少 我们最好就要找到数得重心 这样深度就变为了 log2(n)

    
    
    int siz[maxn],SIZ,dp[maxn],root; // 这里部分是之后用的
    bool use[maxn];
    
    void get_root(int u,int fa){
        dp[u]=0,siz[u]=1;
        for (int i=head[u];i;i=nxt[i]) {
            int v=to[i];
            if (v==fa||use[v]) continue;
            get_root(v,u);
            dp[u]=max(dp[u],siz[v]);
            siz[u]+=siz[v];
        }
        dp[u]=max(dp[u],SIZ-siz[u]);
        if (dp[u]<dp[root]) root=u;
    }
    

分治代码  
dfs 处理出子树到跟的距离 然后存到d里面  
然后答案计数  
get_ans(x,0，1); 这一句的作用是将答案加上经过x的路径答案。 而这一个0是为了解决掉一些，有重复计算的结果；（看不懂先假装没有这个0）  
get_ans（[v.to](http://v.to),edges[i].cost,-1);
这一句是将在既经过x这个点，又经过v.to这一个点的路径来去重。因为像这种路径会在get_ans（x,0)和get_ans([v.to](http://v.to),0)中都计算一次。所以在减一次  
S = size[[v.to](http://v.to)];
现在我们要分治v.to的这一颗子树，又将求重心的树的大小改为size[[v.to](http://v.to)];

    
    
    void solve(int u) {
        get_ans(u,0,1);
        use[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(use[v]) continue;
            get_ans(v,dis[i],-1);
            root=0,SIZ=siz[v],dp[0]=n;
            get_root(v,u);
            solve(root);
        }
    }
    

图例  
把树分成2部分后 我们找的是路过root的点相对距离长度 但是会有一种情况 我们必须剪掉 她是不合法的![在这里插入图片描述](https://img-
blog.csdnimg.cn/20190515132548605.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
路过根节点 但是在同一个子树中 显然我们加多了

给定一棵有n个点的树

询问树上距离为k的点对是否存在。  
这代码实际上是N^2的 之后又更低复杂度的 在下2题

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    //#define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    const long long mod = 1e8;
    const int maxn = 4e4+5;
    
    int n,m,k;
    
    int cnt,head[maxn];
    int to[maxn<<1],nxt[maxn<<1],dis[maxn<<1];
    
    void ade(int a,int b,int c) {
        to[++cnt]=b;
        nxt[cnt]=head[a];
        dis[cnt]=c;
        head[a]=cnt;
    }
    
    int siz[maxn],SIZ,dp[maxn],root;
    bool use[maxn];
    
    void get_root(int u,int fa){
        dp[u]=0,siz[u]=1;
        for (int i=head[u];i;i=nxt[i]) {
            int v=to[i];
            if (v==fa||use[v]) continue;
            get_root(v,u);
            dp[u]=max(dp[u],siz[v]);
            siz[u]+=siz[v];
        }
        dp[u]=max(dp[u],SIZ-siz[u]);
        if (dp[u]<dp[root]) root=u;
    }
    
    int d[maxn],tail;
    
    void get_dist(int u,int fa,int len) {
        d[++tail]=len;
        siz[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(v==fa||use[v]) continue ;
            get_dist(v,u,len+dis[i]);
            siz[u]+=siz[v];
        }
    }
    
    int ans[int(1e6)+5];
    
    void get_ans(int u,int len,int w) {
        d[tail=0]=0;
        get_dist(u,0,len);
        int l=1,r=tail,res=0;
        for(int i=1;i<=tail;i++)
            for(int j=1;j<=tail;j++)
            if(i!=j) ans[d[i]+d[j]]+=w;
    }
    
    void solve(int u) {
        get_ans(u,0,1);
        use[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(use[v]) continue;
            get_ans(v,dis[i],-1);
            root=0,SIZ=siz[v],dp[0]=n;
            get_root(v,u);
            solve(root);
        }
    }
    
    signed main() {
        fastio;
        cin>>n>>m;
        for(int i=1; i<n; i++) {
            int a,b,c;
            cin>>a>>b>>c;
            ade(a,b,c),ade(b,a,c);
        }
        dp[0]=SIZ=n,root=0;
        get_root(1,0);
        solve(root);
        while(m--){
            cin>>k;
            if(ans[k]) cout<<"AYE"<<endl;
            else cout<<"NAY"<<endl;
        }
        return 0;
    }
    

**CF161D Distance in Tree**

输入点数为N一棵树

求树上长度恰好为K的路径个数  
这里用二分 压复杂度 k-一个子树到根节点距离 在另一个子树查 存在这个数据对于 就是一对点

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    #define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    const long long mod = 1e8;
    const int maxn = 5e4+5;
    
    int n,m,k;
    
    int cnt,head[maxn];
    int to[maxn<<1],nxt[maxn<<1],dis[maxn<<1];
    
    void ade(int a,int b,int c) {
        to[++cnt]=b;
        nxt[cnt]=head[a];
        dis[cnt]=c;
        head[a]=cnt;
    }
    
    int siz[maxn],SIZ,dp[maxn],root;
    bool use[maxn];
    
    void get_root(int u,int fa) {
        dp[u]=0,siz[u]=1;
        for (int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if (v==fa||use[v]) continue;
            get_root(v,u);
            dp[u]=max(dp[u],siz[v]);
            siz[u]+=siz[v];
        }
        dp[u]=max(dp[u],SIZ-siz[u]);
        if (dp[u]<dp[root]) root=u;
    }
    
    int d[maxn],tail;
    
    void get_dist(int u,int fa,int len) {
        d[++tail]=len;
        siz[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(v==fa||use[v]) continue ;
            get_dist(v,u,len+dis[i]);
            siz[u]+=siz[v];
        }
    }
    
    int ans;
    
    void get_ans(int u,int len,int w) {
        d[tail=0]=0;
        get_dist(u,0,len);
        sort(d+1,d+1+tail);
        for (int i=1; i<=tail; i++) {
            int t1=lower_bound(d+1,d+1+tail,k-d[i])-d;
            int t2=upper_bound(d+1,d+1+tail,k-d[i])-d-1;
            if (t2<t1) continue;
            if (t1==0) continue;
            if ((d[t1]!=k-d[i])) continue;
            ans+=(w)*(t2-t1+1);
            if (i>=t1&&i<=t2) ans-=w;
        }
    }
    
    void solve(int u) {
        get_ans(u,0,1);
        use[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(use[v]) continue;
            get_ans(v,dis[i],-1);
            root=0,SIZ=siz[v],dp[0]=n;
            get_root(v,u);
            solve(root);
        }
    }
    
    signed main() {
        fastio;
        cin>>n>>k;
        for(int i=1; i<n; i++) {
            int a,b,c;
            cin>>a>>b;
            ade(a,b,1),ade(b,a,1);
        }
        dp[0]=SIZ=n,root=0;
        get_root(1,0);
        solve(root);
        cout<<(ans>>1)<<endl;
        return 0;
    }
    

**Tree POJ - 3237**  
给你一棵TREE,以及这棵树上边的距离.问有多少对点它们两者间的距离小于等于K

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    //#define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    const long long mod = 1e8;
    const int maxn = 4e4+5;
    
    int n,m,k,ans;
    
    int cnt,head[maxn];
    int to[maxn<<1],nxt[maxn<<1],dis[maxn<<1];
    
    void ade(int a,int b,int c) {
        to[++cnt]=b;
        nxt[cnt]=head[a];
        dis[cnt]=c;
        head[a]=cnt;
    }
    
    int siz[maxn],SIZ,dp[maxn],root;
    bool use[maxn];
    
    void get_root(int u,int fa){
        dp[u]=0,siz[u]=1;
        for (int i=head[u];i;i=nxt[i]) {
            int v=to[i];
            if (v==fa||use[v]) continue;
            get_root(v,u);
            dp[u]=max(dp[u],siz[v]);
            siz[u]+=siz[v];
        }
        dp[u]=max(dp[u],SIZ-siz[u]);
        if (dp[u]<dp[root]) root=u;
    }
    
    int d[maxn],tail;
    
    void get_dist(int u,int fa,int len) {
        d[++tail]=len;
        siz[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(v==fa||use[v]) continue ;
            get_dist(v,u,len+dis[i]);
            siz[u]+=siz[v];
        }
    }
    
    int get_ans(int u,int len) {
        d[tail=0]=0;
        get_dist(u,0,len);
        sort(d+1,d+1+tail);
        int l=1,r=tail,res=0;
        while(l<=r) {
            if(d[l]+d[r]<=k) res+=r-l,l++;
            else r--;
        }
        return res;
    }
    
    void solve(int u) {
        ans+=get_ans(u,0);
        use[u]=1;
        for(int i=head[u]; i; i=nxt[i]) {
            int v=to[i];
            if(use[v]) continue;
            ans-=get_ans(v,dis[i]);
            root=0,SIZ=siz[v],dp[0]=n;
            get_root(v,u);
            solve(root);
        }
    }
    
    signed main() {
        fastio;
        cin>>n;
        for(int i=1; i<n; i++) {
            int a,b,c;
            cin>>a>>b>>c;
            ade(a,b,c),ade(b,a,c);
        }
        cin>>k;
        dp[0]=SIZ=n,root=0;
        get_root(1,0);
        solve(root);
        cout<<ans<<endl;
        return 0;
    }
    

