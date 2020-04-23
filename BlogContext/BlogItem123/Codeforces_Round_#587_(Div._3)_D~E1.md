## Codeforces_Round_#587_(Div._3)_D~E1

还是思维不够啊 切的好慢 还手残  
<https://codeforces.com/contest/1216>

## D. Swords

这题 每个人拿的武器一样 而且还告诉你 一开始每堆都一样 而且 又一个堆没有碰过 最后告诉你 每堆省多少  
要求输出 最少人数 那样 每人拿武器一定最多  
武器最多肯定是 差的 gcd 人最少

  1. n∗x−sum=z∗yn*x-sum=z*yn∗x−sum=z∗y
  2. x=a[n]x=a[n]x=a[n]
  3. z=gcdz=gcdz=gcd

## E1. Numerical Sequence (easy version)

先暴力预处理了  
找到它在什么范围那 直接-到我们可以直接输出的范围就好  
字符串在一直重复 最多也加不过1e5次 num字符串长度 要开长点

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    const int maxn = 1e7 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef long long ll;
    typedef pair<int, int> P;
    int n, m, k, cas;
     
    int a[maxn];
    //n*x-sum=z*y
    //x=a[n]
    //z=gcd
    signed main() {
    	int n;
    	int sum = 0;
    	cin >> n;
    	for(int i = 1; i <= n; i ++) {
    		scanf("%d", &a[i]);
    		sum += a[i];
    	}
    	sort(a + 1, a + 1 + n);
    	int gcd = a[2] - a[1];
    	
    	for(int i = 3; i <= n; i ++) {
    		gcd = __gcd(gcd, a[i] - a[i - 1]);
    	}
    	int p = a[n];
    	int ans = (p * n - sum)/gcd;
    	cout << ans << " " << gcd << endl;
    	return 0;
    }
    
    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    const int maxn = 5e4 + 5;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    typedef long long ll;
    typedef pair<int, int> P;
    int n, m, k, cas;
     
    int len[maxn];
    int pc[maxn];
    int num[500000], t = 0;
    signed main() {
    	for(int i = 1; i < maxn; i ++) {
    		len[i] = len[i - 1] + log10(i) + 1;
    	}
    	pc[0] = 1;
    	for(int i = 1; i < maxn; i ++)
    		pc[i] = pc[i - 1] + len[i - 1];
    	for(int i = 1; i < maxn; i ++) {
    		int d = i, k = 0;
    		char s[15];
    		while(d) {
    			s[k ++] = d % 10;
    			d /= 10;
    		}
    		for(int j = k - 1; j >= 0; j --) num[++ t] = s[j];
    	}
    	int cas;
    	cin >> cas;
    	while(cas --) {
    		cin >> n;
    		for(int i = 1; i < maxn; i ++) {
    			if(pc[i] <= n && pc[i + 1] > n) {
    				cout << num[n - pc[i] + 1] << endl;
    				break;
    			}
    		}
    	}
    	return 0;
    }
    

