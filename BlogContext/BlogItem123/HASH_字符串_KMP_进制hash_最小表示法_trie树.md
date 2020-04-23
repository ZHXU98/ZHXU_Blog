## HASH_字符串_KMP_进制hash_最小表示法_trie树

**雪花雪花**

> 有N片雪花，每片雪花由六个角组成，每个角都有长度。  
>  第i片雪花六个角的长度从某个角开始顺时针依次记为ai,1,ai,2,…,ai,6。  
>  因为雪花的形状是封闭的环形，所以从任何一个角开始顺时针或逆时针往后记录长度，得到的六元组都代表形状相同的雪花。  
>  例如ai,1,ai,2,…,ai,6和ai,2,ai,3,…,ai,6，ai,1就是形状相同的雪花。  
>  ai,1,ai,2,…,ai,6和ai,6,ai,5,…,ai,1也是形状相同的雪花。  
>  我们称两片雪花形状相同，当且仅当它们各自从某一角开始顺时针或逆时针记录长度，能得到两个相同的六元组。  
>  求这N片雪花中是否存在两片形状相同的雪花。  
>  输入格式  
>  第一行输入一个整数N，代表雪花的数量。  
>  接下来N行，每行描述一片雪花。  
>  每行包含6个整数，分别代表雪花的六个角的长度（这六个数即为从雪花的随机一个角顺时针或逆时针记录长度得到）。  
>  同行数值之间，用空格隔开。  
>  输出格式  
>  如果不存在两片形状相同的雪花，则输出：  
>  No two snowflakes are alike.  
>  如果存在两片形状相同的雪花，则输出：  
>  Twin snowflakes found.  
>  数据范围  
>  1≤n≤100000,  
>  0≤ai,j<10000000  
>  输入样例：  
>  2  
>  1 2 3 4 5 6  
>  4 3 2 1 6 5  
>  输出样例：  
>  Twin snowflakes found.

虽然说是hash表过的题 但是这里我写一个 最小表示过的代码

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 1e6 + 5 ;
    const double pi = acos(-1.0);
    const int mod = 991 ;
    int s[300],ss[300];
    int ge(int s[]) {
    	int len = 6;
    	for(int i=1; i<=len; i++) s[i+len]=s[i];
    	int i=1,j=2,k;
    	while(i<=len&&j<=len) {
    		for(k=0; k<len&&s[i+k]==s[j+k]; k++);
    		if(k==len) break;
    		if(s[k+i]>s[j+k]) {
    			i=i+1+k;
    			if(i==j) i++;
    		} else {
    			j=j+1+k;
    			if(j==i) j++;
    		}
    	}
    	int pos=min(i,j);
    	return pos;
    }
    
    set<int> mp;
    
    signed main() {
    	int n;
    	cin>>n;
    	while(n--) {
    		for(int i=1; i<=6; i++) {
    			cin>>s[i];
    		}
    		for(int i=1; i<=6; i++) ss[7-i]=s[i];
    		int hash = 0;
    		int k1=ge(s);
    		for(int i=0; i<6; i++) {
    			hash=hash*mod+s[k1+i];
    		}
    		int k2=ge(ss);
    		int hash2=0;
    		for(int i=0; i<6; i++) {
    			hash2=hash2*mod+ss[k2+i];
    		}
    		if(mp.find(min(hash,hash2))!=mp.end()) {
    			cout<<"Twin snowflakes found."<<endl;
    			return 0;
    		}
    		mp.insert(min(hash,hash2));
    	}
    	cout<<"No two snowflakes are alike."<<endl;
    	return 0;
    }
    

**进制hash**  
快速找 字符串内部连续串是否存在 一样

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int unsigned long long
    
    const int maxn = 1e6 + 5 ;
    const double pi = acos(-1.0);
    const int mod = 131;
    
    char str[maxn];
    int p[maxn],f[maxn];
    int n;
    
    signed main() {
    	scanf("%s",str+1);
    	int len = strlen(str+1);
    	p[0]=1;
    	for(int i=1; i<=len; i++) {
    		f[i]=f[i-1]*131+(str[i]-'a'+1);
    		p[i]=p[i-1]*mod;
    	}
    	cin>>n;
    	while(n--) {
    		int l,r,x,y;
    		cin>>x>>y>>l>>r;
    		if(f[y]-f[x-1]*p[y-x+1]==f[r]-f[l-1]*p[r-l+1]) {
    			cout<<"Yes"<<endl;
    		} else cout<<"No"<<endl;
    	}
    	return 0;
    }
    

**马拉车** **KMP** 板子不讲

## Trie

**前缀统计**

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 1e6+5;
    
    int trie[maxn][30],tot=1;
    int ens[maxn];
    
    void ins(char str[]){
    	int len=strlen(str),p=1;
    	for(int k=0;k<len;k++){
    		int ch=str[k]-'a';
    		if(trie[p][ch]==0) trie[p][ch]=++tot;
    		p=trie[p][ch]; 
    	}
    	ens[p]++;
    }
    
    int serach(char str[]){
    	int res=0;
    	int len=strlen(str),p=1;
    	for(int k=0;k<len;k++){
    		int ch=str[k]-'a';
    		if(trie[p][ch]==0) break;
    		p=trie[p][ch];
    		if(ens[p]) res+=ens[p];
    	}
    	return res;
    }
    
    signed main() {
    	fastio;
    	int n,m;
    	cin>>n>>m;
    	char s[maxn];
    	for(int i=1;i<=n;i++){
    		cin>>s;
    		ins(s);
    	}
    //	cin>>m;
    	while(m--){
    		int ans=0;
    		cin>>s;
    		ans+=serach(s);
    		cout<<ans<<endl;
    	}
    	return 0;
    }
    

**最大异或对**

> 在给定的N个整数A1，A2……AN中选出两个进行xor（异或）运算，得到的结果最大是多少？  
>  输入格式  
>  第一行输入一个整数N。  
>  第二行输入N个整数A1～AN。  
>  输出格式  
>  输出一个整数表示答案。  
>  数据范围  
>  1≤N≤105,  
>  0≤Ai<231  
>  输入样例：  
>  3  
>  1 2 3  
>  输出样例：  
>  3

第一次完全没有想到这跟trie有关系 orz  
PS 这里一定要高位往低位取贪心 先拿高位 比低位贪

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 4e6+5;
    
    int trie[maxn][5],tot=1;
    
    void ins(int s){
    	int p=1;
    	for(int k=0;k<=31;k++){
    	//	int ch = ((1<<k)&s?1:0);
    		int ch=s>>k&1;
    		if(trie[p][ch]==0) trie[p][ch]=++tot;
    		p=trie[p][ch]; 
    	}
    }
    
    int serach(int s){
    	int res=0;
    	int p=1;
    	for(int k=0;k<=31;k++){
    	//	int ch=((1<<k)&s);
    		int ch=s>>k&1;
    		if(trie[p][!ch]){
    			p=trie[p][!ch];
    			res+=(1<<k);
    		}
    		else{
    			p=trie[p][ch];
    		}
    	}
    	return res;
    }
    
    int a[maxn];
    
    signed main() {
    	fastio;
    	int n,m;
    	cin>>n;
    	int s;
    	for(int i=1;i<=n;i++){
    		cin>>a[i];
    		ins(a[i]);
    	}
    	int ans=0;
    	for(int i=1;i<=n;i++){
    		ans=max(ans,serach(a[i]));
    	}
    	cout<<ans<<endl;
    	return 0;
    }
    

**最长异或值路径**  
树上 2点之间路径的最大异或值  
无根树转有根树 记录每个点到跟路径异或值 重复部分刚好是2次 被消掉  
如上找最大异或值就好

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    //#define int long long
    
    const int maxn = 4e6+5;
    
    int trie[maxn][5],tot=1;
    
    int cnt,head[maxn];
    int nxt[maxn<<1],to[maxn<<1],dis[maxn<<1];
    
    void ade(int a,int b,int c) {
    	to[++cnt]=b;
    	dis[cnt]=c;
    	nxt[cnt]=head[a];
    	head[a]=cnt;
    }
    int fa[maxn];
    
    void dfs(int x,int pre,int sum) {
    	fa[x]=sum;
    	for(int i=head[x]; i; i=nxt[i]) {
    		if(to[i]==pre) continue;	
    		dfs(to[i],x,sum^dis[i]);
    	}	
    }
    
    void ins(int s) {
    	int p=1;
    	for(int k=31; k>=0; k--) {
    		int ch=s>>k&1;
    		if(trie[p][ch]==0) trie[p][ch]=++tot;
    		p=trie[p][ch];
    	}
    }
    
    int serach(int s) {
    	int res=0;
    	int p=1;
    	for(int k=31; k>=0; k--) {
    		int ch=s>>k&1;
    		if(trie[p][!ch]) {
    			p=trie[p][!ch];
    			res+=(1<<k);
    		} else {
    			p=trie[p][ch];
    		}
    	}
    	return res;
    }
    
    int a[maxn];
    
    signed main() {
    	fastio;
    	int n,m;
    	cin>>n;
    	int st,ed,d;
    	for(int i=1;i<n;i++){
    		cin>>st>>ed>>d;
    		st++,ed++;
    		ade(st,ed,d),ade(ed,st,d);
    	}
    	dfs(1,-1,0);
    	for(int i=1;i<=n;i++){
    		ins(fa[i]);
    	}
    	int ans=0;
    	for(int i=1;i<=n;i++){
    		ans=max(ans,serach(fa[i]));
    	}
    	cout<<ans<<endl;
    	return 0;
    }
    

