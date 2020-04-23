## 2019_牛客多校第十场_E_Hilbert_Sort_(分形__平面坐标旋转)

算法竞赛进阶指南 差不多就是 分形之城  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190819162241932.jpg?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
看作向量旋转 平移 细节还不算多

    
    
    #include <bits/stdc++.h>
    using namespace  std;
     
    long long f(int n, int x, int y) {
        if (n == 0) return 1;
        int m = 1 << (n - 1);
        if (x <= m && y <= m) {
            return f(n - 1, y, x);
        }
        if (x <= m && y > m) {
            return 3LL * m * m + f(n - 1, m * 2 - y + 1 , m - x + 1);
        }
        if (x > m && y <= m) {
            return 1LL * m * m + f(n - 1, x - m, y);
        }
        if (x > m && y > m) {
            return 2LL * m * m + f(n - 1, x - m, y - m);
        }
    }
     
    struct node {
        int x, y;
        int id;
        bool operator < (const node &a) const {
            return id < a.id;
        }
    } a[1000005];
     
    int main() {
        int n, k;
        scanf("%d %d", &n, &k);
        for(int i = 1; i <= n; i ++) {
            scanf("%d %d", &a[i].x , &a[i].y);
        }
        for(int i = 1; i <= n; i ++) {
            a[i].id = f(k, a[i].x, a[i].y);
        }
        sort(a + 1, a + 1 + n);
        for(int i = 1; i <= n; i ++) {
            printf("%d %d\n",a[i].x, a[i].y);
        }
        return 0;
    }
    

