## 2019HDU多校第二场_HDU_6598_Harmonious_Army_(最小割)

还是第一次见到网络流 还能这么见图的 找最小割 看最大匹配价值的  
mark 学习了  
割图 肯定分成要不和s连 要不和t连  
如果多个点 之间还有价值 同时在一个集合中 比如把 a， b 割了  
总价值 - 最小割 我们会把e算进去 不丢掉点与点之间的价值  
割边的时候 这些情况都照顾到了  
我们解方程建边 算最小割  
总价值 剪掉这个最小割 就是我们匹配的最大价值  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190731190712775.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
注释部分 跑300ms 还是自己板子好啊 才90ms

    
    
    #include<bits/stdc++.h>
    using namespace std;
    #define inf 0x3f3f3f3f
    
    const int N = 515;
    const int M = 6e4 + 10;
    
    int head[N], depth[N], cur[N], cnt = 1;
    int nxt[M << 1], to[M << 1], cap[M << 1];
    
    void ade(int a, int b, int c) {
        to[++ cnt] = b, cap[cnt] = c;
        nxt[cnt] = head[a], head[a] = cnt;
        to[++ cnt] = a, cap[cnt] = 0;
        nxt[cnt] = head[b], head[b] = cnt;
    }
    
    bool bfs(int s, int t) {
        memset(depth, 0, sizeof depth);
        queue<int> que;
        depth[s] = 1;
        que.push(s);
        while(!que.empty()) {
            int u = que.front(); que.pop();
            for(int i = head[u]; i; i = nxt[i]) {
                int v = to[i];
                if(!cap[i] || depth[v]) continue;
                depth[v] = depth[u] + 1;
                que.push(v);
            }
        }
        return depth[t];
    }
    
    //long long dfs(int s, int t, int flow) {
    //    if(s == t) return flow;
    //    long long ret = 0;
    //    for(int i = head[s]; flow && i; i = nxt[i]) {
    //        int v = to[i];
    //        if(depth[v] == depth[s] + 1 && cap[i]) {
    //            long long tmp = dfs(v, t, min(flow, cap[i]));
    //            cap[i] -= tmp;
    //            cap[i ^ 1] += tmp;
    //            flow -= tmp;
    //            ret += tmp;
    //        }
    //    }
    //    if(!ret) depth[s] = 0;
    //    return ret;
    //}
    
    long long dfs(int u, int t,int dist){
        if(u == t) return dist;
        for(int& i=cur[u];i;i=nxt[i]){
            if(depth[to[i]]==depth[u]+1&&cap[i]){
                long long tmp=dfs(to[i], t, min(dist,cap[i]));
                if(tmp>0){
                    cap[i]-=tmp;
                    cap[i^1]+=tmp;
                    return tmp;
                }
            }
        }
        return 0;
    }
    
    //
    //long long dinic(int s, int t) {
    //    long long ret = 0;
    //    while(bfs(s, t)){
    //        ret += dfs(s, t, inf);
    //    }
    //    return ret;
    //}
    
    long long dinic(int s, int t){
        long long res=0,d;
        while(bfs(s, t)){
            for(int i=0;i< 504;i++) cur[i]=head[i];
            while(d = dfs(s, t,inf)) res+=d;
        }
        return res;
    }
    
    int main() {
        int n, m, s, t, a, b, c, u, v;
        while(~scanf("%d %d", &n, &m)) {
            cnt = 1;
            memset(head, 0, sizeof head);
            s = n + 1, t = n + 2;
            long long sum = 0;
            while(m--) {
                scanf("%d%d%d%d%d", &u, &v, &a, &b, &c);
                ade(s, u, a + b);
                ade(s, v, a + b);
                ade(u, t, b + c);
                ade(v, t, b + c);
                ade(u, v, -2 * b + a + c);
                ade(v, u, -2 * b + a + c);
                sum += a + b + c;
            }
            long long ans = round((2.0 * sum - dinic(s, t)) / 2);
            printf("%lld\n",ans);
        }
        return 0;
    }
    
    

