## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_I_起起落落_【DP】

DP  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408142110711.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_5,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_8,color_FFFFFF,t_60)  
样例1  
5  
4 2 3 1 5  
输出 1

序列 是3个数字的波浪折线  
dp[ i ]表示以 i 结尾的子序列个数 遍历每个位置 i 记录可以作为中心点的个数k  
j从i-1遍历之前处理过的答案  
如果a[ j ] < a [ i ]那么他可以作为 i 的一个中点k++ 如果a[ j ] > a[ i ]那么以 j 结尾的子序列都可以接上k和 i
作为一个新子序列 并且j k i也是一个新子序列 dp[ i ] += (dp[ j ] + 1 ) * k

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 2e3+10;
    const int mod = 1e9+7;
    int n,k;
    ll dp[maxn];
    int a[maxn];
    
    int main() {
    	cin>>n;
    	for(int i=1; i<=n; i++) cin>>a[i];
    	for(int i=3; i<=n; i++) {
    		k=0;
    		for(int j=i-1; j>=1; j--) {
    			if(a[j]>a[i]) dp[i]+=(dp[j]+1)*k%mod,dp[i]%=mod;
    			else k++;
    		}
    	}
    	ll ans=0;
    	for(int i=1;i<=n;i++) ans+=dp[i],ans%=mod;
    	cout<<ans<<endl;
    	return 0;
    }
    

