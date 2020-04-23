## 【并查集】_食物链_POJ_-_1182_&&_A_Bug's_Life_HDU_-_1829

<https://blog.csdn.net/c0de4fun/article/details/7318642/>  
特别感谢 这篇博客的博主  
虽然之前看过其他一些博客 但是一直没有理解  
但在这篇博文学会了  
[poj.org/problem?id=1182](http://poj.org/problem?id=1182)  
食物链 POJ - 1182

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    #include <map>
    #include <queue>
    #include <set>
    #include <stack>
    #include <list> 
    using namespace std;
    typedef long long ll;
    
    const int maxn = 50000+5 ;
    const int maxv = 100*1000+5;
    const int mod = 1000000 ;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int n,m,d,x,y;
    
    struct node{
    	int pre;
    	int ral;
    }an[maxn];
    
    void init(){for(int i=0;i<=n;i++){an[i].pre=i;an[i].ral=0;}}
    
    int find(int x){
    	if(x==an[x].pre) return x;
    	int tmp=an[x].pre;
    	an[x].pre=find(an[x].pre);
    	an[x].ral=(an[x].ral+an[tmp].ral)%3;
    	return an[x].pre;
    }
    
    int main(){
    	while(scanf("%d %d",&n,&m)!=EOF){
    		int ans=0;
    		init();
    		for(int i=0;i<m;i++){
    			scanf("%d %d %d",&d,&x,&y);
    			if(x>n||y>n) ans++;
    			else if(d==2&&x==y) ans++;
    			else{
    				int a=find(x);
    				int b=find(y);
    				if(a!=b){
    					an[b].pre=a;
    					an[b].ral=((3-an[y].ral)+(d-1)+an[x].ral)%3;
    				}
    				else{
    					if(d==1) if((an[y].ral+3-an[x].ral)%3!=0) ans++;
    					if(d==2) if((an[y].ral+3-an[x].ral)%3!=1) ans++;
    				}
    			}
    		}
    		printf("%d\n",ans);
    	}
    	
    	return 0;
    }
    

这道题 还有个白书上的写法

    
    
    // 这代码是我6月前按照白书 的思虑慢慢写的
    // 能这样维护3个集的关系 真的还不如上面那个好理解
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    //typedef long long ll;
    const int maxn = 50000<<2;
    
    int pre[maxn];//x3
    //int test1[50005],test2[50005];
    
    int finds(int a) {  
        return a == pre[a] ? a : pre[a] = finds(pre[a]);  
    }  
    
    void unions(int x,int y){
    	int x1=finds(x),y1=finds(y);
    	if(x1!=y1){
    		pre[x1]=y1;
    	}
    	return ;
    }
    
    bool same(int x,int y){
    	return finds(x)==finds(y);
    }
    
    void init(){
    	for(int i=0;i<maxn;i++){
    		pre[i]=i;
    	}
    }
    
    int main(){
    	int n,t,cmd,x,y;
    	cin>>n>>t;
    		init();
    		int ans=0;
    		
    		for(int i=0;i<t;i++){
    			scanf("%d%d%d",&cmd,&x,&y);
    			if(x<1||y<1||x>n||y>n||(x==y&&cmd==2)) {
    				ans++;
    		//	cout<<i<<endl;
    				continue;
    			}
    			
    			if(cmd==1){
    				if(same(n+x,y)||same(x+n*2,y)){
    					ans++;
    			//	cout<<i<<endl;
    					continue;
    				}
    				else{
    					unions(x,y);unions(x+n,y+n);unions(x+2*n,y+2*n);
    				}
    			}
    			else {
    				if(same(x,y)||same(x+2*n,y)){
    					ans++;
    			//	cout<<i<<endl;
    					continue;
    				}
    				else {
    					unions(x,y+n*2);unions(x+n,y);unions(x+2*n,y+n);
    				}
    			}
    		//	cout<<i+1<<endl;
    		}
    	//	cout<<"end"<<endl;
    		cout<<ans<<endl;
    	return 0;
    }
    

A Bug’s Life HDU - 1829  
<http://acm.hdu.edu.cn/showproblem.php?pid=1829>  
1 2 3 是3个集合  
找其中 1->2 2->3 内的元素有这样的关系 但如果还可以有元素让 3->1 便有同性恋 有点难解释

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    #include <map>
    #include <queue>
    #include <set>
    #include <stack>
    #include <list> 
    using namespace std;
    typedef long long ll;
    
    const int maxn = 50000+5 ;
    const int maxv = 100*1000+5;
    const int mod = 1000000 ;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int n,m,d,x,y;
    
    struct node{
    	int pre;
    	int ral;
    }an[maxn];
    
    void init(){for(int i=0;i<=n;i++){an[i].pre=i;an[i].ral=0;}}
    
    int find(int x){
    	if(x==an[x].pre) return x;
    	int tmp=an[x].pre;
    	an[x].pre=find(an[x].pre);
    	an[x].ral=(an[x].ral+an[tmp].ral)%2;
    	return an[x].pre;
    }
    
    int main(){
    	int t;
    	cin>>t;
    	for(int cas=1;cas<=t;cas++){
    		scanf("%d %d",&n,&m);
    		int ans=0;
    		init();
    		for(int i=0;i<m;i++){
    			scanf("%d %d",&x,&y);
    			int a=find(x),b=find(y);
    			if(a!=b){
    				an[b].pre=a;
    				an[b].ral=(2-an[y].ral+1-an[x].ral)%2;
    			}else{
    				if((an[y].ral+2-an[x].ral)%2!=1) ans++;
    			}
    		}
    		printf("Scenario #%d:\n",cas);
    		if(ans!=0)printf("Suspicious bugs found!\n\n");
    		else printf("No suspicious bugs found!\n\n");
    	}
    	return 0;
    }
    

