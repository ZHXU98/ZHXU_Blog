## 【线段树】_Codeforces_482B_Interesting_Array_(思维小优化)

## Codeforces 482B Interesting Array(线段树)

题目大意：给定一个长度为N的数组，现在有M个限制，每个限制有l，r，q，表示从a[l]~a[r]取且后的数一定为q，问是否有满足的数列。

考虑维护 30颗线段树 每个代表这位二进制 0 1  
区间修改 区间查 这线段树代表的二进制位 覆盖区间 与 q 这位二进制 是一样的  
如果q 是 0 但是 这树区间二进制 区间全是1 cout <<“NO”<< endl;  
然后 我们光荣的T了

在仔细想想 一个树上之维护 一位 有点少 毕竟 二进制每一位也没有啥关系  
一个树就完成了 毕竟int 存30位二进制没有啥压力  
我们进行的区间覆盖，按位或运算，可以直接把三十颗线段树合到一棵，支持区间或修改，每次区间或上val[i];  
最后 在& 回去 暴力对所有条件进行判断 不行就NO  
这题就完成了

    
    
    #include <bits/stdc++.h>
    using namespace std;
     
    const int maxn = 1e5 + 6;
     
    int tree[maxn << 2], lazy[maxn << 2];
     
    inline void push_up(int rt) {
    	tree[rt] = tree[rt << 1] & tree[rt << 1 | 1];
    }
    
    inline void push_down(int rt) {
    	lazy[rt << 1] |= lazy[rt];
    	lazy[rt << 1 | 1] |= lazy[rt];
    	tree[rt << 1] |= lazy[rt];
    	tree[rt << 1 | 1] |= lazy[rt];
    	lazy[rt] = 0;
    }
     
    void updata(int L, int R, int l, int r, int rt, int val) {
    	if(L <= l && R >= r) {
    		tree[rt] |= val;
    		lazy[rt] |= val;
    		return ;
    	}
    	if(lazy[rt]) push_down(rt);
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, R, l, mid, rt << 1, val);
    	if(R > mid) updata(L, R, mid + 1, r, rt << 1 | 1, val);
    	push_up(rt);
    }
     
    int query(int L, int R, int l, int r, int rt) {
    	if(L <= l && R >= r) {
    		return tree[rt];
    	}
    	if(lazy[rt]) push_down(rt);
    	int mid = l + r >> 1;
    	int res = (1 << 31) - 1;
    	if(L <= mid) res &= query(L, R, l, mid, rt << 1);
    	if(R > mid) res &= query(L, R, mid + 1, r, rt << 1 | 1);
    	return res;
    }
     
    int l[maxn], r[maxn], val[maxn];
    signed main() {
    	int n, m, flag = 0;
    	scanf("%d %d", &n, &m);
    	for(int i = 1; i <= m; i ++) {
    		scanf("%d %d %d", &l[i], &r[i], &val[i]);
    		updata(l[i], r[i], 1, n, 1, val[i]);
    	}
     
    	for(int i = 1; i <= m; i ++) {
    		if(query(l[i], r[i], 1, n, 1) != val[i]) {
    			flag = 1;
    			break;
    		}
    	}
    	if(flag) puts("NO");
    	else {
    		puts("YES");
    		for(int i = 1; i <= n; i ++) {
    			if(i != 1) printf(" ");
    			printf("%d", query(i, i, 1, n, 1));
    		}
    		puts("");
    	}
     
    	return 0;
    }
    

