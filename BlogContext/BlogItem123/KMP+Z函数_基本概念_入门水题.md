## KMP+Z函数_基本概念_入门水题

KMP  
Z函数 | 拓展KMP

## kmp

字符串 abcabcd 的前缀函数为 0 0 0 1 2 3 0 ，  
字符串 aabaaab的前缀函数为 0 1 0 1 2 2 3 .  
以第 i 个 作为 结尾 和前缀匹配 最多往前匹多长

## Z函数

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019081211182145.png)  
已第i个 作为开始 和前缀匹配 往后面最长匹配多少

##### 洛谷板子题 kmp 找位置

    
    
    #include<iostream>
    #include<cstdio>
    #include<cstring>
    using namespace std;
    const int maxn = 1e6 + 10;
    
    int nxt[maxn];
    char s1[maxn], s2[maxn];
    
    void get_nxt(int n, char *s){
        memset(nxt, 0, (n + 5) * sizeof(int));
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
    
    void kmp(int n, int m) {
    	int t1 = 0, t2 = 0;
    	while(t1 < n) {
    		if(t2 == -1 || s1[t1] == s2[t2]) t1 ++, t2 ++;
    		else t2 = nxt[t2];
    		if(t2 == m) {
    			printf("%d\n", t1 - m + 1);
    			t2 = nxt[t2]; // t2 = 0 不重复用的输出
    		}
    	}
    }
    
    int main() {
    	scanf("%s %s", s1, s2);
    	int n, m;
    	n = strlen(s1), m = strlen(s2);
    	get_nxt(m, s2);
    	kmp(n, m);
    	for(int i = 1; i <= m; i ++) printf("%d ", nxt[i]);
    	puts("");
    	return 0;
    }
    

##### kmp 找循环节

POJ 2406

    
    
    #include<iostream>
    #include<cstdio>
    #include<cstring>
    using namespace std;
    const int maxn = 1e6 + 10;
    
    int nxt[maxn];
    char s[maxn];
    int n;
    
    void get_nxt(){
        memset(nxt, 0, (n + 5) * sizeof(int));
        nxt[0] = -1;
        int i = 0, j = -1;
        while(i < n){
            if(j == -1 || s[j] == s[i]){
                i++;
                j++;
                nxt[i] = j;
            }
            else
                j = nxt[j];
        }
    }
    
    int main() {
    	while(scanf("%s", s) && s[0] != '.') {
    		n = strlen(s);
    		get_nxt();
    		if(n % (n - nxt[n]) != 0) puts("1");
    		else printf("%d\n", n / (n - nxt[n]));
    	}
    	return 0;
    }
    

##### kmp 找所有循环节周期

POJ 1961

    
    
    #include<iostream>
    #include<cstdio>
    #include<cstring>
    using namespace std;
    const int maxn = 1e6 + 10;
    
    int nxt[maxn];
    char s[maxn];
    int n;
    
    void get_nxt(){
        memset(nxt, 0, (n + 5) * sizeof(int));
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
    
    int main() {
    	int cas = 1;
    	while(scanf("%d", &n) && n) {
    	
    		scanf("%s", s);
    		get_nxt();
    		printf("Test case# %d\n", cas ++);
    		for(int i = 1; i <= n; i ++) {
    			int k = i - nxt[i];
    			if(i % k == 0 && i != k) printf("%d %d\n", i, i/k);
    		}	puts("");
    	}
    	return 0;
    }
    

## Z 函数

#### Codeforces 126B Password

你要在一个串中找到“密码”，密码定义为既是前缀，也是后缀，同时在串中间出现过的子串。  
i + z[i] == n i位开始能匹配到的 后缀 同时 ma >= n - i 之前 出现过不小它 的串

    
    
    #include<bits/stdc++.h>
    using namespace std;
    const int maxn = 1e6 + 10;
     
    int z[maxn];
    string s;
    int n;
     
    void get_z() {
    	int l=0,r=0;
    	for (int i=1; i<n; i++) {
    		if (i>r) {
    			l=i,r=i;
    			while (r<n && s[r-l]==s[r]) r++;
    			z[i]=r-l,r--;
    		} else {
    			int k=i-l;
    			if (z[k]<r-i+1) z[i]=z[k];
    			else {
    				l=i;
    				while (r<n && s[r-l]==s[r]) r++;
    				z[i]=r-l,r--;
    			}
    		}
    	}
    }
     
    int main() {
    	cin >> s;
    	n = s.size();
    	get_z();
    	int pos = -1, ma = 0;
    	for(int i = 1; i < n; i ++) {
    		if(z[i] + i == n && ma >= n - i) {
    			pos = i;
    			break;
    		}
    		ma = max(ma, z[i]);
    	}
     
    //	for(int i = 1; i < n; i ++) {
    //		cout << z[i] << " ";
    //	}
    //	cout << endl;
    //	cout << pos << " " << ma << endl;
    	if(pos == -1) cout << "Just a legend";
    	else for(int i = 0; i < n - pos; i ++) cout << s[i];
    	cout << endl;
    	return 0;
    }
    

#### Codeforces 535D Tavas and Malekas

每个位置 如果不与之前重合 直接放就行  
重合 判断 能不能合法放进去  
用差分标记 最后0得位置就是26 ^ 多少

    
    
    #include<bits/stdc++.h>
    using namespace std;
    const int maxn = 1e6 + 10;
    const int mod = 1e9 + 7;
    
    int z[maxn], pos[maxn], a[maxn];
    string s;
    int n, m, l;
    
    void get_z(int n) {
    	int l=0,r=0;
    	for (int i=1; i<n; i++) {
    		if (i>r) {
    			l=i,r=i;
    			while (r<n && s[r-l]==s[r]) r++;
    			z[i]=r-l,r--;
    		} else {
    			int k=i-l;
    			if (z[k]<r-i+1) z[i]=z[k];
    			else {
    				l=i;
    				while (r<n && s[r-l]==s[r]) r++;
    				z[i]=r-l,r--;
    			}
    		}
    	}
    }
    
    bool chk(int i, int j) {
    	if(i + l <= j) return 1;
    	return z[j - i] >= i + l - j;
    }
    
    int ksm(int a, int b) {
    	int res = 1;
    	for(; b; b >>= 1) {
    		if(b & 1) res = 1ll * res * a % mod;
    		a = 1ll * a * a % mod;
    	}
    	return res;
    }
    
    signed main() {
    	cin >> n >> m;
    	cin >> s;
    	l = s.size();
    	get_z(l);
    	for(int i = 1; i <= m; i ++) cin >> pos[i], pos[i]--;
    
    	for(int i = 1; i < m; i ++) {
    		if(chk(pos[i], pos[i + 1])) {
    			a[pos[i]] ++, a[pos[i] + l] --;
    		} else {
    			cout << 0 << endl;
    			return 0;
    		}
    	}
    	if(m) a[pos[m]] ++, a[pos[m] + l] --;
    	for(int i = 1; i < n; i ++) 
    		a[i] += a[i - 1];
    	
    	int tot = 0;
    	for(int i = 0; i < n; i ++) if(!a[i]) tot ++;
    	cout << ksm(26, tot) << endl;
    	return 0;
    }
    

