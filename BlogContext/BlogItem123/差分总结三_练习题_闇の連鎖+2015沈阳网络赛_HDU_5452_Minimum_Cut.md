## 差分总结三_练习题_闇の連鎖+2015沈阳网络赛_HDU_5452_Minimum_Cut

下面2题 差不多 都是边差分

## 闇の連鎖

<https://www.acwing.com/problem/content/354/>  
这个题 删一个树边 和 一个非树边 让树不连通  
那样 删一个 经过边是 0 的树边 和 删 一个 经过边是 1 的树边 才能成功  
前置 是 m0 * m 数量 后者 是唯一确定的

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 10;
    
    int n, m;
    int head[maxn], cnt;
    int nxt[maxn << 1], to[maxn << 1];
    
    void ade(int a, int b) {
    	to[++ cnt] = b;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    int depth[maxn], fa[maxn][25];
    void dfs_lca(int x, int pre) {
    	depth[x] = depth[pre] + 1;
    	fa[x][0] = pre;
    	for(int i = 1; (1 << i) <= depth[x]; i ++)
    		fa[x][i] = fa[fa[x][i - 1]][i - 1];
    	for(int i = head[x]; i; i = nxt[i])
    		if(to[i] != pre) {
    			dfs_lca(to[i], x);
    		}
    }
    
    int LCA(int x, int y) {
    	if(depth[x] > depth[y]) swap(x, y);
    	for(int i = 24; ~i; -- i)
    		if(depth[x] <= depth[y] - (1 << i))
    			y = fa[y][i];
    	if(x == y) return x;
    	for(int i = 24; ~i; -- i)
    		if(fa[x][i] != fa[y][i])
    			x = fa[x][i], y =fa[y][i];
    	return fa[x][0];
    }
    
    int coun[maxn];
    void dfs(int x, int pre) {
    	for(int i = head[x]; i; i = nxt[i]) {
    		int y = to[i];
    		if(y == pre) continue;
    		dfs(y, x);
    		coun[x] += coun[y];
    	}
    }
    
    signed main() {
    	scanf("%d %d", &n, &m);
    	for(int i = 1, a, b; i < n; i ++) {
    		scanf("%d %d", &a, &b);
    		ade(a, b), ade(b, a);
    	}
    	dfs_lca(1, 0);
    	for(int i = 1, a, b; i <= m; i ++) {
    		scanf("%d %d", &a, &b);
    		int lca = LCA(a, b);
    		++ coun[a], ++ coun[b], coun[lca] -= 2;
    	} 
    	dfs(1, 0);
    	int m1 = 0, m2 = 0;
    	for(int i = 2; i <= n; i ++) {
    		if(coun[i] == 0) m1 ++;
    		else if(coun[i] == 1) m2 ++;
    	}
    	printf("%d\n", m1 * m + m2);
    	return 0;
    }
    

## 2015沈阳网络赛 HDU 5452 Minimum Cut

必须删一个树边 和 删其他 非树边 最少 删几个 不连通  
只要有一个 树边 没有被覆盖过 删他就完成了 次数是1  
如果没有 找 最小被覆盖的 + 1 所有路过这树边的边 + 这个树边

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 4e5 + 10;
    
    int n, m, cas;
    int head[maxn], cnt;
    int nxt[maxn << 1], to[maxn << 1];
    
    void ade(int a, int b) {
    	to[++ cnt] = b;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    int depth[maxn], fa[maxn][25];
    void dfs_lca(int x, int pre) {
    	depth[x] = depth[pre] + 1;
    	fa[x][0] = pre;
    	for(int i = 1; (1 << i) <= depth[x]; i ++)
    		fa[x][i] = fa[fa[x][i - 1]][i - 1];
    	for(int i = head[x]; i; i = nxt[i])
    		if(to[i] != pre) {
    			dfs_lca(to[i], x);
    		}
    }
    
    int LCA(int x, int y) {
    	if(depth[x] > depth[y]) swap(x, y);
    	for(int i = 24; ~i; -- i)
    		if(depth[x] <= depth[y] - (1 << i))
    			y = fa[y][i];
    	if(x == y) return x;
    	for(int i = 24; ~i; -- i)
    		if(fa[x][i] != fa[y][i])
    			x = fa[x][i], y =fa[y][i];
    	return fa[x][0];
    }
    
    int coun[maxn];
    void dfs(int x, int pre) {
    	for(int i = head[x]; i; i = nxt[i]) {
    		int y = to[i];
    		if(y == pre) continue;
    		dfs(y, x);
    		coun[x] += coun[y];
    	}
    }
    
    signed main() {
    	scanf("%d", &cas);
    	for(int CAS = 1; CAS <= cas; CAS ++) {
    		scanf("%d %d", &n, &m);
            memset(head, 0, (n + 5) * sizeof (int));
            cnt = 0;
    		for(int i = 1, a, b; i < n; i ++) {
    			scanf("%d %d", &a, &b);
    			ade(a, b), ade(b, a);
    		}
    		dfs_lca(1, 0);
            memset(coun, 0, (n + 5) * sizeof(int));
    		for(int i = 1, a, b; i <= m - n + 1; i ++) {
    			scanf("%d %d", &a, &b);
    			int lca = LCA(a, b);
    			++ coun[a], ++ coun[b], coun[lca] -= 2;
    		}
    		dfs(1, 0);
    		int ans = 0x3f3f3f3f;
    		for(int i = 2; i <= n; i ++) {
    			if(coun[i] == 0) ans = 1;
    			else if(coun[i]) ans = min(ans, coun[i] + 1);
    		}
    		printf("Case #%d: ", CAS);
    		printf("%d\n", ans);
    	}
    	return 0;
    }
    

