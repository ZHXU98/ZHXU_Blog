## 2019CCPC-网络选拔赛_HDU-6703_array_(主席树_or_线段树)

CY提供的 主席树思路  
<https://blog.csdn.net/chenyume/article/details/100045386>

> 题意：给出一个序列，保证序列是一个1~n的全排列，q次操作，两种类型，一是给a[i] a[i]a[i]加107 10^710  
>  7  
>  ，另一种是给出r，k，询问一个最小的数字x，使得x>=k
> x>=kx>=k，x不等于区间[1,r]内的任何一个数字，强制在线，数据范围：n,m<=105,k<=n n,m<=10^5,k<=nn,m<=10  
>  5  
>  ,k<=n  
>
> 首先观察数据发现查询的答案最多为n+1，对于操作1来说，给一个数字加一个超大的数字，是不是说明，这个数字之后就不会再出现了，他是不是就可以作为查询的答案之一了。  
>  再考虑序列是一个1~n的全排列，那么是不是说明[1,r] [1,r][1,r]内出现过的数字，在区间[r+1,n]
> [r+1,n][r+1,n]内都不会出现了。  
>  所以序列从后往前向主席树中添加数字，对于操作1如果这个数字被修改了就放到set中，那对于查询操作，是不是查询[r+1,n]
> [r+1,n][r+1,n]的主席树中大于等于k的最小值，set中大于等于k的最小值两者取最小即可。  
>  为了查询的方便，同时维护一个区间最大数字。
    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    int a[maxn];
    int n, m, cas;
    set<int> S;
    
    struct node {
        int lc, rc;
        int mas;
    } tree[maxn * 20];
    int tot, root[maxn];
    
    int build(int l, int r) {
        int p = ++ tot;
        tree[p].mas = 0;
        if(l == r) return p;
        int mid = l + r >> 1;
        tree[p].lc = build(l, mid);
        tree[p].rc = build(mid + 1, r);
        return p;
    }
    
    int ins(int now, int l, int r, int x) {
        int p = ++ tot;
        tree[p] = tree[now];
        if(l == r) {
            tree[p].mas = l;
            return p;
        }
        int mid = l + r >> 1;
        if(x <= mid) tree[p].lc = ins(tree[now].lc, l, mid, x);
        else tree[p].rc = ins(tree[now].rc, mid + 1, r, x);
        tree[p].mas = max(tree[tree[p].lc].mas, tree[tree[p].rc].mas);
        return p;
    }
    
    int query(int now, int l, int r, int k) {
        if(tree[now].mas < k) return n + 1;
        if(l == r) return l;
        int mid = l + r >> 1;
        if(mid >= k && tree[tree[now].lc].mas >= k) return query(tree[now].lc, l, mid, k);
        else return query(tree[now].rc, mid + 1, r, k);
    }
    bool vis[maxn];
    int main() {
        scanf("%d", &cas);
        while(cas --) {
            S.clear();
            tot = 0;
            scanf("%d %d", &n, &m);
            S.insert(n + 1);
            for(int i = 1; i <= n; i ++) scanf("%d", &a[i]);
            root[n + 1] = build(1, n);
            for(int i = n; i >= 1; i --)
                root[i] = ins(root[i + 1], 1, n, a[i]);
    
            int op, pos, k, r, pre = 0;
            int t1, t2, t3;
            while(m --) {
                scanf("%d", &op);
                if(op == 1) {
                    scanf("%d", &t1);
                    pos = t1 ^ pre;
                    S.insert(a[pos]);
                } else {
                    scanf("%d %d", &t2, &t3);
                    r = t2 ^ pre, k = t3 ^ pre;
                    int ans = min(query(root[r + 1], 1, n, k), *S.lower_bound(k));
                    printf("%d\n", ans);
                    pre = ans;
                }
            }
        }
        return 0;
    }
    

官方题解  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190824100530604.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    int tree[maxn << 2];
    int cas, n, m;
    int a[maxn];
    
    void build(int l, int r, int rt) {
    	tree[rt] = 0;
    	if(l == r) return ;
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1);
    	build(mid + 1, r, rt << 1 | 1);
    }
    
    void updata(int L, int pos, int l, int r, int rt) {
    	if(l == r) {
    		tree[rt] = pos;
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, pos, l, mid, rt << 1);
    	else updata(L, pos, mid + 1, r, rt << 1 | 1);
    	tree[rt] = max(tree[rt << 1], tree[rt << 1 | 1]);
    }
    
    int query(int l, int r, int rt, int k, int pos) {
    	if(tree[rt] <= pos) return -1;
    	if(l == r) return l;
    	int mid = l + r >> 1;
    	int res = - 1;
    	if(k <= mid) res = query(l, mid, rt << 1, k, pos);
    	if(res == - 1) res = query(mid + 1, r, rt << 1 | 1, k, pos); 
    	return res;
    }
    
    signed main() {
    	scanf("%d", &cas);
        while(cas --) {
            scanf("%d %d", &n, &m);
            build(1, 1 + n, 1);
            for(int i = 1; i <= n; i ++) {
            	scanf("%d", &a[i]);
            	updata(a[i], i, 1, n + 1, 1);
    		} 
    		updata(n + 1, n + 1, 1, n + 1, 1);
            int op, pos, k, r, pre = 0;
            int t1, t2, t3;
            while(m --) {
                scanf("%d", &op);
                if(op == 1) {
                    scanf("%d", &t1);
                    pos = t1 ^ pre;
                    updata(a[pos], n + 1, 1, n + 1, 1);
                } else {
                    scanf("%d %d", &t2, &t3);
                    r = t2 ^ pre, k = t3 ^ pre;
                    int ans = query(1, n + 1, 1, k, r);
                    printf("%d\n", ans);
                    pre = ans;
                }
            }
        }
    	return 0;
    }
    

