## Codeforces_Round_#598_(Div._3)_A~F题解

[https://codeforces.com/contest/1256/p](https://codeforces.com/contest/1256/)

## A. Payment Without Change

题意 给了 a 个 n价值硬币 b个 1价值硬币 能不能拼出 S 问  
直接算下就行

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    #define int long long
    long long cas, a, b, n, s;
     
    signed main() {
        cin >> cas;
        while(cas --) {
            cin >> a >> b >> n >> s;
            if(a * n >= s) {
                int t = s / n;
                s -= t * n;
                if(s > b) {
                    cout << "NO" << endl;
                } else {
                    cout << "YES" << endl;
                }
            } else {
                s -= n * a;
                if(s > b) {
                    cout << "NO" << endl;
                } else {
                    cout << "YES" << endl;
                }
            }
        }
        return 0;
    }
    

## B. Minimize the Permutation

一开始读错题了 以为是随意交换n - 1次  
题意 : 相邻的位置 可以交换 这相邻位置一旦交换过就不在执行  
考虑 贪心的选择 第i大小数据放置到对应位置 同时我标记了这个是否换过 贪心到底就可以了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    #define int long long
    long long cas, b, n, s;
     
    int a[maxn];
    map<pair<int, int>, int> vis;
     
    signed main() {
        cin >> cas;
        while(cas --) {
            vis.clear();
            cin >> n;
            for(int i = 1; i <= n; i++)
                cin >> a[i];
     
            int cnt = n - 1;
            for(int i = 1; i <= n; i ++) {
                int pos = 0;
                for(int j = 1; j <= n; j ++) {
                    if(a[j] == i)
                        pos = j;
                }
                while(pos > i && a[pos] < a[pos - 1] && cnt > 0 && !vis[make_pair(pos, pos - 1)]) {
                    swap(a[pos - 1], a[pos]);
                    vis[make_pair(pos, pos - 1)] = 1;
                    pos --;
                    cnt --;
                }
            }
     
            for(int i = 1; i <= n; i ++)
                cout << a[i] << " ";
            cout << endl;
        }
        return 0;
    }
    

## C. Platforms Jumping

题意 给了你一些跳板 让你找到一个排列使你每次最多在跳d格的情况下 跳到n + 1 位置 (不是 n 位置 我wa14醉了)

一开始我们考虑 所有板子按顺序连续 挨着最右端排布 只要人跳不到 就接个板过去 看看最后我们能不能到n + 1 位置 同时 如果这人
可以直接调到后面连续的板子上 那么 我们就直接当他能去就好了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    const int mod = 1e9 + 7;
    #define int long long
     
    int cas;
    int n, m, k, d;
    string str, ans;
    int a[maxn], h[maxn];
     
    signed main() {
        cin >> n >> m >> d;
        int p1 = n;
        for(int i = 1; i <= m; i ++) cin >> h[i], p1 -= h[i];
        p1 += 1;
        int pos = 0;
        int cnt = 1;
        while(pos < n) {
        //	cout << pos << " " << p1 << " " << cnt << endl;
        	if(pos + d < p1) {
        		pos += d;
        		for(int i = 1; i <= h[cnt]; i ++) {
        			a[pos + i - 1] = cnt;
    			}
    			if(cnt == m + 1) break;
        		pos += h[cnt] - 1;
        		p1 += h[cnt];
        		cnt ++;
    		} else {
    			pos = n + 1;
    		}
    	}
    	if(pos <= n) cout << "NO" << endl;
    	else {
    		cout << "YES" << endl;
    		int j = n;
    		for(int i = m; i >= cnt; i --) {
    			for(int t = 1; t <= h[i]; t ++) {
    				a[j] = i;
    				j --;
    			}
    		}
    		for(int i = 1; i <= n; i ++) {
    			cout << a[i] << " ";
    		}cout << endl;
    	}
    	
        return 0;
    }
    

## D. Binary String Minimizing

仍然是 使 字典序最小 同时 01串 如果想让它尽可能的小 显然我尽可能的提0  
其实 我们肯定是将0提到前面 假定-1位置是个0 那么 每次遇到一个0 我们都是尽可能把它放到前一个0的位置后面一个
这样在操作数不够的时候我们只需要尽可能提0到最前位置 操作数够的 放到前一个0位置之后就好 O(1)O(1)O(1)的交换

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    const int mod = 1e9 + 7;
    #define int long long
     
    int cas;
    int n, k;
    string str, ans;
     
    signed main() {
        cin >> cas;
        while(cas --) {
        	cin >> n >> k;
    		cin >> str; 
    		ans = str;
    		int p1 = 0, p2 = 0;
    		while(p1 < str.size() && str[p1] == '0') p1 ++;
    		p2 = p1;
    		for(int i = p1; i < str.size();) {
    			while(i < str.size() && str[i] == '1') {
    				i ++;
    			}
    			if(i == n) break;
    			int t = min(k, i - p1);
    		//	cout << p1 << " " << i << " " << k << " " << t << endl;
    			ans[i - t] = '0';
    			str[i] = '1';
    			ans[i] = '1';
    			p1 ++;
    		//	cout << ans << endl;
    			k -= t;
    			if(k == 0) break;
    		}
    		cout << ans << endl;
    	}
         
        return 0;
    }
    

## E. Yet Another Division Into Teams

题意 : 你要进行分队 每队至少3人 你的目的是让每队 内的 最大值 减 最小值 求和 尽可能的小  
我们考虑 先排序 让每队尽可能的差小 显然 如果满足6人 我们一定把它拆了  
1 2 4 6 7 8 这样的数据一次作差 1 2 2 1 1 我们从中分开 显然会少加一个数据作为最大值减最小值 ans变小  
满足6人 显然拆2队比较合适 这样 我们dp复杂度就降低了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 2005;
    const int mod = 1e9 + 7;
     
    pair<int, int> a[maxn];
    int dp[maxn];
    int ans[maxn];
    int n, m;
     
    signed main() {
        cin >> n;
        for(int i = 1; i <= n; i ++) cin >> a[i].first, a[i].second = i;
        sort(a + 1, a + 1 + n);
     
        for(int i = 1; i <= n; i ++) {
            dp[i] = 0x3f3f3f3f;
            for(int j = 3; j <= min(i, 6); j ++) {
                if(dp[i] > dp[i - j] + a[i].first - a[i - j + 1].first)
                    dp[i] = dp[i - j] + a[i].first - a[i - j + 1].first;
            }
        }
        int cnt = 0;
        for(int i = n; i; ) {
            for(int j = 3; j <= min(6, i); j ++) {
                if(dp[i] == dp[i - j] + a[i].first - a[i - j + 1].first) {
                    ++ cnt;
                    for(int k = 1; k <= j; k ++) ans[a[i - k + 1].second] = cnt;
                    i = i - j;
                    break;
                }
            }
        }
     
        cout << dp[n] << " " << cnt << endl;
        for(int i = 1; i <= n; i ++)
            cout << ans[i] << " ";
        cout << endl;
     
        return 0;
    }
    

## F. Equalizing Two Strings

题意 你可以无限次交换相邻2个数据 但是 上下交换的次数得一直 要求你换的一样  
一开始就考虑到逆序对了  
然后 考虑 无解的情况只有数据字母对不上 还有 逆序对奇偶性不一样  
这样大概就过 其实 如果字母个数对上 一旦某个数据 出现2次 那么 就意味着 可以另外一个串可以在不改动的情况下 自己可以无限交换  
abcc 和 cabc 只要c换到一起 我们每次换cc 就可以 在不改变一个串的情况下 无限改另外一个串

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 2005;
     
    string s1, s2;
    string S, T;
    int cas, n;
     
    int cal(const string& s) {
        int ret = 0;
        for(int i = 0; i < n; i ++)
            for(int j = 0; j < i; j ++)
                if(s[i] < s[j])
                    ret ++;
        return ret;
    }
     
    signed main() {
        cin >> cas;
        while(cas --) {
            cin >> n;
            cin >> s1 >> s2;
            S = s1, T = s2;
            int f = 0;
            sort(s1.begin(), s1.end());
            sort(s2.begin(), s2.end());
     
            for(int i = 0; i < n; i ++) {
                if(s1[i] != s2[i]) {
                    f = 1;
                    cout << "NO" << endl;
                    break;
                }
            }
            if(f) continue;
            for(int i = 1; i < n; i ++) {
                if(s1[i] == s1[i - 1] || s2[i] == s2[i - 1]) {
                    f = 1;
                    cout << "YES" << endl;
                    break;
                }
            }
            if(f) continue;
            if(cal(S) % 2 == cal(T) % 2)
                cout << "YES" << endl;
            else
                cout << "NO" << endl;
        }
        return 0;
    }
    

