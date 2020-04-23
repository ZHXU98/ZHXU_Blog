## 【map_priority_q】_Merge_Equals_CodeForces_-_962D

<http://codeforces.com/problemset/problem/962/D>  
维护一个数列 不出现重复数字 如果出现把valx2 放到重复出现位置 安输入顺序  
7  
3 4 1 2 2 1 1

4  
3 8 2 1  
按样例理解

[3,4,1,2,2,1,1]  
[3,4,1,2,2,1,1] → [3,4,2,2,2,1] → [3,4,4,2,1] → [3,8,2,1]

.  
1,map：输入时用 数组存数据 map记录位置 如果再次出现 就可以用map存的位置吧之前的那个数在数组更新成0 在释放这个数据 输出时判断下就好
然后把重复出现在数组原位置的x2 更新到map  
细节见代码  
2.用priority_queue 实现 友情链接 我是被告知map做才做出的。。。这里是同学用pq实现的  
<https://blog.csdn.net/qq_40508713/article/details/81913404>

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <queue>
    #include <stack>
    #include <map> 
    using namespace std;
    typedef long long ll;
    
    const int maxn=150000+5;
    
    map<ll,int> id;// ll不然wa
    ll a[maxn];
    
    int main(){
        int n;
        cin>>n;
        int b;
        memset(a,0,sizeof(a));
        int cnt=1;
        for(int i=1;i<=n;i++){
            cin>>a[cnt];
            if(!id[a[cnt]]){
                id[a[cnt]]=cnt;
                cnt++;
            }
            else{
                while(true){// 这里完成了对之后可能重复数据的处理
                    a[id[a[cnt]]]=0;
                    id.erase(a[cnt]);// 释放了就方便统计个数 map只要出现就会被统计 所以释放方便之后输出
                //  id[a[cnt]]=0; 这里试了下
                    a[cnt]=a[cnt]*2;
                    if(!id[a[cnt]]){
                        id[a[cnt]]=cnt;
                        break;
                    }
                }   
                cnt++;
            }
        }
        cout<<id.size()<<endl;
        int f=0;
        for(int i=0;i<cnt;i++){
        //  cout<<"     "<<id[a[i]]<<   "   "<<a[i] <<endl;
            if(f&&id[a[i]]) cout<<" ";
            if(id[a[i]]){
                f=1;
                cout<<a[i];
            }
        }cout<<endl;
        return 0;
    }

