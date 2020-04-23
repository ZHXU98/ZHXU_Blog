## 【逆向并查集+STLmap存图奇法】_Connections_in_Galaxy_War_ZOJ_-_3261

<http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=3261>

这题 感谢bo同学大力帮助。。。告诉我 奇技淫巧  
虽然思路蛮顺的 题也没有什么太坑的点(goupi 我现在补上这句话 2个月后)

题意：有n个星球编号为0—n-1;能量分别为p[i]；有m句话，每句话输入a，b表示星球a和星球b可以相通的；  
但是由于银河之战,破坏了一些通道  
接下来有Q句话：destroy a b代表ab之间的通道被破坏；  
query a代表求a可以向哪个星球求助，并输出编号，如果没有就输出-1；  
各个星球只像能量值比自己大的星球求助，而且是尽量找到最大能量值的星球求助。  
如果有多个能量值一样的星球可以求助，则找编号小的。  
思路:  
输入时记录每一条边  
记录每一个操作和销毁的边。  
输入结束后先用并查集加入所有没有被销毁的边  
然后再逆序操作记录结果，此时遇到 destroy 则加入销毁的边 ，遇到 query 直接查找即可。  
最终逆序输出结果。  
不同的测试数据有空行。

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <queue>
    #include <stack>
    #include <map>
    using namespace std;
    typedef unsigned long long ull;
    typedef long long ll;
    const int QMAXN=50000+5;
    const int VMAXN=10000+5;
    const int EMAXN=20000+5;
    const int mod=1e9+7;
    
    struct node{
    	int pre;
    	int val;
    }G[VMAXN];
    
    struct op{
    	int op,x,y;
    }cmd[QMAXN];
    
    struct links{
    	int x,y;
    }links[EMAXN];
    				// links 存一开始可以形成的图 之后再用map确定最终状态的图就好了
    map<pair<int,int>,int> mp;// 这简直了。。。第一次标1 破坏 标0 
                             //看其他题解还有用set的 orz
    
    stack<int> ans; //倒序找的答案 用这玩意输出就好
    
    int finds(int x){
    	return G[x].pre==x?x:G[x].pre=finds(G[x].pre);
    }
    
    void unions(int x,int y){
    	int tx=finds(x),ty=finds(y);
    	if(G[tx].val>G[ty].val) {
    		G[ty].pre=tx;
    	}
    	else if(G[tx].val<G[ty].val){
    		G[tx].pre=ty;	
    	}
    	else{
    		if(tx>ty) G[tx].pre=ty;
    		else G[ty].pre=tx;
    	}
    }
    
    char s[10];
    int n,m,val,sdsty,slt,sop;
    int st,ed;
    
    int main(){
    	int f=0;
    	while(~scanf("%d",&n)){
    		if(f) cout<<endl;
    		f=1;
    		for(int i=0;i<n;i++){
    			scanf("%d",&G[i].val);
    			G[i].pre=i;
    		}
    		scanf("%d",&m);
    		slt=m;
    		for(int i=0;i<m;i++){
    			scanf("%d %d",&st,&ed);
    			if(st>ed) swap(st,ed);
    			links[i].x=st;links[i].y=ed;
    			mp[make_pair(st,ed)]=1;
    		}
    		scanf("%d",&m);
    		sop=m-1;
    		for(int i=0;i<m;i++){
    			scanf("%s %d",s,&cmd[i].x);
    			if(s[0]=='q') cmd[i].op=1;
    			else {
    				scanf("%d",&cmd[i].y);
    				st=cmd[i].x;
    				ed=cmd[i].y;
    				if(st>ed) swap(st,ed);
    				mp[make_pair(st,ed)]=0;
    				cmd[i].op=2;
    			}
    		}
    		for(int i=0;i<slt;i++){
    			if(mp[make_pair(links[i].x,links[i].y)]==1){
    				unions(links[i].x,links[i].y);
    			}
    		}
    		
    		for(int i=sop;i>=0;i--){
    			if(cmd[i].op==1){
    				st=finds(cmd[i].x);
    				if(G[st].val>G[cmd[i].x].val) ans.push(st);// 最开始压错了 我以为找到父节点 输出1呢结果是父节点坐标
    				else ans.push(-1);
    			}
    			else{
    				unions(cmd[i].x,cmd[i].y);
    			}
    		}
    		while(!ans.empty()){
    			printf("%d\n",ans.top());
    			ans.pop();
    		}	
    	}
        return 0;
    }
    

