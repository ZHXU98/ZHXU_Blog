## 【数学】_CF_1174D_-_Ehab_and_the_Expected_XOR_Problem

codeforces  
[1174D - Ehab and the Expected XOR
Problem](https://codeforces.com/contest/1174/problem/D)

题意  
让你构造一个数列A 使他任何一个子段异或值不能是0 也不能是 x  
同时使他最长

这里我们考虑 优先构造一个数列A 的前缀异或和 B数列  
那么 A[ i ] = B[ i ] ^ B [ i - 1 ];

我们首先考虑 如果想要尽可能的长同时不出现 0 和 x 那样的话  
我们构造数列B 的时候就最好只选取 i ^ x 和 i 中的 仅一个 那样就保证了 我们异或的时候不会出现 x

由于我们 只能选 这异或里面的一半数据 所以 最后l最长 是 2^（n-1）

由于 B 里面相邻每位都是不同的 所以自然不会出现 0

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    const int maxn = 2e5 + 5;
    
    int n, x;
    
    bool vis[1 << 18];
    
    signed main() {
        fastio;
        cin >> n >> x;
        int pre = 0;
        vector<int> ans;
        vis[x] = 1; // x 不能出现
        ans.push_back(0);
        for(int i = 1; i < (1 << n); i ++) {
            if(vis[i])
                continue;
    
            vis[i] = vis[i ^ x] = 1; // 优先选取 成对出现的一个
            ans.push_back(i); // 现在构成 a的前缀异或和数组
        } // 那样 a[i] = b[i + 1] ^ b[i];
    
        cout << ans.size() - 1 << endl;
    
        for(int i = 1; i < ans.size(); i ++) {
            cout << (ans[i] ^ ans[i - 1]) << " ";
        }cout << endl;
        return 0;
    }
    

