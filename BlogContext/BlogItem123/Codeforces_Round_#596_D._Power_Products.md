## Codeforces_Round_#596_D._Power_Products

## D. Power Products

<https://codeforces.com/contest/1247/problem/D>

题意 找 ai∗aj=xka_i * a_j = x^kai​∗aj​=xk 的数量  
既然是 xkx^kxk 那么 对于能乘出他们的数来说 质因子的质数个数必然是 k 的倍数  
然后就可以考虑O(n)O(n)O(n) 求了  
用STL存 每个数质因子 和 它的个数 如果mod k==0mod\, k == 0modk==0 对这个没有必要存 谁乘都是成立的

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 5;
    const int INF = 0x3f3f3f3f;
    typedef pair<int, int> pii;
    #define int long long
    int n, k;
    long long ans;
    
    vector<pii> tmp, tp;
    map<vector<pii>, int > cnt;
    
    int c[maxn], p[maxn];
    int m;
    
    void div(int n) {
        m = 0;
        for(int i = 2; i <= sqrt(n); i ++) {
            if(n % i == 0) {
                p[++ m] = i, c[m] = 0;
                while(n % i == 0) n /= i, c[m] ++, c[m] %= k;// 这少mod了 导致后面-c[i] + 2 * k 还是可能小于0 醉了 然后wa6.。。。。
            }
        }
        if(n > 1) p[++ m] = n, c[m] = 1;
        tmp.clear();
        tp.clear();
        for(int i = 1; i <= m; i ++) {
            if((k - c[i] + k) % k) tp.push_back(make_pair(p[i], (k - c[i] + k) % k));
            if(c[i] % k) tmp.push_back(make_pair(p[i], c[i] % k));
        }
        ans += 1ll * (cnt[tp]);
        cnt[tmp] ++;
    }
    
    signed main() {
        cin >> n >> k;
        for(int i = 1, x; i <= n; i ++) {
            cin >> x;
            div(x);
        }
        cout << ans << endl;
        return 0;
    }
    

