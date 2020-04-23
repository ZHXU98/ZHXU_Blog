## B_-_Heshen's_Account_Book_HihoCoder_-_1871_北京ICPC_字符串模拟

北京有毒 字符串年年是毒瘤。。。。。。

一些样例  
特别注意 行首空格 。。。。 0拍在一起

    
    
    0
    0
    0
    输出
    空行
    0
    0
    0
    
    123 12
    (空格) 12 0
    bb1
    输出 
    123 12 12 0
    2
    2
    0
    
    12 a15 9
    12356
    54wwwf 6
    输出
    12 9123456 6
    2
    0
    1
    
    0
    a
    b
    c
    输出
    0
    1
    0
    0
    0
    
    
    
    
    #include <bits/stdc++.h>
    typedef unsigned long long ull;
    using namespace std;
    const int maxn = 1005;
    long long n, m, p;
    
    string str;
    vector<ull> G[maxn];
    vector<string> s[maxn];
    int V[maxn];
    vector<ull> ans;
    
    void sol(int x, int y) {
        string tmp = s[x][y];
        if(tmp.size() == 1 && tmp[0] == '0') {
            ans.push_back((ull)0);
            V[x] ++;
            return ;
        }
        if(tmp[0] == '0') return ;
        if(!isdigit(tmp[0]) || !isdigit(tmp[tmp.size() - 1])) return;
        ull res = 0;
        for(int i = 0; i < tmp.size(); i ++) {
            if(isdigit(tmp[i])) {
                res = res * 10 + tmp[i] - '0';
            }
        }
        G[x].push_back(res);
        ans.push_back(res);
        V[x] ++;
    }
    
    signed main() {
        int cnt = 0;
        int f = 1;
        string tmp = "";
        while(getline(cin, str)) {
           // cout << str << endl;
            ++cnt;
            int i;
            for(i = 0; i < str.size(); i ++) {
            //    cout << tmp << endl;
                if(i == 0 && tmp != "") {
                    if(!isdigit(str[i])) s[f].push_back(tmp), tmp = "", f = cnt;
                  //  else if(tmp.size() == 1 && tmp[0] == '0') s[f].push_back("0"), tmp = "", f = cnt;
                    else if(isdigit(str[i])) tmp += str[i];
                    else tmp = "";
                }
                else if(str[i] == ' ') s[f].push_back(tmp), tmp = "", f = cnt;
                else tmp += str[i];
            }
            if(!isdigit(str[str.size() - 1])) tmp = "", f ++;
        }
        s[f].push_back(tmp);
    //    for(int i = 1; i <= cnt; i ++) {
    //        for(int j = 0; j < s[i].size(); j ++) {
    //            cout << s[i][j] << " ";
    //        }cout << endl;
    //    }
    
        for(int i = 1; i <= cnt; i ++) {
            for(int j = 0; j < s[i].size(); j ++) {
                sol(i, j);
            }
        }
    
    //    for(int i = 1; i <= cnt; i ++) {
    //        for(int j = 0; j < G[i].size(); j ++) {
    //            cout << G[i][j] << " ";
    //        }cout << endl;
    //    }
    
        for(int i = 0; i < ans.size(); i ++) {
            if(i != 0)  cout << " ";
            cout << ans[i] ;
        }cout << endl;
        for(int i = 1; i <= cnt; i ++) {
            cout << V[i] << endl;
        }
        return 0;
    }
    

