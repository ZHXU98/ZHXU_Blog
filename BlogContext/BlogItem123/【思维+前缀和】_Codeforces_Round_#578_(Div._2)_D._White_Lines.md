## 【思维+前缀和】_Codeforces_Round_#578_(Div._2)_D._White_Lines

<https://codeforces.com/contest/1200/problem/D>

## D. White Lines

涂一个 m * m 的格子变白 问你最后又多少白线 只统计 横竖到头的线

唉 n∗n∗kn*n*kn∗n∗k 假算法T 12 // ORZ

对于这道题 我们考虑 白色 和 黑色 行列分别处理  
这样就稳定的n^2了  
首先 只对黑色进行 前缀和 (行列都处理一次)  
再次 处理一次前缀和 我们统计又多少区间被m的白线覆盖 可以使得这一行or列 变白线

统计的时候 这一区块可以带来的白线数量 就是 ls 左右间的差 hs 上下行区间的差 和 + 原来的

    
    
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;
    const int maxn = 2e3 + 5;
     
    int n, m, k, sy;
    int cas;
     
    char mp[maxn][maxn];
    int ls[maxn][maxn], hs[maxn][maxn];
    int l[maxn][maxn], h[maxn][maxn];
     
    signed main() {
    	scanf("%d %d", &n, &m);
    	k = m;
    	for(int i = 1; i <= n; i ++)
    		for(int j = 1; j <= n; j ++)
    			scanf(" %c", &mp[i][j]);
     
    	for(int i = 1; i <= n; i ++) {
    		for(int j = 1; j <= n; j ++) {
    			h[i][j] = h[i][j - 1] + (mp[i][j] == 'B');
    			l[i][j] = l[i - 1][j] + (mp[i][j] == 'B');
    		}
    	}
     
    	for(int i = 1; i <= n - m + 1; i ++)
    		for(int j = 1 ; j <= n ; j ++)
    			ls[i][j] = ls[i][j - 1] + (l[n][j] && l[i + m - 1][j] - l[i - 1][j] == l[n][j]);
    	for(int i = 1; i <= n; i ++)
    		for(int j = 1 ; j <= n - m + 1; j ++)
    			hs[i][j] = hs[i - 1][j] + (h[i][n] && h[i][j + m - 1] - h[i][j - 1] == h[i][n]);
     
    	int ans = 0, tot = 0;
    	for(int i = 1; i <= n; i ++)
    		tot += (h[i][n] == 0) + (l[n][i] == 0);
    	for(int i = 1; i <= n - m + 1; i ++)
    		for(int j = 1; j <= n - m + 1; j ++)
    			ans = max(ans, ls[i][j + m - 1] - ls[i][j - 1] + \
    			          hs[i + m - 1][j] - hs[i - 1][j] + tot);
    	printf("%d\n", ans);
    	return 0;
    }
    

