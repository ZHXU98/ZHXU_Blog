## 2019HDU杭电多校第六场_HDU_6638_Snowy_Smile_(二维最大矩阵和_线段树)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190810202933875.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190810202937290.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
一个巨大的矩阵 1e9 之间 稀疏矩阵

现在给了一些点权值 让你找一个矩形 圈主的权值全拿了 问最多可以拿多少  
hdu MAXsum 有一维的题 不带修改  
如果带修改 也只是 线段树维护 最大子段和的题  
<https://blog.csdn.net/qq_40831340/article/details/90726050>

这次 变成二维的了  
我们选择离散化数据 枚举上下边界 用子段和最大的方式来寻求可能存在的最大矩阵  
选区 x 轴 离散化 枚举y高度上下界 再这一过程中 maxsum 直接可以找到每轮加入的点 可能产生的最大值  
特地的注意 再跑上界的过程中 我们就已经可以算 枚举底部 到现在枚举上界的里面的最大值了 我第一次整成 n^3 lgn了

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    const int maxn = 4e3 + 5;
    const int INF = 0x3f3f3f3f;
    typedef pair<int, int> P;
    typedef long long ll;
    int cas, n;
    struct point{
        int x, y, val;
        bool operator < (const point &a) const {
            return y < a.y;
        }
    }a[maxn];
    vector<int> p[maxn];
    
    int bx[maxn], by[maxn];
    
    struct node{
        ll sum, lmax, rmax, lrmax;
    }tree[maxn << 2];
    
    void push_up(int rt) {
        tree[rt].sum = tree[rt << 1].sum + tree[rt << 1 | 1].sum;
        tree[rt].lmax = max(tree[rt << 1].lmax, \
                            tree[rt << 1].sum + tree[rt << 1 | 1].lmax);
        tree[rt].rmax = max(tree[rt << 1 | 1].rmax, \
                            tree[rt << 1 | 1].sum + tree[rt << 1].rmax);
        tree[rt].lrmax = max( max(tree[rt << 1].lrmax, tree[rt << 1 | 1].lrmax), \
                             tree[rt << 1].rmax + tree[rt << 1 | 1].lmax);
    }
    
    void updata(int L, int l, int r, int rt, int c) {
        if(l == r) {
            tree[rt].sum += 1ll * c;
            tree[rt].lrmax = tree[rt].lmax = tree[rt].rmax = tree[rt].sum;
            return ;
        }
        int mid = l + r >> 1;
        if(L <= mid) updata(L, l, mid, rt << 1, c);
        else updata(L, mid + 1, r, rt << 1 | 1, c);
        push_up(rt);
    }
    
    int main() {
        scanf("%d", &cas);
        while(cas --) {
            scanf("%d", &n);
            for(int i = 1; i <= n; i ++) {
                scanf("%d %d %d", &a[i].x, &a[i].y, &a[i].val);
                bx[i] = a[i].x, by[i] = a[i].y;
            }
            sort(bx + 1, bx + 1 + n);
            int xlen = unique(bx + 1, bx + 1 + n) - bx - 1;
            sort(by + 1, by + 1 + n);
            int ylen = unique(by + 1, by + 1 + n) - by - 1;
    
            for(int i = 1; i <= n; i ++) {
                a[i].x = lower_bound(bx + 1, bx + xlen + 1, a[i].x) - bx;
                a[i].y = lower_bound(by + 1, by + ylen + 1, a[i].y) - by;
            }
            sort(a + 1, a + 1 + n);
            ll ans = 0;
            for(int i = 1; i <= ylen; i ++) {
                memset(tree, 0, (xlen * 4 + 5) * sizeof(node));
                int pos = 1;
                while(a[pos].y < i && pos <= n) pos ++;
                for(int j = i; j <= ylen; j ++) {
                    while(a[pos].y <= j && pos <= n) {
                        updata(a[pos].x, 1, xlen, 1, a[pos].val);
                        pos ++;
                    }
                    ans = max(ans, tree[1].lrmax);
                }
            }
            printf("%lld\n", ans);
        }
        return 0;
    }
    

