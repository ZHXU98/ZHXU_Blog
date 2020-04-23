## 【容斥】_codeforces1027D_Number_Of_Permutations_【数学】

[题目链接](https://codeforces.com/contest/1207/problem/D)

##### 题意

一些二元组(x,y)  
求多少种排列，使得x不递增(包括相等)，y不递增(包括相等)

第一反应 二维偏序 然后想想不对劲 这玩意有组合数  
所以想到了 倒着来求 可是 又要去重 很快 就意识到 是一个容斥问题了  
13样例 wa了3 发 真实。。。。 因为我减了2次 只加了一次mod 输出一直是负数 加 2 * mod 就过了 醉了  
细节要注意orz  
ps tmp3 等于0的情况 只有 整体不能产生 一个二维 都是 不递减的序列 这样产生不了first 和 second 重复的  
此时 必然是0

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0);
    #define int long long
    using namespace std;
    typedef long long ll;
    const int maxn = 3e5 + 10;
    const int mod = 998244353;
    
    int n;
    pair<int, int > a[maxn];
    int fec[maxn];
    
    bool cmp(pair<int, int> a, pair<int, int> b) {
    	return a.second < b.second;
    }
    
    signed main() {
    	fastio;
    	cin >> n;
    	fec[0] = 1;
    	for(int i = 1; i <= n; i ++) {
    		cin >> a[i].first >> a[i].second;
    		fec[i] = fec[i - 1] * i % mod;
    	}
    
    	sort(a + 1, a + 1 + n);
    
    	int tmp1 = 1;
    	int tmp2 = 1;
    	int tmp3 = 1;
    
    	for(int i = 1; i <= n; i ++) {
    		int cnt = 1;
    		int j = i + 1;
    		while(j <= n && a[i].first == a[j].first) cnt ++, j ++;
    		tmp1 = tmp1 * fec[cnt] % mod;
    		i = j - 1;
    	}
    	for(int i = 1; i <= n; i ++) {
    		int cnt = 1;
    		int j = i + 1;
    		while(j <= n && a[i].first == a[j].first && a[i].second == a[j].second) cnt ++, j ++;
    		tmp3 = tmp3 * fec[cnt] % mod;
    		i = j - 1;
    	}
    
    	for(int i = 1; i < n; i ++)
    		if(a[i].second > a[i + 1].second) tmp3 = 0;
    
    
    	sort(a + 1, a + 1 + n, cmp);
    	for(int i = 1; i <= n; i ++) {
    		int cnt = 1;
    		int j = i + 1;
    		while(j <= n && a[i].second == a[j].second) cnt ++, j ++;
    		tmp2 = tmp2 * fec[cnt] % mod;
    		i = j - 1;
    	}
    
    	int sum = 1;
    	for(int i = 1; i <= n; i ++)
    		sum = (sum * i) % mod;
    		
    	cout << (sum - tmp1 - tmp2 + tmp3 + 2 * mod) % mod << endl;
    
    	return 0;
    }
    

