## 【DFS】_Curling_2.0_POJ_-_3009

<https://vjudge.net/problem/15201/origin>

然后图上有不能到的点（墙），起点扔一个球，球只能砸到墙才能停止，但是砸到墙上之后，这个墙就没了，最多可以砸10次，扔出界就算输，能不能把这个球从起点扔到终点。
注意，要是该点紧挨着就是一个墙，那就不能往这个墙的方向上扔。  
一开始想用广度优先搜索的，但是因为要每次撞完之后墙就消失了，所以感觉还是深度优先级写起来方便一些，要使用bfs的话，有可能到一个点用相同的步数但是有不同的扔法，这就导致了两种办法消失的墙不一样。总是实现起来是够麻烦的
然后 超时了  
这地对深度有一个限制 显然DFS 不怕炸了 不过我还是写了发BFS 果然系数太大 tle了

猛然回头看着题 限制让我想起了迭代加深 听起来蛮高大上 实际上用起来很方便

dfs AC代码

    
    
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
    
    const int maxn = 25 ;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    const int cx[]={1,0,-1,0};
    const int cy[]={0,1,0,-1};
    
    int n,m,x;
    int mp[maxn][maxn];
    bool vis[maxn][maxn];
    int ans;
    
    struct point{
    	pair<int,int> P;
    	int sp;	
    }st,ed;
    
    bool check(int x,int y){
    	return (x==ed.P.first)&&(y==ed.P.second);
    }
    
    void dfs(int x, int y, int deep) {
    	if (deep >= 10 || deep>=ans) return ;
    	for (int i = 0;i < 4;++i) {
    		int nx = x, ny = y;
    		while (mp[nx + cx[i]][ny + cy[i]] == 0){
    			nx = nx + cx[i], ny = ny + cy[i];
    			if (check(nx,ny)){
    				ans=min(ans,deep+1);return;
    			}
    		} // 确定不直接碰墙
    		if ((mp[nx + cx[i]][ny + cy[i]] == -1) || (nx == x && ny == y)) continue;
    		mp[nx + cx[i]][ny + cy[i]] = 0;// 改变
    		dfs(nx, ny, deep + 1); 
    		mp[nx + cx[i]][ny + cy[i]] = 1; // 回溯
    	}
    	return ;
    }
    
    int main(){
    	while(scanf("%d %d",&m,&n)!=EOF&&m+n){
    		ans=INF;
    		memset(mp,-1,sizeof(mp));
    		for(int i=1;i<=n;i++) for(int j=1;j<=m;j++){
    			scanf("%d",&x);
    			if(x==0) mp[i][j]=0;
    			else if(x==1) mp[i][j]=1;
    			else if(x==2){mp[i][j]=0; st.P.first=i;st.P.second=j;}
    			else{mp[i][j]=0;ed.P.first=i;ed.P.second=j;} 
    		}
    		dfs(st.P.first,st.P.second,0);
    		printf("%d\n",ans!=INF?ans:-1);
    		
    	}
    	return 0;
    }

最开始写的bfs代码 系数太大  
先别吐槽goto 我改完goto 准备交的时候 poj 炸到现在都没有好orz 等 poj好了 我在交发试一试；  
几天后补上 炸内存了

    
    
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
    
    const int maxn = 25 ;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int n,m,x;
    
    struct point{
    	pair<int,int> P;
    	int sp;
    	int mp[maxn][maxn];
    	bool vis[maxn][maxn];
    	void init(){
    		memset(mp,0,sizeof(mp));
    		memset(vis,0,sizeof(vis));
    		sp=0;
    	}
    	void operator = (point &a){
    		this->P=a.P;
    		this->sp=a.sp;
    		for(int i=0;i<=n;i++) for(int j=0;j<=m;j++) 
    			this->mp[i][j]=a.mp[i][j],this->vis[i][j]=a.vis[i][j];
    	}
    	point(){};
    	point(pair<int,int> _p,int _sp,int _mp[maxn][maxn],bool _vis[maxn][maxn]){
    		P=_p;sp=_sp;
    		for(int i=0;i<=n;i++) for(int j=0;j<=m;j++) 
    			mp[i][j]=_mp[i][j],vis[i][j]=_vis[i][j];
    	}
    	
    }st,ed,tmp,tp;
    
    bool check(int x,int y){
    	return (x==ed.P.first)&&(y==ed.P.second);
    }
    
    int bfs(){
    	int res=-1;
    	queue<point> que;
    	que.push(st);
    	int nx,ny;
    	while(!que.empty()){
    		tp=que.front();que.pop();
    		tmp=tp;
    		nx=tp.P.first;ny=tp.P.second;
    		tmp.vis[nx][ny]=1;
    		for(int i=0;i<4;i++){
    			nx=tp.P.first;ny=tp.P.second;
    			tmp=tp;
    		//	cout<<"********"<<nx<<"     "<<ny<<"       "<<tmp.sp<<endl;
    			if(i==0){
    				if(tmp.mp[nx-1][ny]==1) continue;
    				while(nx!=0&&!tmp.mp[nx-1][ny]){
    					nx--;
    					if(check(nx,ny)){res=tmp.sp+1;goto end;}
    				}
    				if(nx==0) continue;
    				tmp.mp[nx-1][ny]=0;
    			}else if(i==1){
    				if(tmp.mp[nx+1][ny]==1) continue;
    				while(nx!=n&&!tmp.mp[nx+1][ny]){
    					nx++;
    					if(check(nx,ny)){res=tmp.sp+1;goto end;}
    				} 
    				if(nx==n) continue;
    				tmp.mp[nx+1][ny]=0;
    			}else if(i==2){
    				if(tmp.mp[nx][ny-1]==1) continue;
    				while(ny!=0&&!tmp.mp[nx][ny-1]){
    					ny--;
    					if(check(nx,ny)){res=tmp.sp+1;goto end;}
    				} 
    				if(ny==0) continue;
    				tmp.mp[nx][ny-1]=0;
    			}else{
    				if(tmp.mp[nx][ny+1]==1) continue;
    				while(ny!=m&&!tmp.mp[nx][ny+1]){
    					ny++;
    					if(check(nx,ny)){res=tmp.sp+1;goto end;}
    				} 
    				if(ny==m) continue;
    				tmp.mp[nx][ny+1]=0;
    			}
    		//	cout<<"   "<<nx<<"    "<<ny<<"   "<<tmp.sp<<endl; 
    			if(!tmp.vis[nx][ny]&&tmp.sp+1<=10){
    				que.push(point(make_pair(nx,ny),tmp.sp+1,tmp.mp,tmp.vis));
    			}
    		}
    	}
    	end:
    	return res<=10?res:-1;
    } 
    
    int main(){
    	while(scanf("%d %d",&m,&n)!=EOF&&m+n){
    		st.init();
    		for(int i=1;i<=n;i++) for(int j=1;j<=m;j++){
    			scanf("%d",&x);
    			if(x==0) st.mp[i][j]=0;
    			else if(x==1) st.mp[i][j]=1;
    			else if(x==2){st.mp[i][j]=0; st.P.first=i;st.P.second=j;}
    			else{st.mp[i][j]=0;ed.P.first=i;ed.P.second=j;} 
    		}
    		printf("%d\n",bfs());
    	}
    	return 0;
    }

