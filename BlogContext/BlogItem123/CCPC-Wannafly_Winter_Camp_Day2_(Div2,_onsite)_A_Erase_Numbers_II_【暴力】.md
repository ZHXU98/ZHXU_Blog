## CCPC-Wannafly_Winter_Camp_Day2_(Div2,_onsite)_A_Erase_Numbers_II_【暴力】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408195039262.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
开ull。。。。。  
位置不变 直接n2 暴力找最大

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    typedef unsigned long long ull;
    const int maxn = 6*1e3+10;
    const int INF= 0x3f3f3f3f;
    const int mod = 1000000007;
    int n;
    int a[maxn];
    ull len[maxn];
    
    void init(){
    	for(int i=1;i<=n;i++){
    		int x=a[i];
    		len[i]=1;
    		while(x){
    			len[i]*=10;
    			x/=10;
    		}
    	}
    }
    
    int main(){
    	int t,cas=1;
    	cin>>t;
    	while(t--){
    		cin>>n;
    		for(int i=1;i<=n;i++) cin>>a[i];
    		init();
    		ull ans=0;
    		for(int i=1;i<=n;i++){
    			for(int j=i+1;j<=n;j++){
    				ans=max(ans,a[i]*len[j]+a[j]);
    			}
    		}
    		printf("Case #%d: %llu\n",cas++,ans);
    	}
    	return 0;
    } 
    

