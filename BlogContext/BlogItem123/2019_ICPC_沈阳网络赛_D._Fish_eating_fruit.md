## 2019_ICPC_沈阳网络赛_D._Fish_eating_fruit

## D. Fish eating fruit

哎 我刚写就想着tarjan 压点 废了 写不出  
看题解说了 并查集就开始蛋疼了 菜鸡看到压点就是tarjan 完全忘了并查集了orz

n个点，m条边，点分为两种，一种为空房间，一种为怪兽房间。进入空房间可以拿到一个糖果，重复进入不重复计数。进入怪兽房子不能出去，但有一次机会可以
**随机** 达到一个怪兽房间相邻的房间。询问最多可以找到多少糖果。

必须找最好的路的期望 找第一个过的怪物房 用它开始期望算就行 压点用并查集  
分3个情况

  1. 怪物到怪物
  2. 到1的联通块
  3. 到其他联通快

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 5;
    
    int n, m, k, cas;
    int head[maxn], deg[maxn], cnt;
    int to[maxn << 1], nxt[maxn << 1];
    int pre[maxn], siz[maxn];
    bool gw[maxn], gw1[maxn], vis[maxn];
    
    void ade(int a, int b) {
    	to[ ++ cnt ] = b, deg[a] ++;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    int finds(int x) {
    	return x == pre[x] ? x : pre[x] = finds(pre[x]);
    }
    
    void init(int n) {
    	memset(vis, 0, (n + 5) * sizeof(bool));
    	memset(deg, 0, (n + 5) * sizeof(int));
    	memset(head, 0, (n + 5) * sizeof(int));
    	memset(gw, 0, (n + 5) * sizeof(bool));
    	memset(gw1, 0, (n + 5) * sizeof(bool));
    	cnt = 0;
    	for(int i = 1; i <= n; i ++) pre[i] = i, siz[i] = 1;
    } 
    
    void dfs(int u) {
    	if(gw[u]) {
    		gw1[u] = 1;
    		return ;
    	}
    	if(vis[u]) return ;
    	vis[u] = 1;
    	for(int i = head[u]; i; i = nxt[i]) {
    		if(vis[to[i]]) continue;
    		dfs(to[i]);
    	}
    }
    
    int main() {
    	scanf("%d", &cas);
    	for(int tt = 1; tt <= cas; tt ++) {
    		scanf("%d %d %d", &n, &m, &k);
    		init(n);
    		for(int i = 1, a, b; i <= m; i ++) {
    			scanf("%d %d", &a, &b);
    			ade(a, b), ade(b, a);
    		}
    		for(int i = 1, a; i <= k; i ++) {
    			scanf("%d", &a);
    			gw[a] = 1;
    		} 
    		for(int u = 1; u <= n; u ++) {
    			if(gw[u]) continue;
    			for(int i = head[u]; i; i = nxt[i]) {
    				int v = to[i];
    				if(gw[v]) continue;
    				int fa = finds(u);
    				int fb = finds(v);
    				if(fa != fb) pre[fb] = fa, siz[fa] += siz[fb];
    			}
    		}
    		dfs(1);
    		double ans = 1.0 * siz[finds(1)];
    		for(int u = 1; u <= n; u ++) {
    			if(gw1[u]) {
    			double ma = 0;
                for(int i = head[u]; i; i = nxt[i]) {
                    if(gw[to[i]]) ma += siz[finds(1)];
                    else if(finds(to[i]) != finds(1))
    					ma += siz[finds(to[i])] + siz[finds(1)];
                    else ma += siz[finds(1)];
                }
                ans=max(ans, ma / deg[u]);
    			}
    		}
    		printf("%.6f\n", ans);
    	}
    	return 0;
    }
    

