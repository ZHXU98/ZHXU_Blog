## 【可持续化】_可持续化trie以及_主席树_BZOJ_3261_&_HDU_-_4417_Super_Mario

最大异或和  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190601202201749.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
加了 可持续化 找最大值类比那个 trie 找2个最大异或和 贪就好了  
https://blog.csdn.net/qq_40831340/article/details/90644908

    
    
    #include<iostream>
    #include<cstdio>
    #include<cstring>
    #include<algorithm>
    #include<cstdlib>
    using namespace std;
    const int N = 600010;
    int trie[N * 24][2], latest[N * 24];
    int s[N], root[N], n, m, tot;
    
    void insert(int i, int k, int p, int q) {
    	if (k < 0) {
    		latest[q] = i;
    		return;
    	}
    	int c = s[i] >> k & 1;
    	if (p) trie[q][c ^ 1] = trie[p][c ^ 1];
    	trie[q][c] = ++tot;
    	insert(i, k - 1, trie[p][c], trie[q][c]);
    	latest[q] = max(latest[trie[q][0]], latest[trie[q][1]]);
    }
    
    int ask(int now, int val, int k, int limit) {
    	if (k < 0) return s[latest[now]] ^ val;
    	int c = val >> k & 1;
    	if (latest[trie[now][c ^ 1]] >= limit)
    		return ask(trie[now][c ^ 1], val, k - 1, limit);
    	else
    		return ask(trie[now][c], val, k - 1, limit);
    }
    
    int main() {
    	cin >> n >> m;
    	latest[0] = -1;
    	root[0] = ++tot;
    	insert(0, 23, 0, root[0]);
    	for (int i = 1; i <= n; i++) {
    		int x; scanf("%d", &x);
    		s[i] = s[i - 1] ^ x;
    		root[i] = ++tot;
    		insert(i, 23, root[i - 1], root[i]);
    	}
    	for (int i = 1; i <= m; i++) {
    		char op[2]; scanf("%s", op);
    		if (op[0] == 'A') {
    			int x; scanf("%d", &x);
    			root[++n] = ++tot;
    			s[n] = s[n - 1] ^ x;
    			insert(n, 23, root[n - 1], root[n]);
    		}
    		else {
    			int l, r, x; scanf("%d%d%d", &l, &r, &x);
    			printf("%d\n", ask(root[r - 1], x ^ s[n], 23, l - 1));
    		}
    	}
    }
    

**BZOJ 3261**  
**静态区间K大值**  
复习orz  
外链 ： https://blog.csdn.net/qq_40831340/article/details/82729609

    
    
    #include <iostream>
    #include <cstdio>
    #include <algorithm>
    using namespace std;
    
    const int maxn = 100000 + 10;
    
    int m;
    
    struct node {
    	int lc,rc;
    	int val;
    } tree[maxn * 20];
    
    int tot,root[maxn * 20];
    int n, a[maxn];
    
    int build(int l, int r) {
    	int p = ++ tot;
    	if(l == r) {
    		tree[p].val = 0;
    		return p;
    	}
    	int mid = l + r >> 1;
    	tree[p].lc = build(l, mid);
    	tree[p].rc = build(mid + 1, r);
    	tree[p].val = tree[tree[p].lc].val + tree[tree[p].rc].val;
    	return p;
    }
    
    int insert(int now, int l, int r, int x, int val) {
    	int p = ++ tot;
    	tree[p] = tree[now];
    	if(l == r) {
    		tree[p].val += val ;
    		return p;
    	}
    	int mid = l + r >> 1;
    	if(x <= mid) 
    		tree[p].lc = insert(tree[now].lc, l, mid, x, val);
    	else 
    		tree[p].rc = insert(tree[now].rc, mid + 1, r, x, val);
    	tree[p].val = tree[tree[p].lc].val + tree[tree[p].rc].val;
    	return p;
    }
    
    int ask(int p, int q, int l, int r, int k) {
    	if(l == r) {
    		return l;
    	}
    	int mid = l + r >> 1;
    	int cnt = tree[tree[p].lc].val - tree[tree[q].lc].val ;
    	if(k <= cnt) 
    		return ask(tree[p].lc, tree[q].lc, l, mid, k);
    	else 
    		return ask(tree[p].rc, tree[q].rc ,mid + 1, r, k - cnt);
    }
    
    int b[maxn];
    
    int main() {
    	scanf("%d %d", &n, &m);
    	
    	for(int i = 1; i <= n ;i ++ ) {
    		scanf("%d", &a[i]);
    		b[i] = a[i];
    	}
    	
    	sort(b + 1, b + 1 + n);
    	int cnt = unique(b + 1, b + 1 + n) - b;
    	root[0] = build(1, cnt);
    	for(int i = 1; i <= n; i ++ ) {
    		int pos = lower_bound(b + 1, b + cnt, a[i]) - b;
    		root[i] = insert(root[i - 1], 1, cnt, pos, 1);
    	}
    	int l, r, k;
    	for(int i = 0; i < m; i ++ ) {
    		scanf("%d %d %d", &l, &r, &k);
    		printf("%d\n", b[ask(root[r], root[l - 1], 1, cnt, k)]);
    	}
    	return 0;
    }
    

**HDU - 4417 Super Mario**  
统计区间小于K的树个数

    
    
    #include <iostream>
    #include <cstdio>
    #include <algorithm>
    using namespace std;
    
    const int maxn = 100000 + 10;
    
    struct node {
    	int lc,rc;
    	int val;
    } tree[maxn * 20];
    
    int tot,root[maxn];
    int n, m, a[maxn], b[maxn];
    
    int build(int l, int r) {
    	int p = ++ tot;
    	if(l == r) {
    		tree[p].val = 0;
    		return p;
    	}
    	int mid = l + r >> 1;
    	tree[p].lc = build(l, mid);
    	tree[p].rc = build(mid + 1, r);
    	tree[p].val = tree[tree[p].lc].val + tree[tree[p].rc].val;
    	return p;
    }
    
    int insert(int now, int l, int r, int pos, int val) {
    	int p = ++ tot;
    	tree[p] = tree[now];
    	if(l == r) {
    		tree[p].val += val ;
    		return p;
    	}
    	int mid = l + r >> 1;
    	if(pos <= mid)
    		tree[p].lc = insert(tree[now].lc, l, mid, pos, val);
    	else
    		tree[p].rc = insert(tree[now].rc, mid + 1, r, pos, val);
    	tree[p].val = tree[tree[p].lc].val + tree[tree[p].rc].val;
    	return p;
    }
    
    int ask(int p, int q, int l, int r, int L, int R) {
    	if(L <= l && r <= R)
    		return tree[p].val - tree[q].val;
    
    	int mid = l + r >> 1;
    	int res = 0;
    	if(L <= mid)
    		res += ask(tree[p].lc, tree[q].lc, l, mid, L, R);
    	if(R > mid)
    		res += ask(tree[p].rc, tree[q].rc ,mid + 1, r, L, R);
    	return res;
    }
    
    int main() {
    	int T, casenum = 1;
        scanf("%d", &T);
        while(T--) {
            printf("Case %d:\n", casenum++);
            int cnt, pos, l, r, h;
            scanf("%d %d", &n, &m);
            for(int i = 1; i <= n; ++i) {
                scanf("%d", &a[i]);
                b[i] = a[i];
            }
            sort(b + 1, b + n + 1);
            cnt = unique(b + 1, b + 1 + n) - b - 1;
            tot = 0;
            root[0] = build(1, cnt);
            for(int i = 1; i <= n; ++i) {
                pos = lower_bound(b + 1, b + 1 + cnt, a[i]) - b;
                root[i] = insert(root[i - 1], 1, cnt, pos, 1);
            }
            for(int i = 1; i <= m; ++i) {
                scanf("%d %d %d", &l, &r, &h);
                l ++,r ++;
                pos = upper_bound(b + 1, b + cnt + 1, h) - b - 1;
                if(1 > pos) printf("0\n"); // 疯狂炸内存 这里我也是京了
                else printf("%d\n", ask(root[r],root[l - 1], 1, cnt, 1, pos));
            }
        }
        return 0;
    }
    

