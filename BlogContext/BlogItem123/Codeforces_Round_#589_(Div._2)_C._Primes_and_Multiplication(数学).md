## Codeforces_Round_#589_(Div._2)_C._Primes_and_Multiplication(数学)

## C. Primes and Multiplication

[https://codeforces.com/contest/1228/problem/C](https://codeforces.com/contest/1228/problem/D)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930185113354.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
看懂g函数就好搞了  
就是再问你 45 里面3作为质因子 出现次数是多少

我们考虑f 无非就是分解质因数 最多20个  
然后 1 到 n 这个质因数出现了几次 套阶乘的分解质因子就好  
然后 注意 阶乘质因子就不要乘了 连long long 都炸

    
    
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;
    const int INF = 0x3f3f3f3f;
    const int maxn = 2e5 + 5;
    const int mod = 1e9 + 7;
    int cas;
    int n, k;
    int x;
    int p[maxn], m;
    int c[maxn];
    
    void divide(int n) {
        m = 0;
        int len = sqrt(n);
        for(int i = 2; i <= len; i ++) {
            if(n % i == 0) {
                p[++ m] = i, c[m] = 0;
                while(n % i == 0) {
                    n /= i;
                    c[m] ++;
                }
            }
        }
        if(n > 1) p[++ m] = n, c[m] = 1;
    }
    
    int pow_mod(int a, int b) {
        int res = 1;
        for(; b; b >>= 1) {
            if(b & 1) res = res * a % mod;
            a = a * a % mod;
        }
        return res;
    }
    
    int g(int n, int m) {
        int res = 0;
        for(long long j = m; j <= n; j){
            res += (1ll * n / j);
            n /= j;
        }
        return pow_mod(m, res);
    }
    
    signed main() {
       // cout << log(1e9) << endl;
        cin >> x >> n;
        divide(x);
        long long ans = 1;
        for(int i = 1; i <= m; i ++) {
            long long tmp = 1;
            tmp = g(n, p[i]);
            ans = ans * tmp % mod;
        }
        cout << ans << endl;
        return 0;
    }
    

