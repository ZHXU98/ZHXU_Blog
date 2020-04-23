## Educational_Codeforces_Round_72_Editorial_A~D题解

好菜啊这次打的 都是思维题 不难的 自己打的好菜 菜的无话可说  
<https://codeforces.com/contest/1217>

## A.Creating a Character

就是让 exp 分给 str 和 in 保证str 严格大于in  
这样的话 我们考虑把他们分屏的基础上算最多可以分几次  
用 （exp + str和in 的差 + 1 ）/ 2 算出分配状态  
但是 这坑了 然后str 非常大 in 怎么也补补上 那样的话 ans = exp + 1；  
所以先wa了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    const int maxn = 1e5 + 5;
    typedef pair<int, int> P;
    int n, m, k, p1, p2;
     
    signed main() {
    	cin >> p1;
    	while(p1 --) {
    		cin >> n >> m >> k;
    		cout << max(min((n + k - m + 1)/2, k + 1), 0ll) << endl;
    	}
    	return 0;
    }
    

## B. Zmei Gorynich

小学数学题 砍到 （m - 这轮最大值）/ 实际能砍 + 1

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    const int maxn = 1e5 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef long long ll;
    typedef pair<int, int> P;
    int n, m, k, p1, p2, t;
     
    const int p = 998244353;
     
    signed main() {
    	cin >> t;
    	while(t --) {
    		cin >> n >> m;
    		int mad = -1, mid = -1;
    		for(int i = 1; i <= n; i ++) {
    			cin >> p1 >> p2;
    			mid = max(mid, p1 - p2);
    			mad = max(p1, mad);
    		}
    		if(mad >= m) cout << 1 << endl;
    		else if(mid <= 0) cout << -1 << endl;
    		else cout << (m - mad) / mid + ((m - mad) % mid != 0) + 1 << endl;
    	}
    	return 0;
    }
    

## C. The Number Of Good Substrings

唉 T23  
考虑 01 串 我们暴力往后面找显然复杂度不对 我直接减了一个二进制数转10进制不大于5000 T了  
何时01串二进制 == 长度 而且优化的比较好呢  
我们还是枚举长度 因为最多 20位 我们设二进制现在值为tmp  
当tmp < len还好说 但是大于 我们考虑用前导0去补 这样 复杂度就下来了

    
    
    #include <bits/stdc++.h>
    using namespace std;
     
    int n, m, cas;
    string s;
     
    int main() {
        ios::sync_with_stdio(false), cin.tie(0);
        cin >> cas;
        while(cas --) {
            cin >> s;
            int len = s.size();
            int ans = 0, cnt = 0;
            for(int i = 0; i < len; ++ i) {
                if(s[i] == '0') cnt ++;
                else {
                    int tmp = 1;
                    for(int j = 1; j <= 20 && i + j - 1 < len; j ++) {
                        if(tmp <= cnt + j) ans ++;
                        tmp = tmp * 2 + (s[i + j] == '1');
                    }
                    cnt = 0;
                }
            }
            cout << ans << endl;
        }
        return 0;
    }
    

## D. Coloring Edges

这题可能比上一道题还简单  
很容易猜到 有环 就是2 无环 就是1  
而且 我们只要把最后成环的边换成2就好了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    
    int n, m, cas;
    int head[maxn], cnt;
    int to[maxn << 1], nxt[maxn << 1], val[maxn << 1];
    int vis[maxn], ans[maxn];
    
    void ade(int a, int b, int c) {
        to[++ cnt] = b, val[cnt] = c;
        nxt[cnt] = head[a], head[a] = cnt;
    }
    int f = 0;
    void dfs(int u) { // 有向图 用 1 2 表达 现在链 和 之前链
        vis[u] = 1;
        for(int i = head[u]; i; i = nxt[i]) {
            int v = to[i], id = val[i];
            if(!vis[v]) dfs(v), ans[id] = 1;
            else if(vis[v] == 2) {
                ans[id] = 1;
            } else {
                ans[id] = 2;
                f = 1;
            }
        }
        vis[u] = 2;
    }
    
    int main() {
        ios::sync_with_stdio(false), cin.tie(0);
        cin >> n >> m;
        for(int i = 1, a, b; i <= m; ++ i) {
            cin >> a >> b;
            ade(a, b, i);
        }
        for(int i = 1; i <= n; i ++) {
            if(!vis[i]) dfs(i);
        }
        cout << (f ? 2 : 1) << endl;
        for(int i = 1; i <= m; ++ i) cout << ans[i] << " ";
        return 0;
    }
    

