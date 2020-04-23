## 2019牛客多校第九场_H_Cutting_Bamboos_(二分主席树)

看到题解说二分 心里也有数了。。。。。

## H Cutting Bamboos

给了一些高度得柱子 每区间你可以坎y次 y次之后 必须砍没有了  
没有砍 总长度得一样 问第x次砍得高度在哪里  
因为砍得次数 和 每次砍得总长度是一定得 我们二分高度  
这样 剩下得总长度 就可以用来做二分 得判断了  
小于高度得总距离 和高于 高度得数量 * mid 就是我们剩下得距离

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 10;
    const double dmax = 1e9 + 1;
    const double eps = 1e-8;
     
    int n, m;
    int a[maxn], b[maxn], cnt;
    long long sum[maxn];
    struct node {
        int lc, rc;
        long long sum;
        int cnt;
    } tree[maxn * 20];
    int tot, root[maxn];
     
    int build(int l, int r) {
        int p = ++ tot;
        if(l == r) return p;
        int mid = l + r >> 1;
        tree[p].lc = build(l, mid);
        tree[p].rc = build(mid + 1, r);
        return p;
    }
     
    int ins(int now, int l, int r, int x, long long val) {
        int p = ++ tot;
        tree[p] = tree[now];
        if(l == r) {
            tree[p].sum += val;
            tree[p].cnt += 1;
            return p;
        }
        int mid = l + r >> 1;
        if(x <= mid) tree[p].lc = ins(tree[now].lc, l, mid, x, val);
        else tree[p].rc = ins(tree[now].rc, mid + 1, r, x, val);
        tree[p].sum = tree[tree[p].lc].sum + tree[tree[p].rc].sum;
        tree[p].cnt = tree[tree[p].lc].cnt + tree[tree[p].rc].cnt;
        return p;
    }
     
    long long ask_sum(int p, int q, int l, int r, int L, int R) {
        if(R < L) return 0;
        if(L <= l && r <= R) return tree[p].sum - tree[q].sum;
        int mid = l + r >> 1;
        long long res = 0;
        if(L <= mid) res += ask_sum(tree[p].lc, tree[q].lc, l, mid, L, R);
        if(R > mid) res += ask_sum(tree[p].rc, tree[q].rc, mid + 1, r, L, R);
        return res;
    }
     
    int ask_cnt(int p, int q, int l, int r, int L, int R) {
        if(R < L) return 0;
        if(L <= l && r <= R) return tree[p].cnt - tree[q].cnt;
        int mid = l + r >> 1;
        int res = 0;
        if(L <= mid) res += ask_cnt(tree[p].lc, tree[q].lc, l, mid, L, R);
        if(R > mid) res += ask_cnt(tree[p].rc, tree[q].rc, mid + 1, r, L, R);
        return res;
    }
     
    bool chk(int l, int r, double mid, double tall) {
        int pos = lower_bound(b + 1, b + 1 + cnt, ceil(mid)) - b;
        int k = ask_cnt(root[r], root[l - 1], 1, cnt, pos, cnt);
        long long presum = ask_sum(root[r], root[l - 1], 1, cnt, 1, pos - 1);
        if(1.0 * presum + k * mid > tall) return 0;
        else return 1;
    }
     
    signed main() {
        cin >> n >> m;
        for(int i = 1; i <= n; i ++)
            cin >> a[i], sum[i] = sum[i - 1] + a[i], b[i] = a[i];
     
        sort(b + 1, b + 1 + n);
        cnt = unique(b + 1, b + 1 + n) - b - 1;
        root[0] = build(1, cnt);
        for(int i = 1; i <= n; i ++) {
            int pos = lower_bound(b + 1, b + 1 + cnt, a[i]) - b;
            root[i] = ins(root[i - 1], 1, cnt, pos, 1ll * a[i]);
        }
         
        while(m --) {
            int L, R, X, Y;
            cin >> L >> R >> X >> Y;
            double l = 0, r = dmax;
            double tall = 1.0 * (1.0 * sum[R] - sum[L - 1]) / Y * (Y - X);
            while(r - l > eps) {
                double mid = (l + r) / 2;
                if( chk(L, R, mid, tall) ) l = mid;
                else r = mid;
            }
            printf("%.12lf\n", l);
        }
        return 0;
    }
    

