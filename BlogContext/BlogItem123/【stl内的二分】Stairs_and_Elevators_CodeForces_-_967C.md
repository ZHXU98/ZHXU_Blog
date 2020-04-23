## 【stl内的二分】Stairs_and_Elevators_CodeForces_-_967C

题意：n*m的楼房，cl个楼梯，ce个电梯，除了电梯的最大速度是v外，其他速度都是1。给出q次询问，回答(x1, y1)到(x2,
y2)，最少需要多少时间？  
顺序输入（有小到大）；

简短的。。。。。

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <vector>
    #include <queue>
    #include <cstring>
    using namespace std;
    
    const int maxn = 1e5+5;
    int l[maxn];
    int e[maxn];
    
    
    int main(){
    int n,m,ce,cl,q,x1,x2,yy1,yy2,v,ans;
        while(cin>>n>>m>>cl>>ce>>v){
    
            for(int i=0;i<cl;i++) cin>>l[i];
            for(int i=0;i<ce;i++) cin>>e[i];
    
            cin>>q;
            while(q--){
                ans=0x3f3f3f3f;
                cin>>yy1>>x1>>yy2>>x2;
    
                if(yy1==yy2){cout<<abs(x1-x2)<<endl;continue;}
    
                int lt=lower_bound(l,l+cl,x1)-l;
                int dt=lower_bound(e,e+ce,x1)-e;
    
                if(lt<cl) ans=min(ans,abs(l[lt]-x1)+abs(l[lt]-x2)+abs(yy2-yy1));
                if(lt>0) ans=min(ans,abs(l[lt-1]-x1)+abs(l[lt-1]-x2)+abs(yy2-yy1));
    
                int sx=(abs(yy1-yy2)+v-1)/v;
    
                if(dt<ce) ans=min(ans,(int)(abs(e[dt]-x1)+abs(e[dt]-x2)+sx));
                if(dt>0)  ans=min(ans,(int)(abs(e[dt-1]-x1)+abs(e[dt-1]-x2)+sx));
    
                printf("%d\n",ans);
            }
        }
        return 0;
    }
    

我最开始写的 orz（脑子抽了写这干什么）。。。。。

    
    
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
    
    const int INF=0x3f3f3f3f;
    const int maxn=1e5+5;
    int dianti[maxn];
    int louti[maxn];
    int n,m,c1,c2,v,q;
    
    int main(){
        while(cin>>n>>m>>c1>>c2>>v){
        //  memset(dianti)
            for(int i=0;i<c1;i++) scanf("%d",&louti[i]);
            for(int i=0;i<c2;i++) scanf("%d",&dianti[i]);
            scanf("%d",&q);
            int x1,x2,y1,y2;
            for(int i=0;i<q;i++){
                cin>>y1>>x1>>y2>>x2;
                if(y1==y2){cout<<abs(x2-x1)<<endl;continue;}
                int ans1=INF,ans2=INF;
                if(c1!=0){
                    int louti_r_pos=upper_bound(louti,louti+c1,x1)-louti;
                    int louti_l_pos=-1;
    
                    int louti_r;
                    if(louti_r_pos==c1){
                        louti_r=louti[louti_r_pos-1];
                        louti_r_pos--;
                    }
                    else{
                        louti_r=louti[louti_r_pos];
                    }
                    if(louti_r_pos-1>=0) louti_l_pos=louti_r_pos-1;
                    int louti_l=-1;
                    if(louti_l_pos>=0) louti_l=louti[louti_l_pos];
    
                    int louti_y_time=abs((y2-y1));
    
                    int louti_rx_time=abs(x1-louti_r);
                    int louti_lx_time=louti_l!=-1?abs(x1-louti_l):INF;
    
                    int louti_r_tsum=louti_y_time+louti_rx_time+abs(x2-louti_r);
                    int louti_l_tsum=louti_y_time+louti_lx_time+abs(x2-louti_l);
    
                    ans1=min(louti_r_tsum,louti_l_tsum);
                //  cout<<"    "<<ans1<<endl;
                }
                if(c2!=0){
                    int dianti_r_pos=upper_bound(dianti,dianti+c2,x1)-dianti;
    
                    int dianti_l_pos=-1;
    
                    int dianti_r;
                    if(dianti_r_pos==c2){
                        dianti_r=dianti[dianti_r_pos-1];
                        dianti_r_pos--;
                    }
                    else{
                        dianti_r=dianti[dianti_r_pos];
                    }
    
                    if(dianti_r_pos-1>=0) dianti_l_pos=dianti_r_pos-1;
                    int dianti_l=-1;    
                    if(dianti_l_pos>=0) dianti_l=dianti[dianti_l_pos];  
    
                    int dianti_rx_time=abs(x1-dianti_r);
                    int dianti_lx_time=dianti_l_pos!=-1?abs(x1-dianti_l):INF;
    
                    int dianti_y_time=ceil(abs(1.0*y1-1.0*y2)/v);
    
                    int dianti_r_tsum=dianti_rx_time+dianti_y_time+abs(x2-dianti_r);
                    int dianti_l_tsum=dianti_lx_time+dianti_y_time+abs(x2-dianti_l);
    
                    ans2=min(dianti_r_tsum,dianti_l_tsum);
                //  cout<<"     "<<ans2<<endl;
                }           
                printf("%d\n",(min(ans1,ans2)));
            }
        }
        return 0;
    }
    

