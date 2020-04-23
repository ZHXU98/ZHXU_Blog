## 2019HDU杭电多校第二场_HDU_6599_I_Love_Palindrome_String_I题_回文树

以下 回文树板子

    
    
    const int maxn = 100005;// n(空间复杂度o(n*ALP)),实际开n即可
    const int ALP = 26;
     
    struct PAM{ // 每个节点代表一个回文串
        int next[maxn][ALP]; // next指针,参照Trie树
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
                next[p][i] = 0;
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
            if(!next[cur][c]){
                int now = newnode(len[cur]+2);
                fail[now] = next[get_fail(fail[cur])][c];
                next[cur][c] = now;
                num[now] = num[fail[now]] + 1;
            }
            last = next[cur][c];
            cnt[last]++;
        }
        void count(){
            // 最后统计一遍每个节点出现个数
            // 父亲累加儿子的cnt,类似SAM中parent树
            // 满足parent拓扑关系
            for(int i=p-1;i>=0;i--)
                cnt[fail[i]] += cnt[i];
        }
    }pam;
    

这道题 整段回文 和 前半段回文 就看出 前后是一样的 hash判断就好

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef unsigned long long ull; 
    const int maxn = 300005;// n(?????o(n*ALP)),???n??
    const int ALP = 27;
    string st;
    ull ha[maxn],pp[maxn];
    int ans[maxn];
    
    int get_hash(int l,int r){
        if(l == 0) return ha[r];
        return ha[r]-ha[l-1]*pp[r-l+1];
    }
    
    bool chk(int l,int r){
        int len=r-l+1;
        int mid=(l+r)>>1;
        if(len&1) return get_hash(l,mid)==get_hash(mid,r);
        else return get_hash(l,mid)==get_hash(mid+1,r);
    }
     
    struct PAM{ 
        int next[maxn][ALP]; 
        int fail[maxn]; 
        int cnt[maxn]; 
        int num[maxn];
        int len[maxn]; 
        int s[maxn]; 
        int last;
        int n; 
        int p; 
        int idx[maxn * ALP];
        int newnode(int w){ 
            for(int i=0;i<ALP;i++)
                next[p][i] = 0;
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
            s[n] = -1;
            fail[0] = 1;
        }
        int get_fail(int x){ 
            while(s[n-len[x]-1] != s[n]) x = fail[x];
            return x;
        }
        void add(int c){
            c -= 'a';
            s[++n] = c;
            int cur = get_fail(last);
            if(!next[cur][c]){
                int now = newnode(len[cur]+2);
                fail[now] = next[get_fail(fail[cur])][c];
                next[cur][c] = now;
                num[now] = num[fail[now]] + 1;
            }
            last = next[cur][c];
            cnt[last]++;
            idx[last] = n;
        }
        void count(){
            for(int i=p-1;i>=0;i--)
            	  cnt[fail[i]] += cnt[i];
    		for(int i = 2; i < p ;i ++ ) {
    			if(chk(idx[i] - len[i], idx[i] - 1)) {
    				for(int j = idx[i] - len[i]; j < idx[i];j ++) cout << st[j] ;cout << endl;
    				ans[len[i]] += cnt[i];
    			}
    		}
        }
    }pam;
    
    int main(){
    	pp[0] = 1;
    	for(int i = 1; i < maxn; i ++) pp[i] = pp[i - 1] * 131;
    	while(cin >> st) {
    		int len = st.size();
    		for(int i = 0; i <= len; i ++) ans[i] = 0;
    		pam.init();
    		for(int i = 0; i < len; i ++) pam.add(st[i]);
    
    		ha[0] = st[0];
    		for(int i = 1; i < len; i ++) 
    			ha[i] = ha[i - 1] * 131 + st[i];
    		pam.count();
    		for(int i = 1; i <= len; i ++) {
    			if(i != 1) cout << " ";
    			cout << ans[i];
    		}
    		cout << endl;
    	}
    	return 0;
    	
    } 
    

