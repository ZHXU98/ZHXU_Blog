## Educational_Codeforces_Round_73_(Rated_for_Div._2)_C_D题

## C - Perfect Team

<https://codeforces.com/contest/1221/problem/C>  
推公式 好像有人二分过了  
首先 我们把 n m k 人分好 n m先分一样  
多余的扔k里面

    
    
    	    int t = max(n, m) - min(n, m);
    		k += t;
    		if(n > m) n -= t;
    		else m -= t;
    

这样 第一轮组队 是 min(n, m, k);

然后 我们设答案为 ans  
k多了 也没有意义 第一轮把n，m掏空了 直接就是ans  
取 n 中 x人 m y 人  
x _2 + y <= n ;  
x + y_2 <= m ;  
所以 ans <=( n + m )/3

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e3 + 5;
    typedef long long ll;
    int n, m, k, sy;
    int cas;
    int a[maxn][maxn];
     
    struct node {
    	int x, y, f;
    };
     
    const int cx[] = {1, 2, -1, -2, 1, 2, -1, -2};
    const int cy[] = {2, 1, -2, -1, -2, -1, 2, 1};
     
    signed main() {
    	scanf("%d", &cas);
    	while(cas --) {
    		cin >> n >> m >> k;
    		int t = max(n, m) - min(n, m);
    		k += t;
    		if(n > m) 
    			n -= t;
    		else m -= t;
    		int ans = min(n, min(m, k));
    		n -= ans;
    		m -= ans;
    		k -= ans;
    		ans += (n + m) / 3;
    		cout << ans << endl;
    	}
    	return 0;
    }
    

## D - Make The Fence Great Again

dp orz  
题目要求不能2个连续高度一样 这样 最多一个数据+2 就可以和2边不一样了  
这样维度据确定了  
dp[0/1/2][pos] 如果前一个位置 + k 和当前位置 + j 不一样 花费转义最小就好 开不开longlong wa2

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 3e5 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef long long ll;
    int n, m, k, sy;
    int cas;
     
    long long dp[4][maxn];
    long long a[maxn], b[maxn];
     
    signed main() {
    	scanf("%d", &cas);
    	while(cas --) {
    		scanf("%d", &n);
    		for(int i = 1; i <= n; i ++) {
    			scanf("%lld %lld", a + i, b + i);
    			dp[0][i] = dp[1][i] = dp[2][i] = INF;
    		}
    		dp[0][1] = 0;
    		dp[1][1] = b[1];
    		dp[2][1] = b[1] << 1;
    		for(int i = 2; i <= n; i ++) {
    			for(int j = 0; j <= 2; j ++) {
    				for(int k = 0; k <= 2; k ++) {
    					if(a[i] + j != a[i - 1] + k) {
    						dp[j][i] = min(dp[j][i], dp[k][i - 1] + j * b[i]);
    					}
    				}
    			}
    		}
    		printf("%lld\n", min(dp[0][n], min(dp[1][n], dp[2][n])));
    	}
    	return 0;
    }
    

