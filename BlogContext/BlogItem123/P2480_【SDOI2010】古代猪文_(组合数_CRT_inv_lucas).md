## P2480_【SDOI2010】古代猪文_(组合数_CRT_inv_lucas)

思路  
观察题目，不难发现，我们需要在给定GG,NN的情况下，求

G∑i∣NCNimod&ThinSpace;&ThinSpace; 999911659G^{\sum_{i|N}C_N^i} \mod \
999911659G∑i∣N​CNi​mod 999911659

的值。所以，我们只需要求出GG的幂的值就可以进行计算。

然而数据范围告诉我们，先求 ∑i∣NCNi​\sum_{i|N}C_N^i​∑i∣N​CNi​​
再进行计算是会爆空间的。所以，我们利用费马小定理的一个推论。

ab≡ab mod (p−1)(modp) (a!=p)a^b\equiv a^{b \ mod \ (p-1)}\pmod{p} \ \ \ \ \ \
(a != p)ab≡ab mod (p−1)(modp) (a!=p)

于是，我们考虑对组合数取模。发现p-1，即9999116589不是质数，将它分解，得2 _3_ 4679*35617
。所以，我们先利用Lucas定理对组合数分别取模，然后用中国剩余定理求出GG的幂，最后用快速幂求得答案。

    
    
    #include <bits/stdc++.h>
    #define debug(x, str) cout << (str) << " = [ << : " << (x) << " ]" << endl;
    #define fastio ios::sync_with_stdio(0),cin.tie(0);
    #define fir first
    #define sec second
    using namespace std;
    typedef pair<int, int> pii;
    typedef long long ll;
    const int maxn = 1e5 + 5;
    const int p = 999911659;
    
    ll n, g;
    ll prime[] = {2, 3, 4679, 35617};
    ll X[5], inv[maxn], fac[maxn];
    
    ll qmod(ll a, ll b, ll mod) {
    	ll res = 1;
    	for(; b; b >>= 1) {
    		if(b & 1) res = res * a % mod;
    		a = a * a % mod;
    	}
    	return res;
    }
    
    void init(ll p) {
    	fac[0] = 1;
    	for(int i = 1; i <= p; i ++) fac[i] = fac[i - 1] * i % p;
    	inv[p - 1] = p - 1;
    	for(int i = p - 2; i >= 0; i --) inv[i] = inv[i + 1] * (i + 1) % p; 
    }
    
    ll C(ll n,ll m,ll p) {//Lucas theorem
        if(n<m) return 0;
        if(n<p && m<p) return fac[n]*inv[m]%p*inv[n-m]%p;
        return C(n%p,m%p,p)*C(n/p,m/p,p)%p;
    }
    
    long long exgcd(long long a, long long b, long long &x, long long &y) {
    	if (b == 0) {
    		x = 1, y = 0;
    		return a;
    	}
    	int d = exgcd(b, a % b, y, x);
    	y -= a / b * x;
    	return d;
    }
    
    ll crt(ll mod) {
    	ll ans = 0;
    	for(int i = 0; i < 4; i ++) {
    		ll d, x, y;
    		exgcd(mod/prime[i], prime[i], x, y);
    		ll cach = (x % mod * (mod / prime[i]) % mod + mod) % mod;
    		ans += cach * X[i] % mod;
    		ans %= mod;
    	}
    	return ans;
    }
    
    int sol(ll p) {
    	init(p);
    	ll limit = sqrt(n), ans = 0;
    	for(int i = 1; i <= limit; i ++) {
    		if(n % i) continue;
    		ans = (ans + C(n, i, p)) % p;
    		if(n/i == i) continue;
    		ans = (ans + C(n, n/i, p)) % p;
    	}
    	return ans;
    }
    
    int main() {
    	cin >> n >> g;
    	if(!(g%p)) cout << 0 << endl;
    	else {
    		for(int i = 0; i < 4; i ++) X[i] = sol(prime[i]);
    		cout << qmod(g, crt(p - 1), p);
    	}
    	return 0;
    }
    

