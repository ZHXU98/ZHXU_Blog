##
回文树总结二_2019徐州网络赛_Colorful_String_和_湖南大学第十五届程序设计竞赛_H.Longest_Common_Palindrome_Substring

## Colorful String

问你 每个回文串 有多少不同字符 累和 输出  
回文树 统计回文串 DFS遍历 所有回文串 统计  
<https://nanti.jisuanke.com/t/41389>

    
    
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
        
        long long dfs(int now, int vis) {
        	int num = 0;
        	long long res = 0;
        	for(int i = 0; i < ALP; i ++) if(vis >> i & 1) num ++;
        	if(now >= 2) res += num * cnt[now];
        	for(int i = 0; i < ALP; i ++) {
        		if(nxt[now][i]) res += dfs(nxt[now][i], vis|(1 << i));
    		}
    		return res;
    	}
        
    	long long getans() {
    		long long res = 0;
    		res = dfs(0, 0);
    		res += dfs(1, 0);
    		return res;
    	}
        
    }pam;
    char s[maxn];
    
    int main() {
    	scanf("%s", s);
    	int len = strlen(s);
    	pam.init();
    	for(int i = 0; i < len; i ++) pam.add(s[i]);
    	pam.count();
    	printf("%lld\n", pam.getans());
    	return 0;
    }
    

## 湖南大学第十五届程序设计竞赛 H.Longest Common Palindrome Substring

<https://ac.nowcoder.com/acm/contest/908/H>  
最长公共回文串 直接 dfs 共有的回文串就好

    
    
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
    }p1, p2;
    char s1[maxn], s2[maxn];
    
    int dfs(int n1, int n2) {
    	int res = max(p1.len[n1], 1);
    	for(int i = 0; i < 26; i ++) {
    		if(p1.nxt[n1][i] && p2.nxt[n2][i]) {
    			res = max(res, p1.len[n1]);
    			res = max(res, dfs(p1.nxt[n1][i], p2.nxt[n2][i]));
    		}
    	}
    	return res;
    }
    
    int main() {
    	while(scanf("%s", s1) != EOF) {
    		scanf("%s", s2);
    		p1.init(), p2.init();
    		int len1 = strlen(s1), len2 = strlen(s2);
    		for(int i = 0; i < len1; i ++) p1.add(s1[i]);
    		for(int i = 0; i < len2; i ++) p2.add(s2[i]);
    		printf("%d\n", max(dfs(0, 0), dfs(1, 1)));
    	}
    	return 0;
    }
    

