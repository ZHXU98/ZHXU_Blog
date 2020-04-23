## 【动态区间第K大】_树套数_P2617_Dynamic_Rankings

## P2617 Dynamic Rankings

<https://www.luogu.org/problem/P2617>  
理解用树状数组的方式 把主席树静态第k大前缀和变成了现在类似树状数组做差 找k大

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    inline int lowbits(int x) {
    	return x & -x;
    }
    
    int n, m;
    string op;
    int a[maxn], id[maxn << 1];
    struct Questions {
    	bool op;
    	int l, r, k;
    } q[maxn];
    
    int tot;
    struct SegTree {
    	int val;
    	int ls, rs;
    } t[maxn * 400];
    int root[maxn], tmp[2][30], num[2], cnt;
    
    void updata(int &p, int l, int r, int pos, int val) {
    	if(!p) p = ++ tot;
    	t[p].val += val;
    	if(l == r) return;
    	int mid = l + r >> 1;
    	if(pos <= mid) updata(t[p].ls, l, mid, pos, val);
    	else updata(t[p].rs, mid + 1, r, pos, val);
    }
    
    void updata(int x, int val) {
    	int k = lower_bound(id + 1, id + 1 + cnt, a[x]) - id;
    	for(int i = x; i <= n; i += lowbits(i))
    		updata(root[i], 1, cnt, k, val);
    }
    
    int query(int l, int r, int k) {
    	if(l == r) return l;
    	int mid = l + r >> 1, sum = 0;
    	for(int i = 1; i <= num[0]; i ++) sum += t[t[tmp[0][i]].ls].val;
    	for(int i = 1; i <= num[1]; i ++) sum -= t[t[tmp[1][i]].ls].val;
    	if(k <= sum) {
    		for(int i = 1; i <= num[0]; i ++) tmp[0][i] = t[tmp[0][i]].ls;
    		for(int i = 1; i <= num[1]; i ++) tmp[1][i] = t[tmp[1][i]].ls;
    		return query(l, mid, k);
    	} else {
    		for(int i = 1; i <= num[0]; i ++) tmp[0][i] = t[tmp[0][i]].rs;
    		for(int i = 1; i <= num[1]; i ++) tmp[1][i] = t[tmp[1][i]].rs;
    		return query(mid + 1, r, k - sum);
    	}
    }
    
    int ans_query(int l, int r, int k) {
    	num[0] = num[1] = 0;
    	for(int i = r; i; i -= lowbits(i)) tmp[0][++ num[0]] = root[i];
    	for(int i = l - 1; i; i -= lowbits(i)) tmp[1][++ num[1]] = root[i];
    	return query(1, cnt, k);
    }
    
    int main() {
    	ios::sync_with_stdio(false);
    	cin >> n >> m;
    	for(int i = 1; i <= n; i ++) cin >> a[i], id[++ cnt] = a[i];
    	for(int i = 1; i <= m; i ++) {
    		cin >> op;
    		q[i].op = (op[0] == 'Q');
    		if (q[i].op) cin >> q[i].l >> q[i].r >> q[i].k;
    		else cin >> q[i].l >> q[i].k, id[++ cnt] = q[i].k;
    	}
    
    	sort(id + 1, id + cnt + 1);
    	cnt = unique(id + 1, id + cnt + 1) - id - 1;
    	for(int i = 1; i <= n; i ++) updata(i, 1);
    
    	for (int i = 1; i <= m; i ++) {
    		if(q[i].op) printf("%d\n", id[ans_query(q[i].l, q[i].r, q[i].k)]);
    		else {
    			updata(q[i].l, -1);
    			a[q[i].l] = q[i].k;
    			updata(q[i].l, 1);
    		}
    	}
    	return 0;
    }
    
    

