## 2019n牛客多校第八场_B_Beauty_Values_(DP_or_找规律)

f(i)=f(i-1)+i-(vis[a[i]]?vis[a[i]]:0)  
公式怎么出来的呢?考虑 由1-i 如果 第i个数字出现过,那么他对前一个出现过的i维护的区间是没有贡献的,只有vis[i]-i有贡献.  
如果没出现过肯定 是前面维护的 都+1:

有人可以看出 是总ans不断减 现在数据之前出现过所有同样数据(不止当前这个)出现的位置 和

    
    
    #include<bits/stdc++.h>
    using namespace std;
    #define int long long
    
    int a[100005];
    int k[100005];
    signed main(){
    	int n;
    	scanf("%lld",&n);
    	int ans=0;
    	for(int i=1;i<=n;i++){
    		scanf("%lld",&a[i]);
    		ans=ans+i*(i+1)/2;
    	}
    	int sum=0,num=0;
    	for(int i=1;i<=n;i++){
    		if(k[a[i]]==0) k[a[i]]=i;
    		else {
    			sum+=k[a[i]];
    			k[a[i]]=i;
    		}
    		ans-=sum;
    	}
    	printf("%lld\n",ans);
    }
    
    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef  long long ll;
    const int maxn=1e5+8;
    ll n,m,k;
    ll num[maxn];
    int vis[maxn];
    ll dp[maxn];
    int main()
    {
       ll sum=0;
       scanf("%lld",&n);
       for(int i=1;i<=n;i++) scanf("%lld",&num[i]);
       for(int i=1;i<=n;i++)
       {
            dp[i]=dp[i-1]+i-(vis[num[i]]?vis[num[i]]:0);
            vis[num[i]]=i;
       }
       for(int i=1;i<=n;i++)
         sum+=dp[i];
       printf("%lld\n",sum);
       return 0;
    }
    

