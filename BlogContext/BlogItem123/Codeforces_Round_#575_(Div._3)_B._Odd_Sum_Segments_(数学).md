## Codeforces_Round_#575_(Div._3)_B._Odd_Sum_Segments_(数学)

## B. Odd Sum Segments

<https://codeforces.com/contest/1196/problem/B>

给了你一个序列 让你把他们分成k段 每段都是奇数  
对k段求sum(这一段的奇数数量) 最后他们的和 是奇数的总和  
这样的话 k 是奇数 那么 奇数的个数也是奇数  
他们的奇偶性是一样的  
然后 确保 cnt >= k 每段都分一个 剩下的都给最后一段端

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 5;
     
    int n, m, k, cas;
    int a[maxn];
     
    signed main() {
        cin >> cas;
        while(cas --) {
            cin >> n >> k;
            int odd = 0;
            for(int i = 1; i <= n; i ++) {
                cin >> a[i];
                if(a[i] & 1) odd ++;
            }
            if(odd < k || (k % 2 != odd % 2)) {
                cout << "NO" << endl;
            } else {
                int pos = 0;
                cout << "YES" << endl;
                for(int i = 1; i < n; i ++) {
                    if(a[i] & 1) {
                        if(pos == k - 1) break;
                        else cout << i << " ";
                        pos ++;
                    }
                }
                cout << n << endl;
            }
        }
        return 0;
    }
    

