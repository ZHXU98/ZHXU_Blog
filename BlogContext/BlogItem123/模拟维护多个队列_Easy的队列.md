## 模拟维护多个队列_Easy的队列

1239: Easy的队列  
时间限制: 2 Sec 内存限制: 128 MB

题目描述  
Easy的学生的《数据结构》考试挂掉了，他求着Easy再给他一次机会，Easy出了这么一道题，如果他的学生做出来就给这个学生一次重新考试的机会，题目是这样的：  
维护一个名为队列的数据结构，支持以下四种操作：  
1\. ENQUEUE x  
将值为x的元素放入队列的尾部  
2\. DEQUEUE  
输出当前队列首部的元素之后删除队列首部的元素  
3\. MAX  
输出当前队列中的最大值，若队列为空则输出”EMPTY!”（没有引号）  
4\. MIN  
输出当前队列中的最小值，若队列为空则输出”EMPTY!”（没有引号）

输入  
包含多组测试数据  
每组测试数据以一个整数N作为开始（0<=N<=500000)  
当1<=N<=500000时，意味着会有N次队列操作，当N为0时意味着输入数据结束  
如果1<=N<=500000之后会有N行,每行会有一条队列操作命令，分别是  
1.ENQUEUE x  
将值为x的元素放入队列的尾部  
2.DEQUEUE  
输出当前队列首部的元素之后删除队列首部的元素  
3.MAX  
输出当前队列中的最大值，若队列为空则输出”EMPTY!”（没有引号）  
4.MIN  
输出当前队列中的最小值，若队列为空则输出”EMPTY!”（没有引号）  
请按命令进行相关队列操作或者输出

输出  
对于每一组测试数据，第一行输出”Case X:”表示第X组数据(没有引号)之后  
对每一个要求输出的操作进行输出，一个操作的输出占一行  
样例输入  
3  
ENQUEUE 1  
MAX  
DEQUEUE  
5  
ENQUEUE 2  
MAX  
DEQUEUE  
MIN  
DEQUEUE  
0  
样例输出  
Case 1:  
1  
1  
Case 2:  
2  
2  
EMPTY!  
EMPTY!

单调栈+list

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <string>
    #include <queue>
    #include <stack>
    #include <set>
    #include <ctime>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    
    int main(){ 
    //int t=time(0);
    std::ios::sync_with_stdio(false);
    //freopen("1.in","r",stdin); 
    //freopen("s1.txt","w+",stdout); 
        int n;
        int x;int ts=0;
        while(cin>>n&&n){
            char cmd[20]; 
            printf("Case %d:\n",++ts); 
    list<int> G;
    list<int> G1;
    list<int> G2; 
            for(int i=0;i<n;i++){
                cin>>cmd;
                if(cmd[0]=='E'){
                    cin>>x;
                    G.push_back(x);
                    while(G1.size()&&G1.back()>x){
                        G1.pop_back();
                    }
                    G1.push_back(x);
                    while(G2.size()&&G2.back()<x){
                        G2.pop_back();
                    }
                    G2.push_back(x);
                }
                else if(cmd[0]=='D'){
                    if(G.size()==0){
                        printf("EMPTY!\n");
                    }
                    else{
                        int tp=G.front();
                        if(G1.size()&&tp==G1.front()){
                            G1.pop_front();
                        }
                        if(G2.size()&&tp==G2.front()){
                            G2.pop_front(); 
                        }
                    printf("%d\n",G.front());
                    G.pop_front(); 
                    }
                }
                if(cmd[1]=='A'){
                    if(!G2.size()) printf("EMPTY!\n");
                    else printf("%d\n",G2.front());
                }
                else if(cmd[1]=='I'){
                    if(!G1.size()) printf("EMPTY!\n");
                    else printf("%d\n",G1.front());
                }
            }
        }   
    //  cout<<time(0)-t;           
        return 0;
    }
    

在附带一个 只不过是 双向队列维护的（同学写的）

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <string>
    #include <queue>
    #include <stack>
    #include <set>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    int main( )
    {
        std::ios::sync_with_stdio(false);
        int n;
        int k = 1;
        while(cin >> n&&n){
            printf("Case %d:\n",k++);
            deque<int> a,b,c;
            char  s[20];
            int x;
            while(n--){
                cin >> s;
                if(s[0] == 'E'){
                    cin>>x;
                    a.push_back(x);
                    while(b.size() && b.back()<x){
                        b.pop_back();
                    }
                    b.push_back(x);
                    while(c.size() && c.back()>x){
                        c.pop_back();
                    }
                    c.push_back(x);
                }
                else if(s[0] == 'D'){
                    if(!a.size())  printf("EMPTY!\n");
                    else{
                        if(b.size() && b.front() == a.front()) b.pop_front();
                        if(c.size() && c.front() == a.front()) c.pop_front();
                        printf("%d\n",a.front());
    
                        a.pop_front();
                    }
                }
                else{
                        if(s[1] == 'A'){
                            if(!b.size())  printf("EMPTY!\n");
                            else printf("%d\n",b.front());
    
                        }
                        else{
                            if(!c.size())  printf("EMPTY!\n");
                            else printf("%d\n",c.front());
    
                        }
                }
            }
        }
        return 0;
    }
    

另外附一个 暴力过了数据的。。。。没有tle

    
    
    #include<iostream>
    #include<cstdio>
    #include<algorithm>
    #include<cstring>
    #include<queue> 
    #include<set>
    #include<vector>
    using namespace std;
    int main()
    {
        int t=1;
        while(1)
        {
            int n;
            scanf("%d",&n);
            if(n==0) break;
            printf("Case %d:\n",t);
            t++;
            queue<int> q;
            vector<int> maxx;
            vector<int> minn;
            vector<int>::iterator startIterator;
            vector<int>::iterator tempIterator;
            while(n--)
            {
                char s[10];
                scanf("%s",s);
                if(s[0]=='E')
                {
                    int x;
                    scanf("%d",&x);
                    if(q.empty())
                    {
                        maxx.push_back(x);
                        minn.push_back(x);
                    }
                    else
                    {
                        while(1)
                        {
                            if(maxx.empty()||maxx.back()>x) break;
                            else maxx.pop_back();   
                        }
                        maxx.push_back(x);
                        while(1)
                        {
                            if(minn.empty()||minn.back()<x) break;
                            else minn.pop_back();   
                        }
                        minn.push_back(x);
                    }
                    q.push(x);
                }
                else if(s[0]=='D')
                {
                    if(!q.empty())
                    {
                        printf("%d\n",q.front());
                        for( tempIterator = maxx.begin(); tempIterator != maxx.end(); tempIterator++ )
                        {
                            if(*tempIterator==q.front()) break;
                        }
                        if(tempIterator != maxx.end()) maxx.erase( tempIterator);
    
                        for( tempIterator = minn.begin(); tempIterator != minn.end(); tempIterator++ )
                        {
                            if(*tempIterator==q.front()) break;
                        }
                        if(tempIterator != minn.end()) minn.erase( tempIterator);
                        q.pop();
                    }
                    else 
                    {
                        printf("EMPTY!\n");
                    }
                }
                else if(s[1]=='A')
                {
                    if(!q.empty()) 
                    {
                        printf("%d\n",maxx.front());
                    }
                    else 
                    {
                        printf("EMPTY!\n");
                    }
                }
                else if(s[1]=='I')
                {
                    if(!q.empty()) 
                    {
                        printf("%d\n",minn.front());
                    }
                    else 
                    {
                        printf("EMPTY!\n");
                    }
                }
            }
        }
        return 0;
    }
    

然后是我二分都没有过去的 tle

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <string>
    #include <queue>
    #include <stack>
    #include <set>
    using namespace std;
    typedef long long ll;
    
    vector<int> G;
    vector<int> G1;
    //vector<int> G2; 
    
    bool cmp(int a,int b){
        return a>b;
    }
    
    int main(){ 
    std::ios::sync_with_stdio(false);
        int n;
        int x;int ts=0;
        while(cin>>n&&n){
            char cmd[20]; 
            cout<<"Case "<<++ts<<":"<<endl; 
            G.clear();G1.clear();
            for(int i=0;i<n;i++){
                cin>>cmd;
                if(cmd[0]=='E'){
                    cin>>x;
                    G.push_back(x);
                    G1.insert(lower_bound(G1.begin(),G1.end(),x),x);
                }
                else if(cmd[0]=='D'){
                    if(G.size()==0){
                        cout<<"EMPTY!\n";
                    }
                    else{
                        cout<<G[0]<<endl;
                        G1.erase(lower_bound(G1.begin(),G1.end(),G[0]));
                        G.erase(G.begin());
                    }
                }
                if(cmd[1]=='A'){
                    if(G.size()==0) cout<<"EMPTY!\n";
                    else{
                        cout<<G1[G1.size()-1]<<endl;
                    }
                }
                else if(cmd[1]=='I'){
                    if(G.size()==0) cout<<"EMPTY!\n";
                    else{
                        cout<<G1[0]<<endl;
                    }
                }
            }
        }              
        return 0;
    }
    

