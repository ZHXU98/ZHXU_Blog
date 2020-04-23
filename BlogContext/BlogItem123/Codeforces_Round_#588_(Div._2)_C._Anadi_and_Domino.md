## Codeforces_Round_#588_(Div._2)_C._Anadi_and_Domino

<https://codeforces.com/contest/1230>  
直接搜 主要是题解的方法太秀了 居然还能想到 找直接冲突点 然后减掉 找最大值

下面是直接搜索的 枚举每个点可以是啥 6*6的乱搞

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 50;
    
    int n, m, ans;
    int x[maxn], y[maxn];
    int val[maxn];
    bool vis[maxn][maxn];
    
    void dfs(int pos) {
        if(pos > n) {
            int tmp = 0;
            memset(vis, 0, sizeof vis);
            for(int i = 1; i <= m; i++) {
                int a = val[x[i]], b = val[y[i]];
                if(!vis[a][b]) {
                    tmp ++;
                    vis[a][b] = vis[b][a] = 1;
                }
            }
            ans = max(tmp, ans);
            return ;
        }
        for(int i = 1; i <= 6; i++) {
            val[pos] = i;
            dfs(pos + 1);
        }
    }
    
    int main() {
        cin >> n >> m;
        // 题解也是nb啊  n <= 6的时候就是m个  6个完全够放不同的  最多15边
        for(int i = 1, a, b; i <= m ;i ++)
            cin >> x[i] >> y[i];
        dfs(1);
        cout << ans <<endl;
    	return 0;
    }
    
    
    
    #include <bits/stdc++.h>
    using namespace std;
    //#define int long long
    const int maxn = 2e5 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef long long ll;
    typedef pair<int, int> P;
    int n, m, k, cas;
    int mp[15][15];
     
    signed main() {
    	cin >> n >> m;
    	for(int i = 1, a, b; i <= m; i ++) {
    		cin >> a >> b;
    		mp[a][b] = mp[b][a] = 1;
    	}
    	if(n <= 6) cout << m << endl;
    	else {
    		int ans = 1000;
    		for(int i = 1; i <= n; i ++) {
    			for(int j = i + 1; j <= n;  j++) {
    				int tmp = 0;
    				for(int k = 1; k <= n; k ++) {
    					if(mp[i][k] && mp[j][k]) tmp ++;
    				}
    				ans = min(tmp, ans);
    			}
    		}
    		cout << m - ans << endl;
    	}
    	return 0;
    }
    

