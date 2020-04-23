## Codeforces_Round_#597_(Div._2)_D._Shichikuji_and_Power_Grid_【图论_虚点】

## Shichikuji and Power Grid

<https://codeforces.com/contest/1245/problem/D>  
题意 ：  
给了你一堆城市 让你给他们建发电站 or 把他们连到有点的城市 我们道路的花费会是 题目中给的(k[v]+k[u])∗((u,v)曼哈顿距离)(k[v] +
k[u]) * ((u, v)曼哈顿距离)(k[v]+k[u])∗((u,v)曼哈顿距离)  
直接建立发电站花费是 c[i]  
现在 让你求 全部城市有电的最小花费(默认初始都没有电)

思路：  
我们考虑建立一个虚电与其他所有的城市相连 他们的边权 就是这个城市建立发电站的花费 其他的 城市我们正常 n2n^2n2 边权就是道路花费 暴力建图  
这 既能保证图联通供电 也 简化的模型 仅仅只跑一次 kruskal

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e6 + 2005;
    const int mod = 1e9 + 7;
     
    int n;
    pair<int, int> p[2005];
    int k[2005], c[2005];
    struct Edge {
        int u, v;
        long long dis;
    } e[maxn];
    int cnt;
     
    int pre[2005];
    void init() {for(int i = 0; i <= n; i ++) pre[i] = i;}
    int finds(int x) {return x == pre[x] ? x : pre[x] = finds(pre[x]);}
     
    long long recost(int u, int v) {
        return (1ll * k[u] + k[v]) * (1ll * abs(1ll * p[u].first - p[v].first) + abs(p[u].second - p[v].second));
    }
     
    void ade(int u, int v) {
        e[++ cnt] = Edge{u, v, recost(u, v)};
    }
     
    vector<pair<int, int> > path;
    vector<int> powers;
     
    long long kruskal() {
        long long ans = 0;
        init();
        sort(e + 1, e + 1 + cnt, [](const Edge & a, const Edge & b) {
            return a.dis < b.dis;
        });
        for(int i = 1; i <= cnt; i ++) {
            int fu = finds(e[i].u), fv = finds(e[i].v);
            if(fu != fv) {
                pre[fu] = fv;
                ans += e[i].dis;
                if(e[i].v == n) powers.push_back(e[i].u);
                else path.push_back(make_pair(e[i].u, e[i].v));
            }
        }
        return ans;
    }
     
    signed main() {
        cin >> n;
        for(int i = 1; i <= n; i ++)
            cin >> p[i].first >> p[i].second;
        for(int i = 1; i <= n; i ++)
            cin >> c[i];
        for(int i = 1; i <= n; i ++)
            cin >> k[i];
     
        for(int i = 1; i <= n; i ++)
            for(int j = i + 1; j <= n; j ++)
                ade(i, j);
        n ++;
        for(int i = 1; i < n; i ++) e[++ cnt] = Edge{i, n, c[i]};
        cout << kruskal() << endl;
        cout << powers.size() << endl;
        for(int i = 0; i < powers.size(); i ++) {
            if(i != 0) cout << " ";
            cout << powers[i] ;
            if(i == powers.size() - 1) cout << endl;
        }
        cout << path.size() << endl;
        for(int i = 0; i < path.size(); i ++)
            cout << path[i].first << " " << path[i].second << endl;
     
        return 0;
    }
    

