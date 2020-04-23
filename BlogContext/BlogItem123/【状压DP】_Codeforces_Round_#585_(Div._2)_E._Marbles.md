## 【状压DP】_Codeforces_Round_#585_(Div._2)_E._Marbles

> The main fact is that the number of colors is less than 20, which allows us
> to use exponential solutions.  
>  For each pair of colors (i,j), we can calculate cnt[i][j] — the number of
> swaps required to place all marbles of color i before all marbles of color j
> (if we consider only marbles of these two colors). We can store a sorted
> vector for each color, and calculate this information for a fixed pair with
> two pointers.  
>  Then let’s use subset DP to fix the order of colors. Let d[mask] be the
> minimum number of operations to correctly order all marbles from the mask of
> colors. Let’s iterate on the next color we consider — it should be a
> position in binary representation of mask with 0 in it. We will place all
> marbles of this color after all marbles we already placed. If we fix a new
> color i, let’s calculate the sum (the additional number of swaps we have to
> make) by iterating on the bit j equal to 1 in the mask, and increasing sum
> by cnt[j][i] for every such bit. The new state of DP can be calculated as
> nmask=mask|(1«i). So the transition can be implemented as
> d[nmask]=min(d[nmask],d[mask]+sum).  
>  The answer is the minimum number of swaps required to place all the colors,
> and that is d[220−1].

状态压缩DP  
比较难orz  
只有20个不同数据 我们怎么才能最少移动(相邻)的将他们同数据连续的聚在一起  
首先想到是DP啦 能不能写出来就是令一回事情了  
我们从空状态开始枚举  
1 << 20 可以吧 我们放入 数据各种加入顺序枚举出来 相对于(这个状态)之前没有出现过的 放入尾 这样每次看 加入数据放后面最小操作数DP 代价就是
出现过的数据 版它前面的要移动多少次 我们可以预处理出来

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e3 + 5;
    typedef long long ll;
    int n, m, k, sy;
    int cas;
    vector<int> v[25];
    long long val[25][25];
    long long dp[1100000];
     
    signed main() {
    	scanf("%d", &n);
    	for(int i = 1, x; i <= n; i ++) {
    		scanf("%d", &x);
    		v[x].push_back(i);
    	} 
    	for(int i = 1; i <= 20; i ++) {
    		for(int j = 1; j <= 20; j ++) {
    			if(i == j) continue;
    			for(int p = 0; p < v[i].size(); p ++) {
    				if(v[j].empty() || v[j][0] > v[i][p]) continue;
    				val[i][j] += lower_bound(v[j].begin(), v[j].end(), v[i][p]) - v[j].begin();
    			}
    		}
    	}
    	int tot = 1 << 20;
    	memset(dp, 0x3f, sizeof dp);
    	dp[0] = 0;
    	for(int i = 0; i < tot; ++ i) {
    		for(int j = 0 ; j < 20; ++ j) {
    			if(!(i >> j & 1)) {
    				ll ans = 0;
    				for(int k = 0; k < 20; ++ k) 
    					if((i >> k) & 1) ans += val[k + 1][j + 1];
    				// 所有这轮dp新加 扔最后 找最小代价
    				dp[i | (1 << j)] = min(dp[i | (1 << j)], dp[i] + ans); 
    			}
    		}
    	}
    	printf("%lld\n", dp[tot - 1]);
    	return 0;
    }
    

