## CCPC-Wannafly_Winter_Camp_Day3_(Div2,_onsite)

A 二十四点* (暴力)  
orz 被队友带躺 A题我是暴力不出这么快的  
虽然只有2个测试点 一个 6 一个 10

    
    
    #include <iostream>
    #include <cstdio>
    #include <string>
    #include <cmath>
    #include <algorithm>
    #include <map>
    #include <vector>
    #include <queue>
    using namespace std;
    typedef long long ll;
    typedef unsigned long long ull;
    
    ll ans=0,n;
    
    ll yz(ll zu[],ll s) {	//data shengchende shuzu,s wei changdu
    
    	if(s<2) return 0;
    
    	for(int i=0; i<s; i++) {
    		for(int j=i+1; j<s; j++) {
    			ll a=zu[i]+zu[j];
    			if(a==24) return 1;
    			else {
    				ll ne[15],lz=0;
    				for(int k=0; k<s; k++) {
    					if(k==i||k==j) continue;
    					ne[lz]=zu[k];
    					lz++;
    				}
    				ne[lz]=a;
    				if(yz(ne,s-1)==1) return 1;
    			}
    
    			ll b=abs(zu[i]-zu[j]);
    			if(b==24) return 1;
    			else if(b==0) ;
    			else {
    				ll ne[15],lz=0;
    				for(int k=0; k<s; k++) {
    					if(k==i||k==j) continue;
    					ne[lz]=zu[k];
    					lz++;
    				}
    				ne[lz]=b;
    				if(yz(ne,s-1)==1) return 1;
    			}
    
    			ll c=zu[i]*zu[j];
    			if(c==24) return 1;
    			else {
    				ll ne[15],lz=0;
    				for(int k=0; k<s; k++) {
    					if(k==i||k==j) continue;
    					ne[lz]=zu[k];
    					lz++;
    				}
    				ne[lz]=c;
    				if(yz(ne,s-1)==1) return 1;
    			}
    
    			ll d=-1;
    			if(zu[i]>zu[j]&&zu[i]%zu[j]==0) d=zu[i]/zu[j];
    			else if(zu[i]<zu[j]&&zu[j]%zu[i]==0) d=zu[j]/zu[i];
    			if(d==-1) continue;
    			if(d==24) return 1;
    			else {
    				ll ne[15],lz=0;
    				for(int k=0; k<s; k++) {
    					if(k==i||k==j) continue;
    					ne[lz]=zu[k];
    					lz++;
    				}
    				ne[lz]=d;
    				if(yz(ne,s-1)==1) return 1;
    
    			}
    		}
    	}
    	return 0;
    }
    
    void zxl(ll data[],ll zu[],ll s,ll x) {	//data shengchende shuzu,s wei changdu
    	if(s>=n||x>=n) return;
    
    	ll ne[15]= {0};
    
    	for(int i=0; i<s; i++) {
    		ne[i]=zu[i];
    	}
    
    	for(int i=x; i<n; i++) {
    		ne[s]=data[i];
    
    		if(yz(ne,s+1)!=0) {
    			ans++;
    		}
    		zxl(data,ne,s+1,i+1);
    	}
    }
    
    
    int main() {
    	ans=0;
    	ll data[15]= {0},zu[15]= {2,3,4};
    	scanf("%lld",&n);
    	for(int i=0; i<n; i++) scanf("%lld",&data[i]);
    	zxl(data,zu,0,0);
    
    	printf("%lld",ans);
    	return 0;
    }
    

G 排列 (模拟） 对不起 其实我连题都读不懂。。。。。。。。。。。。orz  
p：原数组  
Ap：前缀数组  
q：p中第i小的前缀的长度  
q 可以还原 Ap 数组 但是 Ap必须是非递增的  
所以 样例  
q 5 3 4 1 2  
Ap 3 0 2 0 1 出现 某个数据位置小于前一个 当0 它必然是 最后cnt++;  
p 3 4 2 5 1

    
    
    #include <bits/stdc++.h> 
    using namespace std;
    typedef long long ll;
    typedef unsigned long long ull;
    
    const int maxn = 1e5+5; 
    int a[maxn];
    int cnt,n,m;
    int ans[maxn];
    
    int main() {
    	cin>>n;
    	for(int i=1;i<=n;i++){
    		cin>>a[i];
    		if(a[i]<a[i-1]||i==1) ans[a[i]]=++cnt;
    	} 
    	for(int i=1;i<=n;i++){
    		if(i!=1) cout<<" ";
    		if(ans[i]) cout<<ans[i];
    		else cout<<++cnt;
    	}puts("");
    	return 0;
    }
    

