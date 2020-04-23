## 【BFS】Ball_Aizu_-_0033_原题正解_DFS

RT ORZ  
硬生生 写成bfs

    
    
    #include <iostream>
    #include <cstdio>
    #include <algorithm>
    #include <cstring>
    #include <stack>
    #include <queue>
    using namespace std;
    
    int a[15];
    //int b[15];
    int n;
    
    struct node {
    	int cnt;
    	int pos1,pos2;
    	int an[15];
    	int bn[15];
    }ball,tmp,t;
    
    bool bfs(){
    	queue<node> q;
    	ball.an[0]=a[0];
    	ball.pos1=0;
    	ball.pos2=0;
    	ball.cnt=0;
    	q.push(ball);
    	while(!q.empty()){
    		tmp=q.front();
    		int f=1;
    		if(tmp.an[tmp.pos1]<a[tmp.cnt+1]){
    			tmp.an[tmp.pos1+1]=a[tmp.cnt+1];
    			tmp.pos1++;
    			tmp.cnt++;
    			q.push(tmp);
    			f=0;
    		}
    		tmp=q.front();
    		if(tmp.pos2==0||tmp.bn[tmp.pos2]<a[tmp.cnt+1]){
    			tmp.bn[tmp.pos2+1]=a[tmp.cnt+1];
    			tmp.pos2++,tmp.cnt++;
    			q.push(tmp);
    			f=0;
    		}
    		/*
    		for(int i=0;i<q.front().pos1;i++){
    			cout<<q.front().an[i]<<"  ";
    		}cout<<endl;
    		for(int i=0;i<q.front().pos2;i++){
    			cout<<q.front().bn[i]<<"  ";
    		}cout<<endl;
    		cout<<"   "<<tmp.cnt<<endl;
    		q.pop();
    	//	if(f) return 0;
    	*/
    	q.pop();
    		if(tmp.cnt==9) return 1;
    	}
    	return 0;
    } 
    
    int main(){
    	while(cin>>n){
    		while(n--){
    			ball.pos1=0,ball.pos2=0,ball.cnt=0,ball.bn[0]=0;
    			for(int i=0;i<10;i++) cin>>a[i];
    			if(bfs()) cout<<"YES\n";
    			else cout<<"NO\n";
    		}
    	//	
    	}
    	return 0;
    }
    
    

无力orz

