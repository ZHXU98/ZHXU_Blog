## 2019HDU杭电多校第三场_HDU_6611_K_Subsequence_(最小费用最大流+dijkstra_模版(处理负边))

唉 自己 spfaT了  
之后又写了份 dj的 还是T了  
只能说自己写的好丑啊 一直写spfa 突然不适

以下 标程扒的 以后当模板使用了

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef pair<int, int> pii;
    const int maxn = 1e4;
    const int inf = 0x3f3f3f3f;
    struct edge {
        int to, cap, cost, rev;
        edge() {}
        edge(int to, int _cap, int _cost, int _rev) :to(to), cap(_cap), cost(_cost), rev(_rev) {}
    };
    int V, H[maxn + 5], dis[maxn + 5], PreV[maxn + 5], PreE[maxn + 5];
    vector<edge> G[maxn + 5];
    void init(int n) {
        V = n;
        for (int i = 0; i <= V; ++i)G[i].clear();
    }
    void AddEdge(int from, int to, int cap, int cost) {
        G[from].push_back(edge(to, cap, cost, G[to].size()));
        G[to].push_back(edge(from, 0, -cost, G[from].size() - 1));
    }
    int Min_cost_max_flow(int s, int t, int f, int& flow) {
        int res = 0; fill(H, H + 1 + V, 0);
        while (f) {
            priority_queue <pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>> > q;
            fill(dis, dis + 1 + V, inf);
            dis[s] = 0; q.push(pair<int, int>(0, s));
            while (!q.empty()) {
                pair<int, int> now = q.top(); q.pop();
                int v = now.second;
                if (dis[v] < now.first)continue;
                for (int i = 0; i < G[v].size(); ++i) {
                    edge& e = G[v][i];
                    if (e.cap > 0 && dis[e.to] > dis[v] + e.cost + H[v] - H[e.to]) {
                        dis[e.to] = dis[v] + e.cost + H[v] - H[e.to];
                        PreV[e.to] = v;
                        PreE[e.to] = i;
                        q.push(pair<int, int>(dis[e.to], e.to));
                    }
                }
            }
            if (dis[t] == inf)break;
            for (int i = 0; i <= V; ++i)H[i] += dis[i];
            int d = f;
            for (int v = t; v != s; v = PreV[v])d = min(d, G[PreV[v]][PreE[v]].cap);
            f -= d; flow += d; res += d*H[t];
            for (int v = t; v != s; v = PreV[v]) {
                edge& e = G[PreV[v]][PreE[v]];
                e.cap -= d;
                G[v][e.rev].cap += d;
            }
        }
        return res;
    }
    int a[maxn];
    int main()
    {
        int t;
        scanf("%d",&t);
        while(t--)
        {
            int n,k;
            scanf("%d%d",&n,&k);
            for(register int i=1;i<=n;++i) scanf("%d",&a[i]);
            int ss=0,s=1,t=2*n+2,tt=2*n+3;
            init(tt+1);
            AddEdge(ss,s,k,0);
            AddEdge(t,tt,k,0);
            for(register int i=1;i<=n;++i)
            {
                AddEdge(s,i+1,1,0);
                AddEdge(i+1+n,t,1,0);
                AddEdge(i+1,i+1+n,1,-a[i]);
                for(register int j=i+1;j<=n;++j)
                {
                    if(a[j]>=a[i])
                    {
                        AddEdge(1+i+n,1+j,1,0);
                    }
                }
            }
            int ans=0;
            printf("%d\n",-Min_cost_max_flow(ss,tt,inf,ans));
        }
        return 0;
    }
    

