## 【板子_kruskal】_onscription_POJ_-_3723

地址 poj.org/problem?id=3723

最大生成树 更新一个板子

题意：征用所有人需要(n+m)*10000元。男孩与女孩之间有联系的，征兵所需费用 -d元

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    #include <map>
    #include <queue>
    #include <set>
    using namespace std;
    typedef long long ll;
    
    const int maxn_e = 50000+5;
    const int maxn_n = 20000+5;
    const int maxn = 1e3+5;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    struct node{
    	int u,v,dis;
        node(){};
        node(int a,int b,int c){u=a,v=b,dis=c;}
        bool operator<(const node &a) const {return dis > a.dis;}
    }edge[maxn_e];
    
    int pre[maxn_n];
    
    void init(){for(int i=0;i<=maxn_n;i++) pre[i]=i;}
    int finds(int x){return pre[x]!=x?pre[x]=finds(pre[x]):x;}
    void unions(int x,int y){x=finds(x);y=finds(y);x!=y?pre[x]=y:1;}
    
    int kruskal(int n){
    	init(); 
    	sort(edge,edge+n);
    	node e;
    	int res=0;
    	for(int i=0;i<n;i++){
    		e=edge[i];
    		if(finds(e.u)!=finds(e.v)){
    			unions(e.u,e.v);
    			res+=e.dis;
    		}
    	}
    	return res;
    }
    
    int main(){
    	int t,q,wo,man,st,ed,dis;
    	scanf("%d",&t);
    	while(t--){
    		scanf("%d %d %d",&man,&wo,&q);
    		for(int i=0;i<q;i++){
    			scanf("%d %d %d",&st,&ed,&dis);
    			edge[i]=node(st,ed+man,dis);
    		}
    		int ans=(man+wo)*(1e4);
    		printf("%d\n",ans-kruskal(q));
    	}
    	return 0; 
    }

