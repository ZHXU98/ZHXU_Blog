## 2019_牛客多校第九场_D_Knapsack_(折半搜索)

#### D Knapsack Cryptosystem (折半搜索)

给了36 个数 给了 s 问 能不能凑出s  
我们折半分开搜索 复杂度下去 就好 二进制啊二进制 打印反了可还行  
唉 打印二进制 打印反了 wa了

    
    
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;
    const int maxn = 3e5 + 10;
     
    int n, m, sum, ky, sy;
    int a[40];
     
    struct node{
        int vis;
        int val;
    }w[maxn];
    int cnt;
    string str, sss;
     
    bool cmp(const node &a, const node &b) {
        return a.val < b.val;
    }
     
    void dfs(int u, int s, int vis) {
        if(s > sum) return ;
        if(u == ky) {
            w[++cnt] = node{vis,s};
            return ;
        }
        if(s + a[u] <= sum) dfs(u + 1, s + a[u], vis | (1 << u));
        dfs(u + 1, s, vis);
    }
     
    void dfs2(int u, int s, int vis) {
        if(s > sum) return;
        if(u == n) {
            int l = 1, r = cnt;
            int sl = sum - s;
            while(l < r) {
                int mid = l + r + 1 >> 1;
                if(w[mid].val <= sl) l = mid;
                else r = mid - 1;
            }
            if(w[l].val == sl) {
               for(int i = 0; i < sy ; i++) {
                   if(w[l].vis >> i & 1) cout << "1";
                   else cout << 0;
               }
               for(int i = sy; i < n; i ++) {
                   if(vis >> (i - sy) & 1) cout << 1;
                   else cout << 0;
               }
               exit(0);
            }
            return ;
        }
        if(s + a[u] <= sum) dfs2(u + 1, s + a[u], vis | (1 << (u - sy)));
        dfs2(u + 1, s, vis);
    }
     
    signed main() {
        scanf("%lld %lld", &n, &sum);
        for(int i = 0; i < n; i ++) scanf("%lld", a + i);
        ky = n/2;
        sy = n/2;
        dfs(0, 0, 0);
        sort(w + 1, w + cnt + 1, cmp);
        dfs2(ky, 0, 0);
        return 0;
    }
    

