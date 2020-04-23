## 【DFS】Pusher_HDU_-_2821_枚举地图上的可行点

<http://acm.hdu.edu.cn/showproblem.php?pid=2821>  
这题的题意是真的难看懂

给若干个可重叠的格子，起点可以任意选择，可以往四个方向一直走到有格子的地方为止，每碰一次就消掉一个格子，并且把剩下的移到下一个位置
如果能够位置有格子就累加。  
注意：①起点不能有格子。②必须要隔一个位置才能碰。③碰的格子在边上时，剩下的格子不能移出矩形范围。④碰的最后部分格子在边上时，如果有剩余，初始位置就放错了
另找

    
    
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
    const int maxn = 30 ;
    const int mdir = 1<<11;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    const int cx[]={0,0,-1,1};
    const int cy[]={1,-1,0,0};
    
    int n,m,t,allblock;
    char path[5000];
    int mp[maxn][maxn];
    char str[maxn];
    char dir[5]="RLUD";
    
    bool check(int x,int y){return (x>=0&&y>=0&&x<n&&y<m);}
    
    bool dfs(int x,int y,int pos){
    	if(pos==allblock){// 碰完格子便是搜完
    		path[pos]=0;
    		return 1;
    	}
    	int nx,ny,sum;
    	for(int i=0;i<4;i++){
    		nx=x+cx[i],ny=cy[i]+y;
    		if(!check(nx,ny)||mp[nx][ny]) continue;//检查 是不是隔一个位置
    		while(check(nx,ny)&&!mp[nx][ny]) nx+=cx[i],ny+=cy[i];// 一直模拟这个球跑动
    		if(!check(nx,ny)) continue;// 确定没有越界
    		if(!check(nx+cx[i],ny+cy[i])) continue; // 确定 碰的那个块不在边 这个注释也也能过 迷 不过变成31ms  不注释 15ms
    		sum=mp[nx][ny];
    		mp[nx+cx[i]][ny+cy[i]]+=sum-1;
    		mp[nx][ny]=0;// 改变状态
    		path[pos]=dir[i];
    		if(dfs(nx,ny,pos+1)) return 1;
    		mp[nx+cx[i]][ny+cy[i]]-=sum-1;
    		mp[nx][ny]=sum;// 恢复状态
    		path[pos]=0;
    	}
    	return 0;
    }
    
    int main(){
    	while(~scanf("%d%d",&m,&n)){
    		allblock=0;
    		for(int i=0;i<n;i++){
    			scanf("%s",str);
    			for(int j=0;j<m;j++){
    				if(str[j]>='a'&&str[j]<='z') mp[i][j]=str[j]-'a'+1;
    				else mp[i][j]=0;
    				allblock+=mp[i][j];
    			}
    		}
    		int flag=1;
    		for(int i=0;i<n&&flag;i++){
    			for(int j=0;j<m&&flag;j++){
    				if(!mp[i][j]&&dfs(i,j,0)){// 枚举可行每个点 搜爆找解
    					printf("%d\n%d\n%s\n",i,j,path);
    					flag=0;
    				}
    			}
    		}
    	}
    	return 0;
    }
    

