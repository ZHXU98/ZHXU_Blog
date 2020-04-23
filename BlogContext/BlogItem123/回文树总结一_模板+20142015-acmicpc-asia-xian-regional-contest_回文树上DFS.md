## 回文树总结一_模板+20142015-acmicpc-asia-xian-regional-contest_回文树上DFS

## 回文串 HYSBZ - 3676

模板  
<https://vjudge.net/problem/HYSBZ-3676#author=0>

    
    
    #include <bits/stdc++.h>
    using namespace std; 
    const int maxn = 300005;// n(空间复杂度o(n*ALP)),实际开n即可
    const int ALP = 26;
     
    struct PAM{ // 每个节点代表一个回文串
        int nxt[maxn][ALP]; // nxt指针,参照Trie树
        int fail[maxn]; // fail失配后缀链接
        int cnt[maxn]; // 此回文串出现个数
        int num[maxn];
        int len[maxn]; // 回文串长度
        int s[maxn]; // 存放添加的字符
        int last; //指向上一个字符所在的节点，方便下一次add
        int n; // 已添加字符个数
        int p; // 节点个数
        
        int newnode(int w){ // 初始化节点，w=长度
            for(int i=0;i<ALP;i++)
                nxt[p][i] = 0;
            cnt[p] = 0;
            num[p] = 0;
            len[p] = w;
            return p++;
        }
        void init(){
            p = 0;
            newnode(0);
            newnode(-1);
            last = 0;
            n = 0;
            s[n] = -1; // 开头放一个字符集中没有的字符，减少特判
            fail[0] = 1;
        }
        int get_fail(int x){ // 和KMP一样，失配后找一个尽量最长的
            while(s[n-len[x]-1] != s[n]) x = fail[x];
            return x;
        }
        void add(int c){
            c -= 'a';
            s[++n] = c;
            int cur = get_fail(last);
            if(!nxt[cur][c]){
                int now = newnode(len[cur]+2);
                fail[now] = nxt[get_fail(fail[cur])][c];
                nxt[cur][c] = now;
                num[now] = num[fail[now]] + 1;
            }
            last = nxt[cur][c];
            cnt[last]++;
        }
    
        void count(){
            // 最后统计一遍每个节点出现个数
            // 父亲累加儿子的cnt,类似SAM中parent树
            // 满足parent拓扑关系
            for(int i=p-1;i>=0;i--)
                cnt[fail[i]] += cnt[i];
        }
        
    	long long getans() {
    		long long res = 0;
    		for(int i = 0; i < p; i ++) {
    			res = max(res, 1ll * len[i] * cnt[i]);
    		}
    		return res;
    	}
        
    }pam;
    
    char s[maxn];
    
    int main() {
    	scanf("%s", s);
    	pam.init();
    	int len = strlen(s);
    	for(int i = 0; i < len; i ++) pam.add(s[i]);
    	pam.count();
    	printf("%lld\n", pam.getans());
    	return 0;
    }
    

## The Problem to Slow Down You

2014-2015区域赛（西安）的G题  
<https://vjudge.net/problem/UVALive-7041>  
题目要求我们统计 2个串 多少相同回文串 我们回文树上的点 就是回文 按找字符串相同的方式 一样遍历 cnt1 * cnt2 + DFS更长的  
回文数树上DFS 处理

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 300005;// n(空间复杂度o(n*ALP)),实际开n即可
    const int ALP = 26;
     
    struct PAM { // 每个节点代表一个回文串
    	int nxt[maxn][ALP]; // nxt指针,参照Trie树
    	int fail[maxn]; // fail失配后缀链接
    	int cnt[maxn]; // 此回文串出现个数
    	int num[maxn];
    	int len[maxn]; // 回文串长度
    	int s[maxn]; // 存放添加的字符
    	int last; //指向上一个字符所在的节点，方便下一次add
    	int n; // 已添加字符个数
    	int p; // 节点个数
     
    	int newnode(int w) { // 初始化节点，w=长度
    		for(int i=0; i<ALP; i++)
    			nxt[p][i] = 0;
    		cnt[p] = 0;
    		num[p] = 0;
    		len[p] = w;
    		return p++;
    	}
    	void init() {
    		p = 0;
    		newnode(0);
    		newnode(-1);
    		last = 0;
    		n = 0;
    		s[n] = -1; // 开头放一个字符集中没有的字符，减少特判
    		fail[0] = 1;
    	}
    	int get_fail(int x) { // 和KMP一样，失配后找一个尽量最长的
    		while(s[n-len[x]-1] != s[n]) x = fail[x];
    		return x;
    	}
    	void add(int c) {
    		c -= 'a';
    		s[++n] = c;
    		int cur = get_fail(last);
    		if(!nxt[cur][c]) {
    			int now = newnode(len[cur]+2);
    			fail[now] = nxt[get_fail(fail[cur])][c];
    			nxt[cur][c] = now;
    			num[now] = num[fail[now]] + 1;
    		}
    		last = nxt[cur][c];
    		cnt[last]++;
    	}
     
    	void count() {
    		// 最后统计一遍每个节点出现个数
    		// 父亲累加儿子的cnt,类似SAM中parent树
    		// 满足parent拓扑关系
    		for(int i=p-1; i>=0; i--)
    			cnt[fail[i]] += cnt[i];
    	}
    } p1, p2;
    char s1[maxn], s2[maxn];
     
    long long dfs(int n1, int n2) {
    	long long res = 0;
    	for(int i = 0; i < 26; i ++) {
    		if(p1.nxt[n1][i] && p2.nxt[n2][i]) {
    			res += 1ll * p1.cnt[p1.nxt[n1][i]] * p2.cnt[p2.nxt[n2][i]] \
    			       + dfs(p1.nxt[n1][i], p2.nxt[n2][i]);;
    		}
    	}
    	return res;
    }
     
    int main() {
    	int cas;
    	scanf("%d", &cas);
    	int tt = 1;
    	while(cas --) {
    		scanf("%s", s1);
    		scanf("%s", s2);
    		p1.init(), p2.init();
    		int len1 = strlen(s1), len2 = strlen(s2);
    		for(int i = 0; i < len1; i ++) p1.add(s1[i]);
    		for(int i = 0; i < len2; i ++) p2.add(s2[i]);
    		p1.count(), p2.count();
    		printf("Case #%d: %lld\n", tt++, dfs(0, 0) + dfs(1, 1));
    	}
    	return 0;
    }
    

