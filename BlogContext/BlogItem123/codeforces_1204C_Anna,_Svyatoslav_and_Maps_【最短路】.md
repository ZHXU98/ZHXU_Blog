## codeforces_1204C_Anna,_Svyatoslav_and_Maps_【最短路】

[codeforces 1204C Anna, Svyatoslav and Maps
[最短路]](http://codeforces.com/problemset/problem/1204/C)  
删掉一些点 但是 这些点是到下一个点(最短路方式)必须过的 输出最短序列  
那莫 显然出现一个点 从上一个位置出发 能从最短路跳过去 （最短路 < dis总路程）  
我们就必须让他在序列里面出现了 防止不走这个点

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(0);cin.tie(0);
    using namespace std;
    typedef long long ll;
    const int maxn = 1005;
    const int INF = 0x3f3f3f3f;
    
    int n, m;
    int G[maxn][maxn], a[int(2e6 + 5)], ans[int(2e6 + 5)];
    
    void floyd() {
        for(int k = 1; k <= n; k ++) 
            for(int i = 1; i <= n; i ++) 
                for(int j = 1; j <= n; j ++) 
                    if(G[i][j] > G[i][k] + G[k][j]) 
                        G[i][j] = G[i][k] + G[k][j];
    }
    
    signed main() {
        scanf("%d", &n);
        for(int i = 1; i <= n; i ++) {
            for(int j = 1; j <= n; j ++) {
                scanf("%1d", &m);
                if(m == 1) G[i][j] = 1;
                else G[i][j] = INF;
                if(i == j) G[i][j] = 0;
            }
        }
        floyd();
        scanf("%d", &m);
        for(int i = 1; i <= m; i ++) scanf("%d", a + i);
        int cnt = 0;
        ans[ ++ cnt] = a[1];
        int dis = 0;
        for(int i = 2; i <= m; i ++) {
            dis += G[a[i - 1]][a[i]];
            if(dis > G[ans[cnt]][a[i]]) {
                ans[++cnt] = a[i - 1];
                dis = G[ans[cnt]][a[i]];
            }
        }
        ans[++cnt] = a[m];
        cout << cnt << endl;
        for(int i = 1; i <= cnt; i ++) cout << ans[i] << " ";
        return 0;
    }
    

