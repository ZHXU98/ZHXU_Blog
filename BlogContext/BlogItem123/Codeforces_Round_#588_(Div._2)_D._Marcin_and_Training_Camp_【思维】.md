## Codeforces_Round_#588_(Div._2)_D._Marcin_and_Training_Camp_【思维】

思维题 要是更快的看懂题就好了  
<https://codeforces.com/contest/1230/problem/D>

## D. Marcin and Training Camp

题意  
给了很多人 每个人会不同算法 只要他会 就必须要求其他人会 不然不组 (60个算法 用二进制表达)  
第二行就是这个人的价值 求最大价值 在可以组的情况下

唯一可以组的情况就是 这个技能 可以和另外的认对应 而且其他认技能 只能是他子集  
这样我们暴力把所有可以的 同时标记了他们的子集就好了

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef  long long ll;
    const int MAXN = 1e4 + 10;
    
    ll n, a[MAXN], b[MAXN];
    map<ll, int> cnt;
    bool vis[MAXN];
    
    int main() {
        cin >> n;
        for(int i = 1; i <= n; ++ i) {
            cin >> a[i];
            cnt[a[i]] ++;
        }
        for(int i = 1; i <= n; ++ i)
            cin >> b[i];
        for(int i = 1; i <= n; ++ i)
            if(cnt[a[i]] > 1)
                for(int j = 1; j <= n; ++ j)
                    if((a[i] | a[j]) == a[i])
                        vis[j] = 1;
        ll ans = 0;
        for(int i = 1; i <= n; ++ i)
            if(vis[i]) ans += b[i];
        cout << ans << endl;
        return 0;
    }
    

