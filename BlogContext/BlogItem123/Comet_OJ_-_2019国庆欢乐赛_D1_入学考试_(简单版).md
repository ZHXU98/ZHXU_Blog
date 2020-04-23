## Comet_OJ_-_2019国庆欢乐赛_D1_入学考试_(简单版)

<https://www.cometoj.com/contest/68/problem/D1?problem_id=3936>

## 入学考试 (简单版)

枚举 已经做完的卷子数量  
然后 剩下的时间 我们二分它最多每个卷子可以做多少题 然后剩下的时间我们自己在补进去

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int N = 1e5 + 5;
    int Q, n, m, k;
    long long D;
    long long t[N], tt[N];
    
    signed main() {
    	scanf("%d", &Q);
    	while(Q --) {
    		long long ans = 0;
    		scanf("%d %d %d %lld", &n, &m, &k, &D);
    		for(int i = 1; i <= m; i++) {
    			scanf("%d", &t[i]);
    		}
    		sort(t + 1, t + 1 + m);
    		for (int i = 1; i <= m; i++) tt[i] = tt[i - 1] + t[i];
    		if (D >= tt[m] * n){
    			printf("%lld\n", 1ll * n * (m + k));
    			continue;
    		}
    		for(int i = 0; i <= n; i++) {
    			long long sy = D - tt[m] * i;
    			long long score = i * (m + k);
    			if (sy < 0) break;
    			int l = 0, r = m;
    			while(l < r) {
    				int mid = (l + r + 1) >> 1;
    				if (tt[mid] * (n - i) <= sy) l = mid;
    				else r = mid - 1;
    			}
    			score += (1LL * n - i) * l + (sy - tt[l] * (1LL * n - i)) / t[l + 1];
    			ans = max(score, ans);
    		}
    		printf("%lld\n", ans);
    	}
    	return 0;
    }
    

