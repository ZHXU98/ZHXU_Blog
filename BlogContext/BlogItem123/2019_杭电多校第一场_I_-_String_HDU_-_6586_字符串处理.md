## 2019_杭电多校第一场_I_-_String_HDU_-_6586_字符串处理

给了你一个字符串 要求你给出 长度为 k 的 子序列 同时要满足 输入 每个字母出现次数的 区间  
我们贪心 + 枚举 就是不太好写

我们处理出每个字母之后下次出现的位置 同时记录之后个数

我们K次枚举26个字母是否可以放入  
之前位置合法 就将队列里面 之前位置扔了  
如果这个字符放入 我们每一个 字母 z  
l[z] - num[z]， 0 长度和 大于剩下位置 就是非法的  
同时 R[z] - num[z] 最多放入还不够 k - i 位也是非法的

合法的直接放入字符串 进入下一层 不然就重复 如果26次还不可以 输出-1

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    const int maxn = 1e5 + 5;
    char s[maxn];
    int sum[maxn][27];
    int num[27];
    int L[27], R[27];
    int k;
    queue<int> que[27];
    
    int main() {
        while(cin >> s >> k){
            string ans;
            int len = strlen(s);
            for(int i = 0; i < 26; i ++){
                cin >> L[i] >> R[i];
                num[i] = 0;
                while(!que[i].empty()) que[i].pop();
                sum[len][i] = 0;
            } 
            for(int i = 0; i < len; i ++) que[s[i] - 'a'].push(i);
            for(int i = len - 1; i >=0; i --) {
                for(int j = 0; j < 26; j ++) sum[i][j] = sum[i + 1][j];
                sum[i][s[i] - 'a'] ++;
            }
        
            bool flag = 0;
            int pos = -1;
            for(int i = 1; i <= k; i ++ ) {
                flag = 0;
                for(int j = 0; j < 26; j ++ ) {
                    if(num[j] >= R[j]) continue;
                    while(!que[j].empty() && que[j].front() <= pos) que[j].pop();
                    if(!que[j].empty()) {
                        int ok = 1;
                        num[j] ++;
                        for(int z = 0; z < 26; z ++ ) {
                            if(sum[que[j].front() + 1][z] + num[z] < L[z]) {
                                ok = 0;
                                break;
                            }
                        }
                        if(ok) {
                            int lma = 0, lmi = 0;
                            for(int z = 0; z < 26; z ++ ) lmi += max(L[z] - num[z], 0);
                            for(int z = 0; z < 26; z ++ ) lma += (R[z] - num[z])
                            if(lmi > k - i || lma < k - i) ok = 0;
                        }
                        
                        if(ok == 0) num[j] --;
                        else{
                            pos = que[j].front();
                            ans.push_back(j + 'a');
                            flag = 1;
                            break;
                        }
                    }
                }
                if(flag == 0) break;
            }
            if(flag) {
                for(int i = 0; i < ans.size(); i ++) cout << ans[i] ;puts("");
            }else cout << -1 << endl;
        }
        return 0; 
    }
    

