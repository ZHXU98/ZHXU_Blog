## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_A_机器人_【模拟】

模拟+贪心 最多穿越2次 一次过去一次回来  
不管同侧异侧 找关键点得左右 >= 最远点  
同侧 双侧 和s的位置 讨论下

注释那里wa上天

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    #define fir first
    #define sec second
    const int maxn = 1e6+5;
    const int INF = 0x3f3f3f3f;
    
    pair<int,int> a[maxn];
    int b[maxn];
    
    int main() {
    	int n,r,m,k,s,x;
    	cin>>n>>r>>m>>k>>s;
    	for(int i=0; i<r; i++) cin>> a[i].fir>> a[i].sec;
    	for(int i=1; i<=m; i++) cin>>b[i];
    	b[0]=1, b[m+1]=n;
    	int al=s,bl=n,ar=s,br=1,flag=0;
    	sort(b,b+m+2);
    	for(int i=0; i<r; i++) {
    		if(a[i].sec) {
    			flag=1;
    			bl=min(bl,*(upper_bound(b,b+m+2,a[i].fir)-1));
    			br=max(br,*(lower_bound(b,b+m+2,a[i].fir)));
    		}
    	}
    	b[m+2]=s;sort(b,b+m+3);// 就是这里 wa飞 一开始赋值al=ar=s 以为可以 然后wa飞了
    	for(int i=0; i<r; i++) {
    		if(!a[i].sec) {
    			al=min(al,*(upper_bound(b,b+m+3,a[i].fir)-1));
    			ar=max(ar,*(lower_bound(b,b+m+3,a[i].fir)));
    		}
    	}
    	al=min(al,bl),ar=max(ar,br);
    	if(flag) {
    		cout<<2*k+(br-bl)+(ar-al)+(bl-al)+(ar-br)+(max(0,s-ar)+max(0,al-s))*2<<endl;
    	} else {
    		cout<<(max(0,s-ar)+max(0,al-s)+ar-al)*2<<endl;
    	}
    	return 0;
    }
    

