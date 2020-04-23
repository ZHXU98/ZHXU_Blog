## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_B_吃豆豆_【DP】

忘记把地图赋值成-inf了

wls在玩一个游戏。wlswlswls有一个nnn行mmm列的棋盘，对于第iii行第jjj列的格子，每过T[i][j]T[i][j]T[i][j]秒会在上面出现一个糖果，第一次糖果出现在第T[i][j]T[i][j]T[i][j]秒，糖果仅会在出现的那一秒存在，下一秒就会消失。假如wlswlswls第kkk秒在第iii行第jjj列的格子上，满足T[i][j]∣kT[i][j]
|
kT[i][j]∣k，则wlswlswls会得到一个糖果。假如当前wlswlswls在第iii行第jjj列的格子上，那么下一秒他可以选择往上下左右走一格或停在原地。现在请问wlswlswls从指定的SSS出发到达指定的TTT，并且在路上得到至少CCC个糖果最少需要多少时间？wlswlswls在SSS的初始时间为第000秒。

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 10000+25 ;
    
    int n,m,k;
    int sx,sy,ex,ey;
    int mp[15][15];
    int dp[15][15][maxn];
    
    int main() {
    	cin>>n>>m>>k;
    	for(int i=1;i<=n;i++) for(int j=1;j<=m;j++) cin>>mp[i][j];
    	cin>>sx>>sy>>ex>>ey;
    	
    	memset(dp,-0x3f,sizeof(dp));
    	dp[sx][sy][0]=0; 
    	
    	for(int t=1;t<maxn;t++){
    		for(int i=1;i<=n;i++){
    			for(int j=1;j<=m;j++){
    				 dp[i][j][t]=max(dp[i-1][j][t-1],max(dp[i][j+1][t-1],max(dp[i+1][j][t-1],max(dp[i][j-1][t-1],dp[i][j][t-1]))))+(t%mp[i][j]==0?1:0);
    			}
    		}
    	}
    	
    	for(int i=1;i<maxn;i++){
    		if(dp[ex][ey][i]>=k){
    			cout<<i<<endl;
    			break;
    		}
    	}
    	return 0;
    }

