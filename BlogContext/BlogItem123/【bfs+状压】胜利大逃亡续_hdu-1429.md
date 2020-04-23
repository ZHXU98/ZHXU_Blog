## 【bfs+状压】胜利大逃亡续_hdu-1429

<http://acm.hdu.edu.cn/showproblem.php?pid=1429>

典型的状压搜索；  
在普通的搜索基础上,利用二进制的特性记录钥匙与门, 二进制的每一位代表一把钥匙；  
比如说拿到了2号钥匙  
那么原有的00000变为了00010,当到大了对应的二号门的时候,利用位运算00010来&上1<<2也就是00010就是非0,  
如果不是二号门的时候  
那么00010&上1<<x都是0,所以此时其它门都进不去,  
如果再拿到了三号钥匙加上之后变成了00110,代表二号三号钥匙都拿到了,  
然后到了三号门的  
时候00110&(1<<3)00100或者到了二号门的时候00110&(1<<2)00010都是非0的,到其他门都是为0的  
这样就很好的解决了钥匙的记录问题,这里有10把钥匙,所以数组要大于2的10次方

    
    
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
    const int maxn = 25;
    const int mdir = 1<<11;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    const int cx[]={1,-1,0,0};
    const int cy[]={0,0,1,-1};
    
    int n,m,t;
    char ch,mp[maxn][maxn];
    bool vis[maxn][maxn][mdir];// 25*25*（2^11） 625*2048 第一次分开写看起来感觉蛮大的实际上 ...
    //二进制 拿钥匙后的新图 也算是简单的状态压缩了
    
    struct point{
    	pair<int,int> P;
    	int sp,dir;
    }st,tmp,tp;
    
    int bfs(){
    	queue<point> que;
    	que.push(st);
    	vis[st.P.fir][st.P.sec][st.dir]=1;
    	while(!que.empty()){
    		tmp=que.front();que.pop();
    		if(mp[tmp.P.first][tmp.P.second]=='^') return tmp.sp;
    		int nx,ny,dir;
    		for(int i=0;i<4;i++){
    			dir=tmp.dir;
    			nx=tmp.P.first+cx[i],ny=tmp.P.second+cy[i];
    			if(nx<1||ny<1||nx>n||ny>m||vis[nx][ny][dir]) continue;
    			if(mp[nx][ny]=='*') continue;
    			// dir 当前钥匙状态 加法即可
    			if(mp[nx][ny]>='a'&&mp[nx][ny]<='j'){
    				int key=mp[nx][ny]-'a';
    				if((dir&(1<<key))==0) dir+=(1<<key);
    				if(!vis[nx][ny][dir]){
    					vis[nx][ny][dir]=1;
    					que.push(point{make_pair(nx,ny),tmp.sp+1,dir});
    				}
    			}
    			// 看 1 的位置和 钥匙是否对应
    			else if(mp[nx][ny]>='A'&&mp[nx][ny]<='J'){
    				int key=mp[nx][ny]-'A';
    				if(!(dir&(1<<key))) continue;
    				if(!vis[nx][ny][dir]){
    					vis[nx][ny][dir]=1;
    					que.push(point{make_pair(nx,ny),tmp.sp+1,dir});
    				}
    			}
    			
    			else{
    				if(!vis[nx][ny][dir]){
    					vis[nx][ny][dir]=1;
    					que.push(point{make_pair(nx,ny),tmp.sp+1,dir});
    				}
    			}
    		}
    	}
    	return -1;
    }
    
    int main(){
    	while(scanf("%d %d %d",&n,&m,&t)!=EOF){
    		memset(vis,0,sizeof(vis));
    		getchar();
    		for(int i=1;i<=n;i++){
    			for(int j=1;j<=m;j++){
    				scanf(" %c",&ch);
    				mp[i][j]=ch;
    				if(ch=='@'){
    					st.P.first=i,st.P.sec=j;
    					st.sp=0,st.dir=0;
    				}
    			}
    		}
    		int ans=bfs();
    		printf("%d\n",ans<t?ans:-1);
    	}
    	return 0;
    }
    

