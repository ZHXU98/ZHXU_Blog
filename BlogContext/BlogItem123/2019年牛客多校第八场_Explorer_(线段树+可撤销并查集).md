## 2019年牛客多校第八场_Explorer_(线段树+可撤销并查集)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190819125622308.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
线段树 上套每个区间可以有哪些并查集  
一直向下 如果已经有大区间的管道到n 这个点覆盖区间线段树 往下就不必要走了  
然后学会了 安秩合并(启发式搜索) 不能压缩路径 我们把大的合并到小的上面 就使得 长的 被查询的的路径 尽可能慢的长 这样不压缩路径 不超时 的完成
我们合并 和 实现撤销的操作

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    const int maxn = 2e5 + 10;
     
    int n, m, ans;
    struct edge {
        int u, v, l, r;
    } e[maxn];
    int b[maxn << 1];
    vector<int> tree[maxn << 2];
     
    void updata(int L, int R, int l, int r, int rt, int x) {
        if(L <= l && r <= R) {
            tree[rt].push_back(x);
            return ;
        }
        int mid = l + r >> 1;
        if(L <= mid) updata(L, R, l, mid, rt << 1, x);
        if(R > mid) updata(L, R, mid + 1, r, rt << 1 | 1, x);
    }
     
    int siz[maxn], fa[maxn];
    int finds(int x) {
        return x == fa[x] ? x : finds(fa[x]);
    }
     
    void query(int l, int r, int rt) {
        vector<int> vec;
        for(int i = 0; i < tree[rt].size(); i ++) {
            int x = tree[rt][i];
            int fau = finds(e[x].u), fav = finds(e[x].v);
            if(fau != fav) {
                if(siz[fau] < siz[fav]) swap(fau, fav);
                fa[fav] = fau; // 只增加小的查询代价
                siz[fau] += siz[fav];
                vec.push_back(fav);
            }
        }
        if(finds(1) == finds(n)) ans += b[r + 1] - b[l];
        else if(l < r) {
            int mid = l + r >> 1;
            query(l, mid, rt << 1);
            query(mid + 1, r, rt << 1 | 1);
        }
        for(int i = 0; i < vec.size(); i ++) fa[vec[i]] = vec[i];
        vec.clear();
    }
     
     
    int main() {
        fastio;
        cin >> n >> m;
        for(int i = 0; i <= n + 5; i ++) fa[i] = i, siz[i] = 1;
        for(int i = 1; i <= m; i ++) {
            cin >> e[i].u >> e[i].v >> e[i].l >> e[i].r;
            b[i * 2 - 1] = e[i].l, b[i * 2] = e[i].r + 1;
        }
        sort(b + 1, b + 1 + 2 * m);
        int cnt = unique(b + 1, b + 1 + 2 * m) - b - 1;
        for(int i = 1; i <= m; i ++) {
            int p1 = lower_bound(b + 1, b + 1 + cnt, e[i].l) - b;
            int p2 = lower_bound(b + 1, b + 1 + cnt, e[i].r + 1) - b - 1;
            updata(p1, p2, 1, cnt, 1, i);
        }
        query(1, cnt, 1);
        cout << ans << endl;
        return 0;
    

这是一开始的暴力代码 2333 真暴力啊

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef pair<int, int> P;
    const int maxn = 1e5 + 5;
     
    int n, m;
    int head[maxn], cnt;
    int to[maxn << 1], nxt[maxn << 1], ls[maxn << 1], rs[maxn << 1];
     
    map<pair<P, int>, int> vis;
    typedef pair<P, int> PP;
     
    void ade(int a, int b, int c, int d) {
        to[++cnt] = b, ls[cnt] = c, rs[cnt] = d;
        nxt[cnt] = head[a], head[a] = cnt;
    }
     
    struct node{
        int u;
        int l, r;
    };
     
    queue <node> que;
     
    struct NN{
        int l, r;
        bool operator < (const NN & a) const {
            return l < a.l || (l == a.l && r < a.r);
        }
    };
    vector <NN> ans;
    void bfs() {
        que.push(node{1, 0, (int)1e9});
     
        while(!que.empty()) {
            node p = que.front(); que.pop();
    //        if()
            if(vis.find(PP(P(p.l, p.r), p.u)) != vis.end()) continue;
            vis[PP(P(p.l, p.r), p.u)] = 1;
     
            for(int i =head[p.u]; i; i = nxt[i]) {
                int v= to[i];
     
                int l = p.l, r = p.r;
     
                l = max(l, ls[i]), r = min(rs[i], r);
     
                if(l > r) continue;
                if(vis.find(PP(P(l, r), v)) != vis.end() ) continue;
     
                que.push(node{v, l, r});
                if(v == n) {
                    ans.push_back(NN{l, r});
                }
            }
        }
    }
     
    signed main(){
        scanf("%d %d", &n, &m);
        for(int i = 1, a, b, c, d; i <= m; i ++) {
            scanf("%d %d %d %d", &a, &b, & c, &d);
            ade(a, b, c, d), ade(b, a, c, d);
        }
     
        bfs();
        if(ans.size()==0){
            cout<<0<<endl;
            return 0;
        }
        sort(ans.begin(), ans.end());
     
        int pre = ans[0].r;
        long long tot = ans[0].r - ans[0].l + 1;
    //  cout << ans[0].l << " " << ans[0].r << endl;
     
    //      cout << ans[i].l << " " << ans[i].r << endl;
     
        for(int i=1;i<ans.size();i++){
            if(pre>=ans[i].l){
                if(ans[i].r<=pre){
                    continue;
                }else{
                    tot+=ans[i].r-pre;
                    pre=ans[i].r;
                }
            }else{
                tot+=ans[i].r-ans[i].l+1;
                pre=ans[i].r;
            }
        }
        //cout << tot << endl;
        printf("%lld\n", tot);
        //cout << ans.size() << endl;
        return 0;
    }
    /*
    4 4
    1 2 1 10
    1 3 5 15
    2 4 1 10
    3 4 5 15
    */
    

