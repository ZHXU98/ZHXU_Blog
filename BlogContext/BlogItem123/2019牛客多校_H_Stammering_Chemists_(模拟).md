## 2019牛客多校_H_Stammering_Chemists_(模拟)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190819135104662.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
这题找到他们的不同特征判断就好了  
题意还说 不是下面的 随便输出 就可以少盘一种了 虽然也没有少写啥  
第一个 连边 只有2个是出现1次的  
4 和 5 用 2个 3边 和 一个4边 判断

2 和 3 我dfs2边 3 的话 4深度出现2次 剩下的直接出 2图就好

    
    
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;
     
    int f[100];
    int n, m;
     
    string str = "*COFFEECHICKEN";
     
    int head[10], cnt;
    int nxt[15], to[15];
     
    void ade(int a, int b) {
        to[++cnt] = b;
        nxt[cnt] = head[a], head[a] = cnt;
    }
     
    int d[15];
    void dfs(int x, int fa, int dep){
        d[x] = dep;
        for(int i = head[x]; i; i = nxt[i]) {
            if(to[i] == fa) continue;
            dfs(to[i], x, dep + 1);
        }
    }
    int du[15];
    signed main() {
        int cas;
        cin >> cas;
        while(cas --) {
            memset(head, 0, sizeof head);
            cnt = 0;
            memset(du, 0, sizeof du);
            int du1 = 0, du2 = 0, du3 = 0, du4 = 0;
            for(int i = 1, a, b; i <= 5; i ++) {
                cin >> a >> b;
                ade(a, b), ade(b, a);
                du[a] ++, du[b] ++;
            }
             
            for(int i = 1; i <= 6; i ++) {
                if(du[i] == 1) du1 ++;
                if(du[i] == 2) du2 ++;
                if(du[i] == 3) du3 ++;
                if(du[i] == 4) du4 ++;
            }
        //  cout << du1 << " " << du2 << " " << du3 << " " << du4 << endl;
            if(du4 != 0) {
                cout << "2,2-dimethylbutane" << endl;
            }else if(du3 == 2) {
                cout << "2,3-dimethylbutane" << endl;
            }else if(du1 == 2 && du2 == 4) {
                cout << "n-hexane" << endl;
            }else {
                memset(d, 0, sizeof d);
                int rt = 1;
                dfs(rt, 0, 1);
                int tmp = 0;
                for(int i = 1; i <= 6; i ++) {
                //  cout << d[i] << "  ";
                    if(d[i] > tmp) rt = i, tmp = d[i];
                }
                memset(d, 0, sizeof d);
                dfs(rt, 0, 1);
                int d4 = 0, d5 = 0;
                for(int i = 1; i <= 6; i ++) {
                //  cout << d[i] << "  ";
                    if(d[i] == 4) d4 ++;
                }
                //cout << d4 << endl;
                if(d4 == 2) {
                    cout << "3-methylpentane" << endl;
                }else {
                    cout << "2-methylpentane" << endl;
                }
            }
             
        }
        return 0;
    }
    

