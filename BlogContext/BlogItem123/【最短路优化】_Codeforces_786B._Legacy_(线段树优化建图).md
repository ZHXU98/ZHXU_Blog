## 【最短路优化】_Codeforces_786B._Legacy_(线段树优化建图)

这建立图的方式 网络流 也可以出 只能说 出题人毒瘤啊  
图是类似线段树结构 动态开点  
参考来源  
https://blog.csdn.net/KIDGIN7439/article/details/83623451

> 线段树优化建图。  
>  建立两棵线段树，其上点的点权分别表示“到达这个区间内所有点的最小花费”和“到达这个区间内任意一个点的最小花费”。  
>  对于第一种路直接加边即可  
>  对于第二种路，添加从v到第一棵线段树对应区间中的点的边  
>  对于第三种路，添加从第二棵线段树对应区间中的点到v的边
    
    
    #include <bits/stdc++.h>
    //#define int long long
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    typedef long long ll;
    const int maxn = 2e6 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef pair<int, int> P;
    int cas, n, m, s;
    
    int head[maxn], cnt;
    int to[maxn << 1], nxt[maxn << 1];
    ll val[maxn << 1];
    int inout[2][maxn], tot;
    
    void ade(int a, int b, int c) {
        to[++ cnt] = b, val[cnt] = c;
        nxt[cnt] = head[a], head[a] = cnt;
    }
    
    void build(int id, int l, int r, int rt) {
        inout[id][rt] = ++ tot;
        if(l == r) {
            if(id == 0) ade(inout[id][rt], l, 0);
            else ade(l, inout[id][rt], 0);
            return ;
        }
        int mid = l + r >> 1;
        build(id, l, mid, rt << 1);
        build(id, mid + 1, r, rt << 1 | 1);
        if(id == 0) {
            ade(inout[id][rt], inout[id][rt << 1], 0);
            ade(inout[id][rt], inout[id][rt << 1 | 1], 0);
        } else {
            ade(inout[id][rt << 1], inout[id][rt], 0);
            ade(inout[id][rt << 1 | 1], inout[id][rt], 0);
        }
    }
    
    void updata(int u, int id, int L, int R, int l, int r, int rt, int val) {
        if(L <= l && r <= R) {
            if(id == 0) ade(u, inout[id][rt], val);
            else ade(inout[id][rt], u, val);
            return ;
        }
        int mid = l + r >> 1;
        if(L <= mid) updata(u, id, L, R, l, mid, rt << 1, val);
        if(R > mid) updata(u, id, L, R, mid + 1, r, rt << 1 | 1, val);
    }
    
    queue<int> que;
    bool vis[maxn];
    ll dis[maxn];
    void spfa() {
        memset(vis, 0, sizeof vis);
        memset(dis, 0x3f, sizeof dis);
        que.push(s);
        vis[s] = 1;
        dis[s] = 0;
        while(!que.empty()) {
            int u = que.front(); que.pop();
            vis[u] = 0;
            for(int i = head[u]; i; i = nxt[i]) {
                int v = to[i];
                if(dis[v] > dis[u] + val[i]) {
                    dis[v] = dis[u] + val[i];
                    if(!vis[v]) que.push(v), vis[v] = 1;
                }
            }
        }
    }
    
    // priority_queue<P> que;
    // bool vis[maxn];
    // ll dis[maxn];
    
    // void dj() {
    //     memset(vis, 0, sizeof vis);
    //     memset(dis, 0x3f, sizeof dis);
    //     que.push(P(0, s));
    //     dis[s] = 0;
    //     while(!que.empty()) {
    //         int u = que.top().second; que.pop();
    //         if(vis[u]) continue;
    //         vis[u] = 1;
    //         for(int i = head[u]; i; i = nxt[i]) {
    //             int v = to[i];
    //             if(dis[v] > dis[u] + val[i]) {
    //                 dis[v] = dis[u] + val[i];
    //                 que.push(P(-dis[v], v));
    //             }
    //         }
    //     }
    // }
    
    signed main() {
        fastio;
        cin >> n >> m >> s;
        tot = n;
        build(0, 1, n, 1), build(1, 1, n, 1);
        for(int i = 1, op, u, v, w, l ,r; i <= m; i ++) {
            cin >> op;
            if(op == 1) {
                cin >> u >> v >> w;
                ade(u, v, w);
            } else if(op == 2){
                cin >> u >> l >> r >> w;
                updata(u, 0, l, r, 1, n, 1, w);
            } else {
                cin >> u >> l >> r >> w;
                updata(u, 1, l, r, 1, n, 1, w);
            }
        }
        spfa();
        for(int i = 1; i <= n; i ++) {
            if(i != 1) cout << " ";
            if(dis[i] == INF) cout << -1 ;
            else cout << dis[i];
        }cout << endl;
        return 0;
    }
    

