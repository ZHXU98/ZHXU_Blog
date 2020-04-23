##
Educational_Codeforces_Round_75_(Rated_for_Div._2)_C._Minimize_The_Integer_【思维】

## C. Minimize The Integer

<https://codeforces.com/contest/1251/problem/C>  
这题过的太慢了 自己好菜啊  
给了你一个排序规则 只能相邻的换 而且 换还需要保证他们的奇偶行不同  
这样 哪怕看样例联系到奇偶性 就很快可以看出 同奇偶的数据他们的顺序是不可能边了 但是不同奇偶的 可以随便换 这题就水了 自己想了好久
怎么取找最前面可以换的不同奇偶数 写了好久 慢慢才发现 不需要

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 106 + 10;
     
    int cas, n;
    string str;
    queue<char> o, e;
     
    int main() {
        cin >> cas;
        while(cas --) {
            cin >> str;
            for(int i = 0; i < str.size(); i ++) {
                if((str[i] - '0') % 2 == 1)
                    o.push(str[i]);
                else
                    e.push(str[i]);
            }
            while(!o.empty() || !e.empty()) {
                if(o.empty()) {
                    cout << e.front();
                    e.pop();
                } else if(e.empty()) {
                    cout << o.front();
                    o.pop();
                } else {
                    if(e.front() > o.front()) {
                        cout << o.front();
                        o.pop();
                    } else {
                        cout << e.front();
                        e.pop();
                    }
                }
            }
            cout << endl;
            //cout << str << endl;
        }
        return 0;
    }
    

