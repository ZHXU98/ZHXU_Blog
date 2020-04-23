## Codeforces_Round_#586_(Div._1+Div._2)_D._Alex_and_Julia_(数论_二部图性质)

## D. Alex and Julian

<https://codeforces.com/contest/1220/problem/D>

二分图性质 无奇环  
而且 我们取尝试一些数据 发现 最后省的奇数最多才是最好的  
1 3 5 7 9  
2 6 10 14 18  
其中 1 2 是不可能同时出现 所以 这题慢慢看出规律了  
找最多的奇数层(2次幂一样多) 剩下不同层的删了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    //#define int long long
    const int maxn = 2e5 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef long long ll;
    typedef pair<int, int> P;
    int n, m, k, cas;
    
    ll a[maxn];
    vector<long long> v[70];
    
    signed main() {
    	cin >> n;
    	for(int i = 1; i <= n; i ++) cin >> a[i];
    
    	for(int i = 1; i <= n; i ++) {
    		ll x = a[i];
    		int j = 0;
    		while(x % 2 == 0) x /= 2, j ++;
    		v[j].push_back(a[i]);
    	}
    	int siz = v[0].size();
    	int pos = 0;
    	for(int i = 0; i <= 64; i ++) {
    		if(siz < v[i].size()) siz = v[i].size(), pos = i;
    	}
    	cout << n - siz << endl;
    	for(int i = 0; i <= 64; i ++) {
    		if(i == pos) continue;
    		for(int j = 0; j < v[i].size(); j ++) {
    			cout << v[i][j] << " ";
    		}
    	}
    	return 0;
    }
    

