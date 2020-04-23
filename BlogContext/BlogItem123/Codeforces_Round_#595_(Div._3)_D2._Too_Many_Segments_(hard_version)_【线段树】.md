## Codeforces_Round_#595_(Div._3)_D2._Too_Many_Segments_(hard_version)_【线段树】

## D2. Too Many Segments (hard version)

<https://codeforces.com/contest/1249/problem/D2>  
题意 ：给了n个线段区间让你尽可能的删最少的线段 使每个区间线段覆盖次数少于k次  
思维题 D1直接贪过去 我们找这个区间最远R的 剪掉 这样一定对后面的最优  
D2 暴力就找最远的显然不合适了  
这里我想的 优先队列 存之前到这个点所有的线段 用Vector 存L点 开始出现的线段  
之后 我们直接开始从1 遍历到 N 那个点大于K 我们选择最远的R 来把它-1 直到小于等于K

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 10;
    const int N = 2e5;
    int n, k;
    struct Seg {
        int id;
        int l, r;
        bool operator < (const Seg & a) const {
            return r < a.r;
        }
    } seg[maxn];
    vector<Seg> G[maxn];
    priority_queue<Seg> que;
    vector<int> ans;
    
    int tree[maxn << 2], add[maxn << 2];
    inline void push_up(int rt) {
        tree[rt] = max(tree[rt << 1], tree[rt << 1 | 1]);
    }
    
    inline void push_down(int rt) {
        if(!add[rt])
            return ;
        tree[rt << 1] += add[rt];
        tree[rt << 1 | 1] += add[rt];
        add[rt << 1] += add[rt];
        add[rt << 1 | 1] += add[rt];
        add[rt] = 0;
    }
    
    void updata(int L, int R, int l, int r, int rt, int val) {
        //cout << l << " " << r << endl;
        if(L <= l && R >= r) {
            tree[rt] += val;
            add[rt] += val;
            return ;
        }
        push_down(rt);
        int mid = l + r >> 1;
        if(L <= mid)
            updata(L, R, l, mid, rt << 1, val);
        if(R > mid)
            updata(L, R, mid + 1, r, rt << 1 | 1, val);
        push_up(rt);
    }
    
    int query(int L, int l, int r, int rt) {
        if(l == r)
            return tree[rt];
        push_down(rt);
        int mid = l + r >> 1;
        if(L <= mid)
            return query(L, l, mid, rt << 1);
        else
            return query(L, mid + 1, r, rt << 1 | 1);
    }
    
    int main() {
        cin >> n >> k;
        for(int i = 1; i <= n; i ++) {
            cin >> seg[i].l >> seg[i].r;
            seg[i].id = i;
            G[seg[i].l].push_back(seg[i]);
            updata(seg[i].l, seg[i].r, 1, N, 1, 1);
        }
    
    //    for(int i = 1; i <= 15; i ++) {
    //        cout << query(i, 1, N, 1) << " ";
    //    }  cout << endl;
    
    //    sort(seg + 1, seg + 1 + n, [](const Seg & a, const Seg & b) {
    //        return a.l < b.l || (a.l == b.l && a.r < b.r);
    //    }); // 本来想遍历线段 发现不用 复习下这个写法 lambda 函数 匿名函数
    
        for(int i = 1; i <= N; i ++) {
            for(Seg t : G[i]) que.push(t);
            while(query(i, 1, N, 1) > k) {
                Seg t = que.top();
                que.pop();
                ans.push_back(t.id);
                updata(t.l, t.r, 1, N, 1, -1);
            }
        }
        cout << ans.size() << endl;
        for(int i = 0; i < ans.size(); i ++) {
            if(i != 0) cout << " ";
            cout << ans[i];
            if(i == ans.size() - 1) cout << endl;
        }
        return 0;
    }
    
    

