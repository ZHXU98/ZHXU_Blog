## 【math_逆元】Alternating_Sum_CodeForces_-_964C

<http://codeforces.com/problemset/problem/964/C>

给 n,a,b,k;  
实现一个求和  
题意：求 (0~n)∑i=si^a^(n−i) *bi  
(0~n)∑i=si*a^(n−i)* bi by 109+9  
s[i]为+1或-1。  
题解：可证：每k项的和相差(b/a)^k。等比数列求和加逆元。  
看出来等比 花了点时间  
另外注意Q==1 以及 一不下心吧负数整出来这难为人情况  
通常+mod 在%mod 处理

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <queue>
    #include <stack>
    #include <map>
    using namespace std;
    typedef long long ll;
    
    const int maxn=1e5+10;
    const ll mod=1e9+9;// 第一次写的1e9+7 然后一直少2 我也醉了
    
    ll pow_mod(ll a,ll b){
         ll res=1;
         while(b){
            if(b&1) res=res*a%mod;
            a=a*a%mod;
            b>>=1;
         }
         return res%mod;
    }
    
    int main(){
    //  int n;
        string s;
        ll n,a,b,k;
        while(cin>>n>>a>>b>>k){
            cin>>s;
            ll v;
            ll a1=0;
            for(int i=0;i<k;i++){
                v=1;
                if(s[i]=='-') v=-1;
                a1+=((v*pow_mod(a,n-i)%mod*pow_mod(b,i)%mod));
                a1%=mod// 首项当然可以为负数
            }
    
        //  cout<<a1<<endl;i
            ll num=(n+1)/k;
    
            ll Q1=((pow_mod(b,k))+mod)%mod;
            ll Q2=(pow_mod(pow_mod(a,k),mod-2)+mod)%mod;
            ll Q=Q1*Q2%mod;
            if(Q==1){// Q=1 单独判断 wa了一发 
                cout<<(a1*num%mod+mod)%mod<<endl;
                continue;
            }
        //  cout<<Q<<endl;
            ll ans=(a1*(1-pow_mod(Q,num)%mod+mod)%mod)*(pow_mod(1-Q,mod-2)%mod+mod)%mod;
            if(ans<0) ans+=mod;
            cout<<ans<<endl;
        }
        return 0;
    }

