## Codeforces_Round_#589_(Div._2)_D._Complete_Tripartite_(构造_思维)

## D. Complete Tripartite

<https://codeforces.com/contest/1228/problem/D>  
思维题 给了个图 让他成为所谓的3分图

我们假定从1(a点)开始是第一个集合 与1第一个连边的点b是第二个集合  
如何找刚好和a，b 连的是c点 而且是必然不同点集合之间的点是两两相连的

我们以这3个 作为集合开头 遍历图 标记123就好

不够这样标记完 其实还不是答案  
因为我们没有在这个基础上保证 同点集合之间的点是两两相连的  
另外我们还要确保相连2个点要不同 之前跑出的点 仅仅只是在满足连的 其他2点 确定的 只是可能 我们最后要自己确定

    
    
    #include <bits/stdc++.h>
    //#define int long long
    using namespace std;
    const int INF = 0x3f3f3f3f;
    const int maxn = 2e5 + 5;
    
    int n, m;
    vector<int> G[maxn];
    int col[maxn];
    int cnt[5];
    
    void ade(int a, int b) {
        G[a].push_back(b), G[b].push_back(a);
    }
    
    signed main() {
        cin >> n >> m;
        for(int i = 1, a, b; i <= m; i++) {
            cin >> a >> b;
            ade(a, b);
        }
        int x = 1;
        col[x] = 1;
        if(G[x].size() == 0) {
            cout << -1 << endl;
            return 0;
        }
        int y = G[x][0];
        col[y] = 2;
        int z = 0;
        for(int i = 2; i <= n; i ++) {
            if(i == y) continue;
            int f1 = 0, f2 = 0;
            for(int j = 0; j < G[i].size(); j ++) {
                int v = G[i][j];
                if(v == x) f1 = 1;
                if(v == y) f2 = 1;
            }
            if(f1 && f2) {
                z = i;
                col[i] = 3;
                break;
            }
        }
        if(!z) {
            cout << -1 << endl;
            return 0;
        }
        for(int i = 2; i <= n; i ++) {
            if(i == y || i == z) continue;
            int f1 = 0, f2 = 0, f3 = 0;
            for(int j = 0; j < G[i].size(); j ++) {
                int v = G[i][j];
                if(v == x) f1 = 1;
                if(v == y) f2 = 1;
                if(v == z) f3 = 1;
            }
            if(f1 + f2 + f3 != 2) {
                cout << -1 << endl;
                return 0;
            }
            if(!f1) col[i] = 1;
            if(!f2) col[i] = 2;
            if(!f3) col[i] = 3;
        }
    
        for(int i = 1; i <= n; i ++) cnt[col[i]] ++;
        if(cnt[1]*cnt[2] + cnt[2]*cnt[3] + cnt[1]*cnt[3] != m) {
            cout << -1 << endl;
            return 0;
        }
        for(int i = 1; i <= n; i++) {
            for(int j = 0; j < G[i].size(); j ++) {
                if(col[i] == col[G[i][j]]) {
                    cout << -1 << endl;
                    return 0;
                }
            }
        }
        for(int i = 1; i <= n; i ++) {
            cout << col[i] << " ";
        }cout << endl;
        return 0;
    }
    

