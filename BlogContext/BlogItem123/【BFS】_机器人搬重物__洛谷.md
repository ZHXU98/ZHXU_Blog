## 【BFS】_机器人搬重物__洛谷

本文章仅纪念自己 把200多行代码压到80多行  
<https://www.luogu.org/problemnew/show/P1126>  
vj上也有 csu1975 说真的 直接输出12 在csu上就过了 洛谷数据多点  
23333 太可怕了 一年后看到给改了改  
改版后

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 50+5;
    const int cx[]= {0,0,1,-1};
    const int cy[]= {1,-1,0,0};
    const int xx[]= {-1,0,-1,0};
    const int yy[]= {-1,-1,0,0};
    const int walkx[]= {1,0,-1,0};
    const int walky[]= {0,-1,0,1};
    const char tof[]="SWNE";
    int n,m,x,pos,sx,sy,ex,ey,flag;
    int mp[maxn][maxn];
    bool vis[maxn][maxn][26];
    
    bool check(int x,int y,char s) {
    	if(x==1||y==1) return 1;
    	if(x<1||y<1||x>n||y>m) return 1;
    	if(vis[x][y][s-'A']) return 1;
    	for(int i=0; i<4; i++)
    		if(mp[x+xx[i]][y+yy[i]]==1) return 1;
    	return 0;
    }
    
    int repos(char s) {
    	for(int i=0; i<4; i++) if(tof[i]==s) return i;
    }
    
    struct node {
    	int sp;
    	char ms;
    	int x,y;
    } st,tp,tmp;
    
    void bfs() {
    	queue<node> que;
    	que.push(st);
    	vis[st.x][st.y][st.ms-'A']=1;
    	while(!que.empty()) {
    		tmp=que.front();
    		que.pop();
    		if(tmp.x==ex&&tmp.y==ey) {
    			printf("%d\n",tmp.sp);
    			return ;
    		}
    		for(int i=0; i<4; i++) {
    			pos=repos(tmp.ms);
    			tp.ms=tmp.ms;
    			if(i==0) {
    				tp.ms=tof[(pos+1)%4];
    				if(!check(tmp.x,tmp.y,tp.ms)) {
    					vis[tmp.x][tmp.y][tp.ms-'A']=1;
    					que.push(node {tmp.sp+1,tp.ms,tmp.x,tmp.y});
    				}
    				tp.ms=tof[(pos+3)%4];
    				if(!check(tmp.x,tmp.y,tp.ms)) {
    					vis[tmp.x][tmp.y][tp.ms-'A']=1;
    					que.push(node {tmp.sp+1,tp.ms,tmp.x,tmp.y});
    				}
    			} else  {
    				tp.x=tmp.x+walkx[pos]*(4-i);
    				tp.y=tmp.y+walky[pos]*(4-i);
    				flag=1;
    				for(int j=3-i; j>=0; j--)
    					if(check(tp.x+(-1)*j*walkx[pos],tp.y+(-1)*j*walky[pos],tp.ms)) flag=0;
    				if(flag) vis[tp.x][tp.y][tp.ms-'A']=1,que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
    			}
    		}
    	}
    	printf("-1\n");
    	return ;
    }
    
    int main() {
    	scanf("%d %d",&n,&m);
    	for(int i=1; i<=n; i++) for(int j=1; j<=m; j++)
    			scanf("%d",&mp[i][j]);
    	scanf("%d %d %d %d %c",&sx,&sy,&ex,&ey,&st.ms);
    	st=node {0,st.ms,sx+1,sy+1};
    	ex++,ey++,n++,m++;
    	bfs();
    	return 0;
    }
    

改版前

    
    
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
    const int maxn = 50+5;
    const int cx[]= {0,0,1,-1};
    const int cy[]= {1,-1,0,0};
    
    const int xx[]={-1,0,-1,0};
    const int yy[]={-1,-1,0,0};
    
    int n,m,x;
    int mp[maxn][maxn];
    bool vis[maxn][maxn][26];
    int sx,sy,ex,ey;
    
    bool check(int x,int y,char s) {
        if(x==1||y==1) return 1;
        if(x<1||y<1||x>n||y>m) return 1;
        if(vis[x][y][s-'A']) return 1;
        for(int i=0; i<4; i++) {
            if(mp[x+xx[i]][y+yy[i]]==-1) return 1;
        }
        return 0;
    }
    
    struct node {
        int sp;
        char ms;
        int x,y;
    } st,tp,tmp;
    
    void bfs() {
        queue<node> que;
        que.push(st);
        vis[st.x][st.y][st.ms-'A']=1;
        while(!que.empty()) {
            tmp=que.front();
            que.pop();
        //	cout<<tmp.x<<"   "<<tmp.y<<"   "<<tmp.ms<<"   "<<tmp.sp<<endl;
            if(tmp.x==ex&&tmp.y==ey) {
                printf("%d\n",tmp.sp);
                return ;
            }
            int nx,ny;
            for(int i=0; i<5; i++) {
                if(i==0) {
                    if(tmp.ms=='S') tp.ms='W';
                    if(tmp.ms=='W') tp.ms='N';
                    if(tmp.ms=='N') tp.ms='E';
                    if(tmp.ms=='E') tp.ms='S';
                    if(check(tmp.x,tmp.y,tp.ms)) continue;
                    vis[tmp.x][tmp.y][tp.ms-'A']=1;
                    que.push(node {tmp.sp+1,tp.ms,tmp.x,tmp.y});
                }
                if(i==1) {
                    if(tmp.ms=='S') tp.ms='E';
                    if(tmp.ms=='W') tp.ms='S';
                    if(tmp.ms=='N') tp.ms='W';
                    if(tmp.ms=='E') tp.ms='N';
                    if(check(tmp.x,tmp.y,tp.ms)) continue;
                    vis[tmp.x][tmp.y][tp.ms-'A']=1;
                    que.push(node {tmp.sp+1,tp.ms,tmp.x,tmp.y});
                }
                if(i==4) {
                    if(tmp.ms=='N') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y;
                        tp.x=tmp.x-1;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='S') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y;
                        tp.x=tmp.x+1;				
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='W') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y-1;
                        tp.x=tmp.x;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='E') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y+1;
                        tp.x=tmp.x;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                }
                if(i==3) {
                    if(tmp.ms=='N') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y;
                        tp.x=tmp.x-2;
                        if(check(tp.x+1,tp.y,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='S') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y;
                        tp.x=tmp.x+2;
                        if(check(tp.x-1,tp.y,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='W') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y-2;
                        tp.x=tmp.x;
                        if(check(tp.x,tp.y+1,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='E') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y+2;
                        tp.x=tmp.x;
                        if(check(tp.x,tp.y-1,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                }
                if(i==2) {
                    if(tmp.ms=='N') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y;
                        tp.x=tmp.x-3;
                        if(check(tp.x+2,tp.y,tp.ms)) continue;
                        if(check(tp.x+1,tp.y,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='S') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y;
                        tp.x=tmp.x+3;
                        if(check(tp.x-2,tp.y,tp.ms)) continue;
                        if(check(tp.x-1,tp.y,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='W') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y-3;
                        tp.x=tmp.x;
                        if(check(tp.x,tp.y+2,tp.ms)) continue;
                        if(check(tp.x,tp.y+1,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                    if(tmp.ms=='E') {
                        tp.ms=tmp.ms;
                        tp.y=tmp.y+3;
                        tp.x=tmp.x;
                        if(check(tp.x,tp.y-2,tp.ms)) continue;
                        if(check(tp.x,tp.y-1,tp.ms)) continue;
                        if(check(tp.x,tp.y,tp.ms)) continue;
                        vis[tp.x][tp.y][tp.ms-'A']=1;
                        que.push(node {tmp.sp+1,tp.ms,tp.x,tp.y});
                    }
                }
            }
        }
        printf("-1\n");
        return ;
    }
    
    int main() {
        scanf("%d %d",&n,&m);
        memset(mp,-1,sizeof(mp));
        for(int i=1; i<=n; i++) {
            for(int j=1; j<=m; j++) {
                scanf("%d",&x);
                if(x==1) continue;
                else mp[i][j]=x;
            }
        }
        n++,m++;
        scanf("%d %d %d %d %c",&sx,&sy,&ex,&ey,&st.ms);
        st=node{0,st.ms,sx+1,sy+1};
        ex++,ey++;
        bfs();
        return 0;
    }
    

