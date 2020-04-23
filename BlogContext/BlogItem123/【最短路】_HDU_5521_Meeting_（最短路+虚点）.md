## 【最短路】_HDU_5521_Meeting_（最短路+虚点）

题目大意：有N个点，给定M个集合，集合Si里面的点两两之间的距离都为Ti，集合里面的所有点数之和<=1e6。两个人分别从1和n出发，要求相遇的最短距离，并输出相遇的点（可能多个）。  
解题思路：首先无疑是最短路，然后因为同一个点可能属于两个或多个集合，故需要虚电。除了n个点外，每一个集合建一个新的点与集合中的点相连，集合中的点要到集合中的另一个点要先经过新建的点，所以走的路变成了2倍，分别从1和n各走一遍最短路，最后答案除以2即可。

又见最短路 + 虚电 <https://blog.csdn.net/qq_40831340/article/details/99672543>

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e6 + 10;
    const int INF = 0x3f3f3f3f;
    typedef pair<int, int> P;
    int cas, n, m;
    
    int head[maxn], cnt;
    int to[maxn << 1], nxt[maxn << 1], val[maxn << 1];
    
    void ade(int a, int b, int c) {
    	to[++ cnt] = b, val[cnt] = c;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    bool vis[maxn];
    int dis[maxn], d[maxn];
    void dj(int s) {
    	priority_queue<P> que;
    	que.push(P(0, s));
    	memset(dis, 0x3f, sizeof dis);
    	memset(vis, 0, sizeof vis);
    	dis[s] = 0;
    	while(!que.empty()) {
    		P p = que.top();
    		que.pop();
    		int u = p.second;
    		if(vis[u]) continue;
    		vis[u] = 1;
    		for(int i = head[u]; i; i = nxt[i]) {
    			int v = to[i];
    			if(dis[v] > dis[u] + val[i]) {
    				dis[v] = dis[u] + val[i];
    				que.push(P(-dis[v], v));
    			}
    		}
    	}
    }
    vector<int> ans;
    int main() {
    	scanf("%d", &cas);
    	for(int tt = 1; tt <= cas; tt ++) {
    		scanf("%d %d", &n, &m);
    		memset(head, 0, sizeof head);
    		cnt = 0;
    		for(int i = 1, u, val, t; i <= m; i ++) {
    			scanf("%d %d", &val, &t);
    			for(int j = 1; j <= t; j ++) {
    				scanf("%d", &u);
    				ade(u, n + i, val), ade(n + i, u, 0);
    			}
    		}
    		printf("Case #%d: ", tt);
    		dj(1);
    		if(dis[n] == INF) puts("Evil John");
    		else {
    			for(int i = 1; i <= n; i ++) d[i] = dis[i];
    			dj(n);
    			ans.clear();
    			int tmp = INF;
    			for(int i = 1; i <= n; i ++) {
    				int mint = max(d[i], dis[i]);
    				if(mint < tmp) {
    					tmp = mint;
    					ans.clear();
    					ans.push_back(i);
    				} else if(mint == tmp) ans.push_back(i);
    			}
    			printf("%d\n", tmp);
    			for(int i = 0; i < ans.size(); i ++) {
    				if(i != 0) printf(" ");
    				printf("%d",ans[i]);
    			}
    			puts("");
    		}
    	}
    	return 0;
    }
    

