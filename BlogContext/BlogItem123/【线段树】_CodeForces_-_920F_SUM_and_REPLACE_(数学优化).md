## 【线段树】_CodeForces_-_920F_SUM_and_REPLACE_(数学优化)

## [线段树] CodeForces - 920F SUM and REPLACE (数学优化)

<https://vjudge.net/problem/1349242/origin>  
题意：给出一个数组，有两个操作，一个操作把区间所有数都变成其因子个数，另一个操作询问区间和。

一个树的约束个数 最多 2∗sqrt(n)2*sqrt(n)2∗sqrt(n) 我每次 变成它的约束个数 最多也就30次到1了 到2 以下 就不可能再来

所以 标记区间最大值 x&lt;=2x &lt;= 2x<=2 不更新完事

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e6 + 6;
    int f[maxn];
    
    int n, m;
    ll tree[maxn << 1];
    int mas[maxn << 1];
    
    void init() {
    	for(int i = 1; i < maxn; i ++)
    		for(int j = 1; j <= maxn/i; j ++)
    			f[i * j] ++;
    }
    
    inline push_up(int rt) {
    	mas[rt] = max(mas[rt << 1], mas[rt << 1 | 1]);
    	tree[rt] = tree[rt << 1] + tree[rt << 1 | 1];
    }
    
    void build(int l, int r, int rt) {
    	if(l == r) {
    		scanf("%lld", &tree[rt]);
    		mas[rt] = tree[rt];
    		return ;
    	}
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1), build(mid + 1, r, rt << 1 | 1);
    	push_up(rt);
    }
    
    void updata(int L, int R, int l, int r, int rt) {
    	if(L <= l && r <= R) if(mas[rt] <= 2) return ;
    	if(l == r) {
    		tree[rt] = f[tree[rt]];
    		mas[rt] = tree[rt];
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, R, l, mid, rt << 1);
    	if(R > mid) updata(L, R, mid + 1, r, rt << 1 | 1);
    	push_up(rt);
    } 
    
    ll query(int L, int R, int l, int r, int rt) {
    	if(L <= l && r <= R) return tree[rt];
    	int mid = l + r >> 1;
    	ll res = 0;
    	if(L <= mid) res += query(L, R, l, mid, rt << 1);
    	if(R > mid) res += query(L, R, mid + 1, r, rt << 1 | 1);
    	return res;
    }
    
    signed main() {
    	init();
    	scanf("%d %d", &n, &m);
    	build(1, n ,1);
    	int op, l, r;
    	while(m--) {
    		scanf("%d %d %d", &op, &l, &r);
    		if(op == 1) {
    			updata(l, r, 1, n , 1);
    		} else {
    			printf("%lld\n", query(l, r, 1, n ,1));
    		}
    	}
    	return 0;
    }
    

