## 【bfs】_Meteor_Shower_POJ_-_3669_预处理地图

<https://vjudge.net/problem/12685/origin>

1.玩家从原点(0,0)出发  
2.有流星在一定时间里往地图上砸。砸到以后，这个点以及上下左右共5个点就再也不能行走了(砸以前可以走)  
3.玩家每个单位时间必须要上下左右走一格，只能在第一象限中移动  
4.找到最短的行走距离，保证这个点是安全的，即所有流星都不会砸到这个点  
// 不一定要离开300x300的范围 让我楞了好一会  
// 重复打击的点 更新最小值就好  
预处理啊预处理 怎么就楞住了呢 这么水的题 楞住了// 翻译完 一直在楞 不知道楞什么呢

    
    
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
    #define fir first
    #define sec second
    const int maxn = 305 ;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    const int cx[]={1,0,-1,0};
    const int cy[]={0,1,0,-1};
    
    int n,m,x,y,t;
    int mp[maxn][maxn];
    bool vis[maxn][maxn];
    int ans;
    
    bool check(int x,int y){return (x>=0&&y>=0);}
    
    struct point{
    	pair<int,int> P;
    	int sp;	
    }tmp,tp;
    
    int bfs(){
    	queue<point> que;
    	que.push(point{make_pair(0,0),0});
    	vis[0][0]=1;
    	while(!que.empty()){
    		tmp=que.front();que.pop();
    		for(int i=0;i<4;i++){
    			int nx=tmp.P.fir,ny=tmp.P.sec;
    			nx+=cx[i],ny+=cy[i];
    			if(check(nx,ny)&&!vis[nx][ny]){
    				if(mp[nx][ny]==INF) return tmp.sp+1;
    				if(mp[nx][ny]>tmp.sp+1){
    					vis[nx][ny]=1;
    					que.push(point{make_pair(nx,ny),tmp.sp+1});
    				}
    			}
    		}
    	}
    	return -1;
    }
    
    int main(){
    	memset(mp,INF,sizeof(mp));
    	memset(vis,0,sizeof(vis));
    	scanf("%d",&n);
    	for(int i=0;i<n;i++){
    		scanf("%d %d %d",&x,&y,&t);	mp[x][y]=min(mp[x][y],t);
    		for(int j=0;j<4;j++) if(check(x+cx[j],y+cy[j])) mp[x+cx[j]][y+cy[j]]=min(mp[x+cx[j]][y+cy[j]],t);
    	} 
    	printf("%d\n",bfs());
    	return 0;
    }
    

