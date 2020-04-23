## Codeforces_Round_#575_(Div._3)_D2._RGB_Substring_(hard_version)

还是思维题

## D2. RGB Substring (hard version)

<https://codeforces.com/contest/1196/problem/D2>

给你一个 RGB 几个字符组成的序列 让他变成RGBRGBRGB的子串  
要有k长度的子串 问 最少改变几个实现  
D1 其实 可以直接 3个k(不同字符开始) 一起暴力跑k长度 找最小值  
D2就不可以了 这样我们考虑 这个改变字符串个数 求一个前缀和  
我们求3个不同字符开头的就好 然后for一边找最小值(sum[i] - sum[i - k]) 这样复杂度就下来了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 5;
    const int INF = 0x3f3f3f3f;
    int n, m, k, cas;
    int ans;
     
    char str[5] = "RGB";
    char s[maxn];
    int sum[maxn];
     
    signed main() {
        ios::sync_with_stdio(false);
        cin.tie(0);
        cin >> cas;
        while(cas --) {
            cin >> n >> m;
            cin >> (s + 1);
            int len = strlen(s + 1);
            int ans = INF;
            //cout << len << endl;
            for(int p = 0; p < 3; p ++) {
                memset(sum, 0, (n + 5) * sizeof(int));
                int mi = INF;
                for(int i = 1; i <= len; i ++) {
                    sum[i] = (s[i] != str[(p + i) % 3]) + sum[i - 1];
                }
                for(int i = len; i >= m; i --) {
                    mi = min(sum[i] - sum[i - m], mi);
                }
                ans = min(ans, mi);
            }
            cout << ans << endl;
        }
        return 0;
    }
    

