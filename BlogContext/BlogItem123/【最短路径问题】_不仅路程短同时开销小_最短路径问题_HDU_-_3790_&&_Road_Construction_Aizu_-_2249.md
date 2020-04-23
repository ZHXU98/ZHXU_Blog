## 【最短路径问题】_不仅路程短同时开销小_最短路径问题_HDU_-_3790_&&_Road_Construction_Aizu_-_2249

最短路径问题 HDU - 3790  
<http://acm.hdu.edu.cn/showproblem.php?pid=3790>

给你n个点，m条无向边，每条边都有长度d和花费p，给你起点s终点t，要求输出起点到终点的最短距离及其花费，如果最短距离有多条路线，则输出花费最少的。  
Input  
输入n,m，点的编号是1~n,然后是m行，每行4个数 a,b,d,p，表示a和b之间有一条边，且其长度为d，花费为p。最后一行是两个数
s,t;起点s，终点。n和m为0时输入结束。  
(1

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <cmath>
    #include <algorithm>
    #include <queue>
    #include <vector>
    using namespace std;
    
    const int maxn = 2000 + 5;
    const int INF = 0x3f3f3f3f;
    int n, m;
    
    struct E {
        int to, dist, cost; //多个权而已
        E(int a=0,int b=0,int c=0):to(a),dist(b),cost(c){}
    };
    
    vector<E> G[maxn];
    int d[maxn], pri[maxn];
    bool vis[maxn];
    typedef pair<int, int> P;
    
    int main() {
        ios::sync_with_stdio(false);
        while (cin >> n >> m &&n+m) {
            for (int i = 0; i <= n; i++) G[i].clear();//多组 记得初始化弄好
    
            int st, ed, dis, ct;
            for (int i = 0; i < m; i++) {
                cin >> st >> ed >> dis >> ct;
        //      --st; --ed;
                G[st].push_back(E(ed, dis, ct));
                G[ed].push_back(E(st, dis, ct));
            }
    
            cin>>st>>ed;
    
        //  st--,ed--;
    
            memset(vis,0,sizeof(vis));
            memset(d, INF, sizeof(d));
            memset(pri, 0, sizeof(pri));
    
            priority_queue<P,vector<P>, greater<P> > que;
    
            d[st] = 0;
            que.push(P(0, st));
    
            while (!que.empty()) {
                P p = que.top(); que.pop();
                int v = p.second;
                if (vis[v] || d[v] < p.first) continue;
                vis[v]=1;
                for (int i = 0; i < G[v].size(); i++) {
                    E e = G[v][i];
                    if (d[e.to] > d[v] + e.dist) {
                        d[e.to] = d[v] + e.dist;
                        que.push(P(d[e.to], e.to));
                        pri[e.to] = e.cost+pri[v];
                    }
                    else if (d[e.to] == d[v] + e.dist) {// 路径一样 在考虑更新花费
                        pri[e.to] = min(pri[e.to], e.cost+pri[v]);
                    }
                }
            }
            cout<<d[ed]<<" "<<pri[ed]<<endl;
        }
        return 0;
    }

Road Construction Aizu - 2249  
<http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2249>  
<https://vjudge.net/problem/Aizu-2249>  
不过这次 是求1点到其他点最短 同时 到每点开销最少  
上面那个题代码其实是这里我改过去的。。

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <cmath>
    #include <algorithm>
    #include <queue>
    #include <vector>
    using namespace std;
    
    const int maxn = 10000 + 5;
    const int INF = 0x3f3f3f3f;
    int n, m;
    
    struct E {
        int to, dist, cost;
        E(int a=0,int b=0,int c=0):to(a),dist(b),cost(c){}
    };
    
    vector<E> G[maxn];
    int d[maxn], pre[maxn];
    typedef pair<int, int> P;
    
    int main() {
        while (cin >> n >> m &&n) {
            for (int i = 0; i < n; i++) G[i].clear();
    
            int st, ed, dis, ct;
            for (int i = 0; i < m; i++) {
                cin >> st >> ed >> dis >> ct;
                --st; --ed;
                G[st].push_back(E(ed, dis, ct));
                G[ed].push_back(E(st, dis, ct));
            }
    
            memset(d, INF, sizeof(d));
            memset(pre, INF, sizeof(pre));
            priority_queue<P,vector<P>, greater<P> > que;
    
            d[0] = 0;
            que.push(P(0, 0));
    
            while (!que.empty()) {
                P p = que.top(); que.pop();
                int v = p.second;//dian wei //first shi mudidi
                if (d[v] < p.first) continue;
                for (int i = 0; i < G[v].size(); i++) {
                    E e = G[v][i];
                    if (d[e.to] > d[v] + e.dist) {
                        d[e.to] = d[v] + e.dist;
                        que.push(P(d[e.to], e.to));//因为可能是个树 所以不能像上面那样固定点 直接找就出ans
                        pre[e.to] = e.cost;//这里就改成 最短路那个路应该花费的时间了
                    }
                    else if (d[e.to] == d[v] + e.dist) {
                        pre[e.to] = min(pre[e.to], e.cost);
                    }
                }
            }
            int ans = 0;
            for (int i = 1; i < n; i++) {
                ans += pre[i];
            }
            cout << ans << endl;
        }
        return 0;
    }

