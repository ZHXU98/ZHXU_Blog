## 【几何数学】_Intersection_HDU_-_5120_2014_北京ICPC

Matt is a big fan of logo design. Recently he falls in love with logo made up
by rings. The following figures are some famous examples you may know.

A ring is a 2-D figure bounded by two circles sharing the common center. The
radius for these circles are denoted by r and R (r < R). For more details,
refer to the gray part in the illustration below.

Matt just designed a new logo consisting of two rings with the same size in
the 2-D plane. For his interests, Matt would like to know the area of the
intersection of these two rings.  
Input  
The first line contains only one integer T (T ≤ 10 5), which indicates the
number of test cases. For each test case, the first line contains two integers
r, R (0 ≤ r < R ≤ 10).

Each of the following two lines contains two integers x i, y i (0 ≤ x i, y i ≤
20) indicating the coordinates of the center of each ring.  
Output  
For each test case, output a single line “Case #x: y”, where x is the case
number (starting from 1) and y is the area of intersection rounded to 6
decimal places.

Sample Input  
2  
2 3  
0 0  
0 0  
2 3  
0 0  
5 0  
Sample Output  
Case #1: 15.707963  
Case #2: 2.250778

给了2个半径。。。 然后2行圆心坐标  
求相交圆环的面积

纯粹数学题

打错一个字母wa了 3发 醉了醉了  
代码丑 见谅 懒得改了

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 1<<20;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1);
    
    int t,n,m;
    double x1,yy1,x2,yy2;
    double r1,r2;
    
    double ras(int x1,int yy1,int x2,int yy2,int r1,int r2) {
        double S;
        double D=sqrt((x1-x2)*(x1-x2)+(yy1-yy2)*(yy1-yy2));
    
        double R=max(r1,r2);
        double r=min(r1,r2);
    
        if(D>=r1+r2) return 0;
        if(D<=R-r) return r*r*PI;
    
        double Angle_a=acos((D*D+R*R-r*r)/(2*D*R));
    
        double Sjia_a=sin(Angle_a)*R*R*cos(Angle_a);
        double Ssan_a=R*R*Angle_a;
        double Angle_b=acos((D*D+r*r-R*R)/(2*D*r));
        double Sjia_b=sin(Angle_b)*r*r*cos(Angle_b);
        double Ssan_b=r*r*Angle_b;
        return Ssan_a-Sjia_a+Ssan_b-Sjia_b;
    }
    
    int main() {
        scanf("%d",&t);
        int cas=1;
        while(t--) {
            scanf("%lf %lf",&r1,&r2);
            scanf("%lf %lf",&x1,&yy1);
            scanf("%lf %lf",&x2,&yy2);
    
            double ans=0;
    
            ans+=ras(x1,yy1,x2,yy2,r1,r1);
    
            ans-=ras(x1,yy1,x2,yy2,r1,r2);
            ans-=ras(x1,yy1,x2,yy2,r1,r2);
    
            ans+=ras(x1,yy1,x2,yy2,r2,r2);
    
            double D=sqrt((x1-x2)*(x1-x2)+(yy1-yy2)*(yy1-yy2));
            double R=max(r1,r2);
            double r=min(r1,r2);
    
            if(D==0) ans=R*R*PI-r*r*PI;
    
            printf("Case #%d: %lf\n",cas++,ans);
        }
        return 0;
    }

