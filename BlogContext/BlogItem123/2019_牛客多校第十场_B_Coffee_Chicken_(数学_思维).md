## 2019_牛客多校第十场_B_Coffee_Chicken_(数学_思维)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190819143242941.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
类似 斐波那契数列  
这个字符在coffe 还是 chicken  
我们只需要 每次减去 dp[n - 2] 判断它在那个串中 能减去就意味是每个串重新换了减去 ad–  
不然-=2

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const ll maxn=1e3+10;
    ll dp[maxn];
    
    int main() {
    	dp[1] = 6, dp[2] = 7;
    	for(ll i=3; i<=60; i++) dp[i]=dp[i-1]+dp[i-2];
    	ll t;
    	cin>>t;
    	string x[4]= {"","*COFFEE","*CHICKEN"};
    	while(t--) {
    		ll n,k;
    		cin>>n>>k;
    		string ans="";
    		while(n>60) n-=2;
    		for(ll i=0; i<10; i++) {
    			ll m=k+i,ad=n;
    			if(m > dp[ad]) break;
    			while(1) {
    				if(ad<=2) break;
    				if(m>dp[ad-2]) m-=dp[ad-2],ad--;
    				else ad=ad-2;
    			}
    			ans=ans+x[ad][m];
    		}
    		cout<<ans<<endl;
    	}
    	return 0;
    }
    

