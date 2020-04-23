## 线段树进阶总结一_DFS序_欧拉序(括号序)

## DFS序

前置的几道题

线段树DFS序 1 单点更新 区间查询  
https://blog.csdn.net/qq_40831340/article/details/84501232  
线段树DFS序 2 区间子树更新 单点查  
https://blog.csdn.net/qq_40831340/article/details/84501478

以上都是关于统计 跟改子树的

## 欧拉序

这里我们可以处理出 关于树上路径的  
虽然我觉得 都是DFS搞这数组也没有啥大区别

##### 树上操作

https://www.luogu.org/problem/P3178  
其实我代码没有过样例 能过题的原因 是我读错题了 样例是父亲指向儿子 可是数据全是 儿子指向父亲 23333  
我可能是出题人以外知道这个事情的人了  
这次我们要处理 树上路径问题 要是像之前2篇博客内样 肯定不行 我们选择要剪掉 out 的才好  
不能再把他们压在一个结构体了 当然 我之前那样写 也是没有想过会有这样的问题

    
    
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;
    const int maxn = 2e5 + 10;
    
    int n, m, val[maxn];
    int head[maxn], cnt;
    int to[maxn], nxt[maxn];
    
    void ade(int a, int b) {
    	to[++ cnt] = b;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    int seq[maxn << 1], inout[maxn << 1];
    int ind[maxn << 1], outs[maxn << 1], tot;
    
    int ss;
    void dfs(int u) {
    	seq[++ tot] = u, inout[tot] = 1, ind[u] = tot;
    	for(int i = head[u]; i; i = nxt[i]) 
    		dfs(to[i]),ss ++;
    	seq[++ tot] = u, inout[tot] = -1, outs[u] = tot;
    }
    
    struct node {
    	int sum, lazy, flag;
    } tree[maxn << 3];
    
    void push_up(int rt) {
    	tree[rt].sum = tree[rt << 1].sum + tree[rt << 1 | 1].sum;
    	tree[rt].flag = tree[rt << 1].flag + tree[rt << 1 | 1].flag;
    }
    
    void push_down(int rt) {
    	tree[rt << 1].sum += tree[rt << 1].flag * tree[rt].lazy;
    	tree[rt << 1].lazy += tree[rt].lazy;
    	tree[rt << 1 | 1].sum += tree[rt << 1 | 1].flag * tree[rt].lazy;
    	tree[rt << 1 | 1].lazy += tree[rt].lazy;
    	tree[rt].lazy = 0;
    }
    
    void build(int l, int r, int rt) {
    	if(l == r) {
    		tree[rt].flag = inout[l];
    		tree[rt].sum = inout[l] * val[seq[l]];
    		return ;
    	}
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1);
    	build(mid + 1, r, rt << 1 | 1);
    	push_up(rt);
    }
    
    void updata(int L, int R, int l, int r, int rt, int val) {
    	if(L <= l && R >= r) {
    		tree[rt].sum += tree[rt].flag * val;
    		tree[rt].lazy += val;
    		return ;
    	}
    	if(tree[rt].lazy) push_down(rt);
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, R, l, mid, rt << 1, val);
    	if(R > mid) updata(L, R, mid + 1, r, rt << 1 | 1, val);
    	push_up(rt);
    }
    
    int query(int L, int R, int l, int r, int rt) {
    	if(L <= l && R >= r) {
    		return tree[rt].sum;
    	}
    	if(tree[rt].lazy) push_down(rt);
    	int mid = r + l >> 1;
    	int res = 0;
    	if(L <= mid) res += query(L, R, l, mid, rt << 1);
    	if(R > mid) res += query(L, R, mid + 1, r, rt << 1 | 1);
    	return res;
    }
    
    signed main() {
    	scanf("%lld %lld", &n, &m);
    	for(int i = 1; i <= n; i ++ ) scanf("%lld", &val[i]);
    	for(int i = 1, a, b; i < n; i ++){
    			scanf("%lld %lld", &a, &b), ade(b, a);
    	}
    	
    	int N = n << 1;
    	dfs(1);
    	build(1, N, 1);
    	int op, x, a;
    
    	while(m --) {
    		scanf("%lld %lld", &op, &x);
    		if(op == 1) {
    			scanf("%lld", &a);
    			updata(ind[x], ind[x], 1, N, 1, a);
    			updata(outs[x], outs[x], 1, N, 1, a);
    		} else if(op == 2) {
    			scanf("%lld", &a);
    			updata(ind[x], outs[x], 1, N, 1, a);
    		} else {
    			printf("%lld\n", query(1, ind[x], 1, N, 1));
    		}
    	}
    	return 0;
    }
    

#### 树上路径 区间查 单点改

接下来就是我们 dfs建立括号序列 如果让路上某个点+v  
路径询问 我们增加了多少  
就可以考虑 根节点 到 u 根节点到 v 我们再减去 2倍根节点 到 lca 再加上 lca

    
    
    #include <bits/stdc++.h>
    //#define int long long
    using namespace std;
    const int maxn = 2e5 + 10;
    
    int n, m, val[maxn];
    int head[maxn], cnt;
    int to[maxn], nxt[maxn];
    
    void ade(int a, int b) {
    	to[++ cnt] = b;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    int seq[maxn << 1], inout[maxn << 1];
    int ind[maxn << 1], outs[maxn << 1], tot;
    
    void dfs(int u, int pre) {
    	seq[++ tot] = u, inout[tot] = 1, ind[u] = tot;
    	for(int i = head[u]; i; i = nxt[i])
    		if(pre != to[i]) dfs(to[i], u);
    	seq[++ tot] = u, inout[tot] = -1, outs[u] = tot;
    }
    
    struct node {
    	int sum, flag;
    } tree[maxn << 2];
    
    void push_up(int rt) {
    	tree[rt].sum = tree[rt << 1].sum + tree[rt << 1 | 1].sum;
    }
    
    void build(int l, int r, int rt) {
    	if(l == r) {
    		tree[rt].flag = inout[l];
    		tree[rt].sum = inout[l] * val[seq[l]];
    		return ;
    	}
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1);
    	build(mid + 1, r, rt << 1 | 1);
    	push_up(rt);
    }
    
    void updata(int L, int l, int r, int rt, int val) {
    	if(l == r) {
    		tree[rt].sum = tree[rt].flag * val;
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, l, mid, rt << 1, val);
    	else updata(L, mid + 1, r, rt << 1 | 1, val);
    	push_up(rt);
    }
    
    int query(int L, int R, int l, int r, int rt) {
    	if(L == 0) return 0;
    	if(L <= l && R >= r) {
    		return tree[rt].sum;
    	}
    	int mid = r + l >> 1;
    	int res = 0;
    	if(L <= mid) res += query(L, R, l, mid, rt << 1);
    	if(R > mid) res += query(L, R, mid + 1, r, rt << 1 | 1);
    	return res;
    }
    
    int fa[maxn][30], depth[maxn];
    int lg[maxn];
    
    void lca_dfs(int x,int pre) {
    	depth[x] = depth[pre] + 1;
    	fa[x][0] = pre;
    	for(int i = 1; (1 << i) <= depth[x]; i ++)
    		fa[x][i] = fa[fa[x][i - 1]][i - 1];
    	for(int i = head[x]; i; i = nxt[i])
    		if(to[i] != pre) {
    			lca_dfs(to[i], x);
    		}
    }
    
    int LCA(int x,int y) {
    	if(depth[x] < depth[y]) swap(x, y);
    	while(depth[x] > depth[y]) x = fa[x][lg[depth[x] - depth[y]] - 1];
    	if(x == y) return x;
    	for(int k = lg[depth[x]] - 1; k >= 0; k --) {
    		if(fa[x][k] != fa[y][k]) {
    			x = fa[x][k], y = fa[y][k];
    		}
    	}
    	return fa[x][0];
    }
    
    signed main() {
    	freopen("in.in", "r", stdin);
    	freopen("a.out","w+",stdout);
    	for(int i = 1; i < maxn; i ++) lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
    	scanf("%d %d", &n, &m);
    	for(int i = 1; i <= n; i ++ ) scanf("%d", &val[i]);
    	for(int i = 1, a, b; i < n; i ++)
    		scanf("%d %d", &a, &b), ade(a, b), ade(b, a);
    
    	int N = n << 1;
    	dfs(1, 0);
    	build(1, N, 1);
    	lca_dfs(1, 0);
    	fa[1][0] = 0;
    	int op , u, v;
    	while(m --) {
    		scanf("%d %d %d", &op, &u, &v);
    		if(op == 1) {
    			updata(ind[u], 1, N, 1, v);
    			updata(outs[u], 1, N, 1, v);
    		} else {
    			int lca = LCA(u, v);
    			printf("%d\n", \
    			       query(1, ind[u], 1, N, 1) + \
    			       query(1, ind[v], 1, N, 1) - \
    			       2 * query(1, ind[lca], 1, N, 1) + \
    			       query(ind[lca], ind[lca], 1, N, 1) \
    			      );
    		}
    	}
    	return 0;
    }
    

~~区间改 就特别需要debug能力拉 orz 之后补上 还是不会orz~~

