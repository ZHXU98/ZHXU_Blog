## 【逆元】The_Wall_(medium)_CodeForces_-_690D2_&&_Sum_HDU_-_4704

费马小定理  
题一

    
    
    #include <cstdio>
    #include <iostream>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    typedef long long ll;
    const int maxn = 1000005;
    const int mod = 1000003;
    
    ll f[maxn + 10];
    
    void init() {//阶乘
        f[0] = 1;
        for (int i = 1; i <= maxn; i++) {
            f[i] = (long long)f[i - 1] * i%mod;
        }
    }
    
    ll pow_mod(ll a, ll b) {
        ll ans = 1; a %= mod;
        while (b) {
            if (b & 1) {
                ans = ans * a % mod;
                b--;
            }
            b >>= 1;
            a = a * a % mod;
        }
        return ans;
    }
    
    int C(int m, int n) {//求逆元
        return f[n] * pow_mod(f[m], mod - 2) % mod*pow_mod(f[n - m], mod - 2) % mod;
    }
    
    int main() {
        init();
        int n, m;
        while (cin >> n >> m) {
    
            cout << C(m, n + m) - 1 << endl;
        }
        return 0;
    }

题二 pow（2,n-1） n巨大

    
    
    #include <iostream>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    typedef long long ll;
    const int maxn = 1000005;
    const int mod = 1000000007 ;
    
    
    ll pow_mod( ll a, ll b ){
          ll ans = 1 ; a %= mod;
        while ( b ) {
            if ( b & 1 ){
                ans = ans * a % mod;  
                b--;
            }
                b >>= 1;  
                a = a * a % mod;
        }
            return ans;
    }
    
    int main(){
        int n,m;
    //  cout<<123456%7<<endl;
        string s;
        while(cin>>s){
            ll sum=0;
            for(int i=0;i<s.size();i++){
                sum=(sum*10+s[i]-48)%(mod-1);//ps1
            }
            cout<<pow_mod(2,sum-1)%mod<<endl;
        }
        return 0;
    } 
    

ps1：  
粗：  
mod-1 显然是 2^(p-1)%p=1以及求2^(n-1)情况下  
所以 (n-1-k(p-1)）；  
稍微细点  
当p是一个素数并且a和p互质时，a^(p-1) %p≡1。  
那么在这道题里a=2，p=mod。显然满足费马小定理。再根据同余模定理，  
当n>p时：设m=n-p。那么an-1=am+p-1=ap-1*am。因而an-1%p=am+p-1%p=((ap-1%p
)*(am%p))%p=1*am%p。

