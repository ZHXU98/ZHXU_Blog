## 【KMP】_Codeforces_Round_#578_(Div._2)_E.Compress_Words

## Codeforces Round #578 (Div. 2) E.Compress Words

KMP 我们需要处理将前一个字符串后缀 与 后面一个字符串的后缀最相同 进行合并  
KMP 算法原理都快忘了 居然还能这么用  
我们 对后面的字符串求NXT数组 将前一个字符串后 min(n - m, 0) 的字符串 与 它KMP匹配 找 可以匹配的位置 如果全找到(t2 == m)
直接不合并 如果 没有全匹配上 我们t2指针 也是指向 我们和前一个字符串进行匹配 s2 与s1可以最后匹配到的位置  
这是kmp算法本质效果

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e6 + 5;
     
    int n, m, cas;
    string ans, str;
    int nxt[maxn];
     
    void get_nxt(int n, const string &s){
        nxt[0] = -1;
        int i = 0, j = -1;
        while(i < n){
            if(j == -1 || s[j] == s[i]){
                i++, j++;
                nxt[i] = j;
            }
            else j = nxt[j];
        }
    }
     
    int kmp(int st, int n, int m, const string &s1, const string &s2) {
    	int t1 = st, t2 = 0;
    	get_nxt(s2.size(), s2);
    	while(t1 < n) {
    		if(t2 == -1 || s1[t1] == s2[t2]) t1 ++, t2 ++;
    		else t2 = nxt[t2];
    		if(t2 == m) return t2;
    	}
    	return t2;
    }
     
    void sol(int n, int m, const string &s1, const string &s2) {
    	int pos = kmp(max(n - m, 0), n, m, s1, s2);
    	for(int i = pos; i < m; i ++) 
    		ans.push_back(str[i]);
    }
     
     
    int main() {
    	ios::sync_with_stdio(false);cin.tie(0);
    	cin >> n;
    	cin >> ans;
    	for(int i = 2; i <= n; ++ i) {
    		cin >> str;
    		sol(ans.size(), str.size(), ans, str);
    	}
    	cout << ans << endl;
    	return 0;
    }
    

