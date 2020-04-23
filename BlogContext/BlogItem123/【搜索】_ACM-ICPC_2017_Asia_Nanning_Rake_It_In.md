## 【搜索】_ACM-ICPC_2017_Asia_Nanning_Rake_It_In

## Rake It In

<https://nanti.jisuanke.com/t/A1538>  
一个另类的搜索吧  
A要最大的 B要最小的 如此对抗的进行  
直接搜很容易 但是什么状态是我们需要的才是关键  
A是奇数轮 B是偶数轮 所以在奇数轮 A选择之后所有状态最大的  
B偶数轮 选择之后状态最小的 这样我们把所有状态都枚举了 也得到了 他们博弈选择的最优解

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e3 + 5;
    const int INF = 0x3f3f3f3f;
    int n, m, k;
    int cas;
    
    int mp[maxn][maxn];
    
    void cg1(int x, int y) {
        int t = mp[x][y];
        mp[x][y] = mp[x][y + 1];
        mp[x][y + 1] = mp[x + 1][y + 1];
        mp[x + 1][y + 1] = mp[x + 1][y];
        mp[x + 1][y] = t;
    }
    
    void cg2(int x, int y) {
        int t = mp[x + 1][y];
        mp[x + 1][y] = mp[x + 1][y + 1];
        mp[x + 1][y + 1] = mp[x][y + 1];
        mp[x][y + 1] = mp[x][y];
        mp[x][y] = t;
    }
    
    int resum(int x, int y) {
        return mp[x][y] + mp[x + 1][y] + mp[x][y + 1] + mp[x + 1][y + 1];
    }
    
    int dfs(int u) {
        if(u > 2 * k)
            return 0;
        int ans = 0;
        if(u & 1) {
            ans = 0;
            for(int i = 1; i <= 3; i ++) {
                for(int j = 1; j <= 3; j ++) {
                    cg1(i, j);
                    ans = max(ans, dfs(u + 1) + resum(i, j));
                    cg2(i, j);
                }
            }
        } else {
            ans = INF;
            for(int i = 1; i <= 3; i ++) {
                for(int j = 1; j <= 3; j ++) {
                    cg1(i, j);
                    ans = min(ans, dfs(u + 1) + resum(i, j));
                    cg2(i, j);
                }
            }
        }
        return ans;
    }
    
    signed main() {
        scanf("%d", &cas);
        while(cas --) {
            scanf("%d", &k);
            for(int i = 1; i <= 4; i ++)
                for(int j = 1; j <= 4; j ++)
                    scanf("%d", &mp[i][j]);
    
            cout << dfs(1) << endl;
        }
        return 0;
    }
    
    

