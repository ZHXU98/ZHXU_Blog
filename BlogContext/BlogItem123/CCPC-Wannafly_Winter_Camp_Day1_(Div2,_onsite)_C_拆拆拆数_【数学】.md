## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_C_拆拆拆数_【数学】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190203152107119.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
猜结论题 我数学不好 难以理解这为什么 如果AB不互质 那么就肯定当n==2 有解

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 3*1e5+5;
    const ll mod = 1e9+7; 
    
    ll gcd(ll a,ll b){
    	return b==0?a:gcd(b,a%b);
    }
    
    int main(){
       	int t;
       	ll n,m;
       	cin>>t;
       	while(t--){
       		cin>>n>>m;
       		if(gcd(n,m)==1){
       			cout<<1<<" "<<n<<" "<<m<<endl;
    		}else{
    			int f=1;
    			for(int i=2;i<20&&f;i++){
    				for(int j=2;j<20&&f;j++){
    					if(f&&gcd(i,j)==1&&gcd(n-i,m-j)==1){
    						cout<<2<<endl;
    						cout<<i<<" "<<j<<endl;
    						cout<<n-i<<" "<<m-j<<endl;
    						f=0;
    					}
    				}
    			}
    		}
       }
       	
        return 0;
    }
    

