## 【BFS】_Beautiful_Now_HDU_-_6351(未完)

<http://acm.hdu.edu.cn/showproblem.php?pid=6351>  
输入 t k  
对于一个数据最多交换K次 找最大最小值  
5  
12 1  
213 2  
998244353 1  
998244353 2  
998244353 3  
12 21  
123 321  
298944353 998544323  
238944359 998544332  
233944859 998544332  
注意0的存在；  
思路 找最小的最大的 交换 对重复的 都压入队列 BFS找最小的  
（贪心的话 就不太可能在多个最大最小中找到该换的 所以考虑BFS把所有的都试一试搞定）  
DFS之后补下  
不过BFS相当快

    
    
    #include <iostream>
    #include <cstdio>
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
    #include <bitset>
    using namespace std;
    typedef long long ll;
    
    int n,k,m,t;
    string s,s1,s2;
    
    struct node{
        string tmp;
        int st;
        int sp;
    };
    
    string bfs1(){
        queue<node> q;
        q.push(node{s,0,0});
        string res="999999999999";
        while(!q.empty()){
            node p=q.front();q.pop();
            res=min(res,p.tmp);
            if(p.sp==k) continue;
            if(p.st==s.size()-1) continue;
            string tmp=p.tmp;
            int pos=p.st;
            for(int i=p.st+1;i<tmp.size();i++){ 
                if(p.st==0&&tmp[i]=='0') continue;//去0
                if(tmp[i]<tmp[pos]){//huan xiao de
                    pos=i;
                }   
            }
            if(pos==p.st){
                q.push(node{p.tmp,p.st+1,p.sp});
            }
            else{
                 for(int i=p.st;i<s.size();i++){
                    if(tmp[i]==tmp[pos]){
                        swap(tmp[i],tmp[p.st]);
                        q.push(node{tmp,p.st+1,p.sp+1});
                        swap(tmp[i],tmp[p.st]);
                    }
                }
            }
        }
        return res;
    }
    
    string bfs2(){
        queue<node> q;
        q.push(node{s,0,0});
        string res="0";
        while(!q.empty()){
            node p=q.front();q.pop();
        //  cout<<p.tmp<<endl;
            res=max(res,p.tmp);
            if(p.sp==k) continue;
            if(p.st==s.size()-1) continue;
        //  if(res>=p.tmp) continue;
            string tmp=p.tmp;
            int pos=p.st;
            for(int i=p.st+1;i<tmp.size();i++){
    
                if(tmp[i]>tmp[pos]){//huan da de
                    pos=i;
                }   
            }
            if(pos==p.st){
                q.push(node{p.tmp,p.st+1,p.sp});
            }
            else{
                 for(int i=p.st;i<s.size();i++){
    
                    if(tmp[i]==tmp[pos]){
                        swap(tmp[i],tmp[p.st]);
                        q.push(node{tmp,p.st+1,p.sp+1});
                        swap(tmp[i],tmp[p.st]);
                    }
    
                }
            }
        }
        return res;
    }
    
    int main(){
        ios::sync_with_stdio(false);
        cin>>t;
        while(t--){
            cin>>s>>k;
            k=min(k,(int)s.size()-1);
            s1=bfs1();//da
            s2=bfs2();//xiao
            cout<<s1<<" "<<s2<<endl; 
        }
        return 0; 
    }

