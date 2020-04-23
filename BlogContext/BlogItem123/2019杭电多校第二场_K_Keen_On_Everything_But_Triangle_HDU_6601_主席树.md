## 2019杭电多校第二场_K_Keen_On_Everything_But_Triangle_HDU_6601_主席树

给了长度为n得序列  
问 l r 区间最大得三角形周长  
首先 ai 在 1e9 之内 所以最多跑50 个边就确定是否存在 合法三角形了  
所以这里建主席树维护区间k值就好 记得主席树初始化除了建树 还要 tot = 0

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    
    int n, m;
    int a[maxn], b[maxn];
    struct node {
    	int lc,rc;
    	int val;
    } tree[maxn * 20];
    int tot,root[maxn];
    
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
    
    int ins(int now, int l, int r, int x, int val) {
    	int p = ++ tot;
    	tree[p] = tree[now];
    	if(l == r) {
    		tree[p].val += val ;
    		return p;
    	}
    	int mid = l + r >> 1;
    	if(x <= mid) 
    		tree[p].lc = ins(tree[now].lc, l, mid, x, val);
    	else 
    		tree[p].rc = ins(tree[now].rc, mid + 1, r, x, val);
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
    
    signed main(){
        while(~scanf("%d %d", &n, &m)){
    		tot = 0;
            for(int i = 1; i <= n; i ++) scanf("%d", &a[i]), b[i] = a[i];
            sort(b + 1, b + 1 + n);
            int cnt = unique(b + 1, b + 1 + n) - b - 1;
            root[0] = build(1, cnt);
            for(int i = 1; i <= n; i++) {
                int pos = lower_bound(b + 1, b + 1 + cnt, a[i]) - b;
                root[i] = ins(root[i - 1], 1, cnt, pos, 1);
            }
            while(m -- ){
                int l, r;
                scanf("%d %d", &l, &r);
                int num = r - l + 1;
                if(num < 3) {
                    printf("-1\n");
                    continue;
                }
                long long ans = -1;
                for(int i = num; i >= max(3, num - 45); i -- ){
                    int a1 = b[ask(root[r], root[l - 1], 1, cnt, i)];
                    int b1 = b[ask(root[r], root[l - 1], 1, cnt, i - 1)];
                    int c1 = b[ask(root[r], root[l - 1], 1, cnt, i - 2)];
                    if(1ll * a1 + b1 > c1 && 1ll * a1 - b1 < c1) {
                        ans = 1ll * a1 + b1 + c1;
                        break;
                    }
                }
                printf("%lld\n", ans);
            }
        } 
        return 0;
    }
    

