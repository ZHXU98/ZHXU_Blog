## Codeforces_1114C_(n阶乘在b进制下的末尾0数)

分解bbb的质因数∏i=1mpici{\prod_{i=1}^m{p_i^{c_i}}}∏i=1m​pici​​  
计算答案并除以cic_ici​,再取min

所以100！末尾有多少个零为：100/5+100/25=20+4=24  
那么1000！  
同理得： 1000/5+1000/25+1000/125=200+40+8=248

<https://codeforces.com/contest/1114/problem/C>

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 1e3 + 5;
    ll a[maxn], b[maxn];
    ll sum;
    ll n, k;
    ll ans = 0x3f3f3f3f3f3f3f3f, tmp;
    //	   999999999 999999999
    void fs(ll s) {
        ll i, j = 0;
        for(i = 2; i * i <= s; i++)
            if(s % i == 0) {
                ll cnt = 0;
                a[j] = i;
                while(s % i == 0) {
                    cnt++;
                    s /= i;
                }
                b[j++] = cnt;
            }
        if(s > 1) {
            a[j] = s;
            b[j++] = 1;
        }
        sum = j;
    }
    ll fb(ll x, ll y) { // 看作为因数有多少个 for我一般这么写
        ll res = 0;
        for(ll i = y; i <= x;) {
            res += x / i;
            x /= i;
        }
        return res;
    }
    
    ll fb1(ll x,ll y) { // 看作为因数有多少个
    	if (x<y)
    		return 0;  
    	else
    		return x/y+fb(x/y,y); 
    }
    
    int main() {
        cin >> n >> k;
        fs(k);
        for(int i = 0; i < sum; i++) {
            tmp = fb(n, a[i]);
            tmp /= b[i];
            if(ans > tmp)
                ans = tmp;
            else
                ans = ans;
        }
        printf("%lld\n", ans);
        return 0;
    }
    

