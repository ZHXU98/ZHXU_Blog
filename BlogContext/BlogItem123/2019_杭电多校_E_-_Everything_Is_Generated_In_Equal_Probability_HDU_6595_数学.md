## 2019_杭电多校_E_-_Everything_Is_Generated_In_Equal_Probability_HDU_6595_数学

给了你一个程序  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190730084116806.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
程序 S1 将传入的 数组 返回一个随机子序列(不一定连续)  
程序 S2 算这个数组 逆序对数量  
程序 S3 算这个数组 经过S1 之后 用S2算逆序对数量

到这里 我们知道了 这个程序是在算 一个序列 包括它子序列 随机排列 最后 逆序对期望值

首先 我们知道 一个长度为n的 它随机排列的逆序对期望 C(n, 2) 可以产生多少逆序对 每个 逆序对的存在概率是/2  
所以 是C（n, 2）/ 2  
知道这个后面 我们就好算 算 它子序列的逆序对期望了 ![在这里插入图片描述](https://img-
blog.csdnimg.cn/20190730085322270.gif)  
移向之后 我们就算出 i 子序列的个数了 在 + 上 它自身期望  
答案 问的是 n 之内 的 期望 所以 我们 sum (f 1 ~ n) / n 就是答案  
n^2 打表处理

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 3005;
    const int mod = 998244353;
    int n;
    int c[maxn][maxn], mi[maxn];
    int f[maxn], ans[maxn];
    
    int q_mod(int a, int b) {
        int res = 1;
        for(; b; b >>= 1) {
            if(b & 1) res = 1ll * res * a % mod;
            a = 1ll * a * a % mod;
        }
        return res;
    }
    
    void init(){
        mi[0] = 1;
        for(int i = 1; i < maxn; i++) {
            mi[i] = (mi[i - 1] << 1) % mod;
            c[i][i] = 1, c[i][0] =1;
            for(int j = 1; j < i; j++)
                c[i][j] = (c[i-1][j] + c[i-1][j-1]) % mod;
        }
        
        f[0] = 0, f[1] = 0;
        for(int i = 2; i < maxn; i++) {
            int fz = 0;
            for(int j = 0; j < i; j++)
                fz = (1ll * fz + (1ll * c[i][j] * f[j]) % mod) % mod;
            fz = (1ll * fz + 1ll * c[i][2] * mi[i-1]) % mod;
            f[i] = (1ll * fz * q_mod(mi[i] - 1 , mod - 2)) % mod;
           // cout << f[i] << " " << fz << endl;
        }
        
        for(int i = 1; i < maxn; i ++) {
            int res = 0;
            for(int j = 1; j <= i; j ++) 
                res = (1ll * res + f[j]) % mod;
            res = (1ll * res * q_mod(i, mod - 2) % mod);
            ans[i] = res;
        } 
        
    }
    
    signed main(){
        init();
        while(cin >> n) {
            cout << ans[n] << endl;
        }
        return 0;
    }
    

