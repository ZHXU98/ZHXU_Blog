## 2019牛客国庆集训派对day2_A_B_C_F_G_K

<https://ac.nowcoder.com/acm/contest/1107#question>

## A Easy h-index

A 题 读懂第一句话 我举得A B C 应该都一样简单的 翻译真难  
H _ index 指的是 论文引用次数 大于等于 h 的总数量 h的最大可能值

所以 我们 A题 直接到这for 一边 找到论文总数量大于i的第一次时刻就好

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 2e5 + 5;
     
    int n;
    int a[maxn];
     
    int main() {
        while(cin >> n) {
            for(int i = 0; i <= n; i ++) cin >> a[i];
            int siz = 0;
            for(int i = n; i >= 0; i --) {
                siz += a[i];
                if(siz >= i) {
                        cout << i << endl;
                    break;
                }
            }
        }
        return 0;
    }

## B Higher h-index

这题 没有给你数据 但是给你 总时间 以及系数 a  
我们考虑 ~~猜对就对了 2333~~  
他一定是平摊才是最好的 任何减少论文数量都会让 引用次数增加 反而论文数量跟不上  
最后发现 下界 是 （n / 2）a 只会导致论文引用直接增加一次a  
所以加上a 之后就是 (n + a) / 2, or n 我没有注意a有上届 所以写多了应该情况

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 2e5 + 5;
     
    long long n, a;
     
    int main() {
        while(cin >> n >> a) {
            cout << min((n + a) / 2, n) << endl;
        }
        return 0;
    }

## C Just h-index

二分主席树 也是醉了 题目蛮裸的 也不至于二分 树上直接二分就好  
处理区间的 h 数据 我们第一反应就是主席树了 然后思考 这个数据如何出现其实也就是 这之后论文数量和 > 当前数据枚举的最左段 二分就好 当然直接树上二分
少一个lg

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e5 + 5;
     
    int n, m;
    struct node {
        int lc, rc;
        int mas;
    } tree[maxn * 20];
    int tot, root[maxn];
     
    int build(int l, int r) {
        int p = ++ tot;
        tree[p].mas = 0;
        if(l == r)
            return p;
        int mid = l + r >> 1;
        tree[p].lc = build(l, mid);
        tree[p].rc = build(mid + 1, r);
        return p;
    }
     
    int ins(int now, int l, int r, int x, int val) {
        int p = ++ tot;
        tree[p] = tree[now];
        if(l == r) {
            tree[p].mas += val;
            return p;
        }
        int mid = l + r >> 1;
        if(x <= mid)
            tree[p].lc = ins(tree[now].lc, l, mid, x, val);
        else
            tree[p].rc = ins(tree[now].rc, mid + 1, r, x, val);
        tree[p].mas = tree[tree[p].lc].mas + tree[tree[p].rc].mas;
        return p;
    }
     
    int ask_sum(int p, int q, int l, int r, int L, int R) {
        if(R < L)
            return 0;
        if(L <= l && r <= R)
            return tree[p].mas - tree[q].mas;
        int mid = l + r >> 1;
        int res = 0;
        if(L <= mid)
            res += ask_sum(tree[p].lc, tree[q].lc, l, mid, L, R);
        if(R > mid)
            res += ask_sum(tree[p].rc, tree[q].rc, mid + 1, r, L, R);
        return res;
    }
     
    bool ok(int mid, int l, int r) {
        int cnt = ask_sum(root[r], root[l - 1], 1, n, mid, n);
        if(cnt >= mid) return 1;
        else return 0;
    }
     
    void chk(int ls, int rs) {
        int l = 1, r = 1e5;
        while(l < r) {
            int mid = l + r + 1 >> 1;
            if(ok(mid, ls, rs))
                l = mid;
            else
                r = mid - 1;
        }
        printf("%d\n", l);
    }
     
    struct no {
        int l, r;
        int id;
    } q[maxn];
     
    int a[maxn];
     
    int main() {
        ios::sync_with_stdio(0), cin.tie(0);
        while(cin >> n >> m) {
            tot = 0;
            root[0] = build(1, maxn - 5);
            for(int i = 1; i <= n; i ++)
                cin >> a[i], root[i] = ins(root[i - 1], 1, n, a[i], 1);
            for(int i = 1; i <= m; i++) {
                cin >> q[i].l >> q[i].r;
                q[i].id = i;
                chk(q[i].l, q[i].r);
            }
        }
        return 0;
    }

## F Sorting

按照 题面给的 直接重载排序就好  
long long 数据范围只有3e18 所以 long double 要注意下

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e4 + 5;
     
    int n;
    struct node {
        long double a, b, c;
        long long id;
        bool operator < (const node & no) const {
            return (a + b) * (no.a + no.b + no.c) < (no.a + no.b) * (a + b + c) \
            || ((a + b) * (no.a + no.b + no.c) == (no.a + no.b) * (a + b + c) && id < no.id);
        }
    }a[maxn];
     
    int main() {
        ios::sync_with_stdio(0),cin.tie(0);
        while(cin >> n) {
            for(int i = 1; i <= n; i ++) {
                cin >> a[i].a >> a[i].b >> a[i].c;
                a[i].id = i;
            }
            sort(a + 1, a + 1 + n);
            for(int i = 1; i <= n; i ++) {
                if(i != 1) cout << " ";
                cout << a[i].id;
            } cout << endl;
        }
        return 0;
    }

## G String Transformation

给了你 一个字符串 你可以选择 aa bb abab 插入删除 同时也只能对这3种处理  
当我看到 ab 可以变成 ba 我就反应过来 23333  
其实 用c分割就好 直接看 每个隔断的 a b 数量奇偶 就ok了

    
    
    #include <bits/stdc++.h>
    using namespace std;
     
    vector<int> od, en;
    vector<int> od1, en1;
    string s1, s2;
    int main() {
        while(cin >> s1) {
            cin >> s2;
            od.clear();
            en.clear();
            od1.clear();
            en1.clear();
            int a = 0, b = 0, c = 0;
            for(int i = 0; i < s1.size(); i ++) {
                if(s1[i] == 'a') a ++;
                else if(s1[i] == 'b') b++;
                else {
                    c ++;
                    od.push_back(a & 1);
                    en.push_back(b & 1);
                    a = 0, b = 0;
                }
            }
            od.push_back(a & 1);
            en.push_back(b & 1);
            a = 0, b = 0;
            int c1 = 0;
            for(int i = 0; i < s2.size(); i ++) {
                if(s2[i] == 'a') a ++;
                else if(s2[i] == 'b') b++;
                else {
                    c1 ++;
                    od1.push_back(a & 1);
                    en1.push_back(b & 1);
                    a = 0, b = 0;
                }
            }
            od1.push_back(a & 1);
            en1.push_back(b & 1);
            if(c != c1) {
                cout << "No" << endl;
            } else {
                int f = 0;
                for(int i = 0; i < od.size(); i ++) {
                  //  cout << od[i] << " " << od1[i] << " || " << en[i] << " " << en1[i] << endl; 
                    if(od[i] != od1[i] || en[i] != en1[i]) {
                        cout << "No" << endl;
                        f = 1;
                        break;
                    }
                }
                if(!f) cout << "Yes" << endl;
            }
        }
        return 0;
    }

## K 2018

哎 签到题 自己做的好菜啊

首先可以肯定是 这题是容斥

  1. a b 区间长度 * c d 区间的 2018 倍数的 数量
  2. c d 区间的长度 * a b 区间的 2018 倍数 的数量
  3. 我们根据容斥 必然要剪掉 a b， c d 区间内2018 数量导致重复的对数
  4. 其次 我们考虑1009 首先是 a b 区间 2 的数量 要减去 2018 的数量 * cd 区间1009 的数量 这样做 是为了不予 2018 乘出来的 冲突
  5. 同理 cd 区间对ab区间的处理

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e3 + 5;
     
    long long a, b, c, d;
     
    int main() {
        ios::sync_with_stdio(0), cin.tie(0);
        while(cin >> a >> b >> c >> d) {
            long long ans = 0;
            long long a1 = (b) / 1 - (a - 1) / 1;
            long long a2 = (b ) / 2 - (a - 1) / 2;
            long long a1009 = (b) / 1009 - (a- 1) / 1009;
            long long a2018 = (b ) / 2018 - (a - 1) / 2018;
     
            long long b1 = (d ) / 1 - (c - 1) / 1;
            long long b2 = (d ) / 2 - (c - 1) / 2;
            long long b1009 = (d ) / 1009 - (c - 1) / 1009;
            long long b2018 = (d) / 2018 - (c - 1) / 2018;
     
            ans += (a2 - a2018) * (b1009 - b2018);
            ans += (b2 - b2018) * (a1009 - a2018);
     
            ans += (a2018) * (d - c + 1);
            ans += (b2018) * (b - a + 1);
     
            ans -= (a2018) * b2018;
     
    //        ans -= ();
            cout << ans << endl;
     
        }
        return 0;
    }

