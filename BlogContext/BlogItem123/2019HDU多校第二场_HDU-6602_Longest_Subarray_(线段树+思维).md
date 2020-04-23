## 2019HDU多校第二场_HDU-6602_Longest_Subarray_(线段树+思维)

题意： 长度为n的序列，求最大的子序列长度，要求子序列中所出现的数字个数>=k。

思路： 枚举右边界r，线段树维护左边界l的范围。  
对于每一个数a[r]来说，我们可以清楚的知道 l 可以在什么地方

放入一个 a[r] 对于 i 位置 c - 1数据不需要出现  
对于它之前出现的 我们是要选择r这个位置的数据的 所以我们要把 它前一个数据位置 到 r - 1 先 -1  
选择r位置 就把之前位置在的地方a[r] 数据出现减去

离a[r]最近的 同一个数据数子 位置为 P1，  
离a[r]第k远的 同一个数据数字 位置 P2，  
它这个数据之后的下一位 P3  
当 a[r] 加入 p2 ~ p3 位置 就对a[r] 数据 合法了 区间 + 1；  
右边 就是 p1 ~ r 这个区间 与之前 1 ~ p2 ~ p3 区间 对应  
最后查询区间个数>=c的最左边的边界l即可。

    
    
    #include<bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 10;
    
    int tree[maxn << 2], lazy[maxn << 2];
    vector<int> vec[maxn];
    int n, C, k;
    int a[maxn], pos[maxn];
    
    void push_up(int rt) {
        tree[rt] = max(tree[rt << 1], tree[rt << 1 | 1]);
    }
    
    void push_down(int rt) {
        if(lazy[rt] == 0) return ;
        tree[rt << 1] += lazy[rt];
        lazy[rt << 1] += lazy[rt];
        tree[rt << 1 | 1] += lazy[rt];
        lazy[rt << 1 | 1] += lazy[rt];
        lazy[rt] = 0;
    }
    
    void build(int l, int r, int rt) {
        tree[rt] = lazy[rt] = 0;
        if(l == r) return ;
        int mid = l + r >> 1;
        build(l, mid, rt << 1);
        build(mid + 1, r, rt << 1 | 1);
    }
    
    void updata(int L, int R, int l, int r, int rt, int val) {
        if(L > R) return ;
        if(L <= l && R >= r) {
            tree[rt] += val;
            lazy[rt] += val;
            return ;
        }
        push_down(rt);
        int mid = l + r >> 1;
        if(L <= mid) updata(L, R, l, mid, rt << 1, val);
        if(R > mid) updata(L, R, mid + 1, r, rt << 1 | 1, val);
        push_up(rt);
    }
    
    int query(int l, int r, int rt) {
        if(tree[rt] < C) return 0;
        if(l == r) return l;
        push_down(rt);
        int mid = l + r >> 1;
        if(tree[rt << 1] >= C) return query(l, mid, rt << 1);
        if(tree[rt << 1 | 1] >= C) return query(mid + 1, r, rt << 1 | 1) ;
    }
    
    int main() {
        while(~scanf("%d%d%d", &n, &C, &k)) {
            for(int i = 1; i <= C; i++)
                vec[i].clear(), vec[i].emplace_back(0);
            int ans = 0;
            build(1, n, 1);
            for(int i = 1, x; i <= n; i++) {
                scanf("%d", &x);
                updata(i, i, 1, n, 1, C - 1);
                updata(vec[x].back() + 1, i - 1, 1, n, 1, -1);
                vec[x].emplace_back(i);
                int t = vec[x].size() - k - 1;
                if(t >= 0) updata(vec[x][t] + 1, vec[x][t + 1], 1, n, 1, 1);
                int j = query(1, n, 1);
                if(j) ans = max(ans, i - j + 1);
            }
            cout << ans << endl;
        }
        return 0;
    }
    

