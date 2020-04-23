## CCPC-Wannafly_Winter_Camp_Day2_(Div2,_onsite)_H_Cosmic_Cleaner_(计算几何)

2球相交 求体积和 很多人的板子应该都是从

> <https://blog.csdn.net/enterprise_/article/details/81624174>  
>  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408211221330.png?x-oss-
> process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
>  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408211243638.png?x-oss-
> process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)
    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    typedef unsigned long long ull;
    const int maxn = 6000+5;
    const int INF = 0x3f3f3f3f;
    const int cx[]= {0,0,1,-1};
    const int cy[]= {1,-1,0,0};
    const double eps= 1>>10;
    const int maxv = 100+5;
    const double PI=acos(-1.0);
    
    int n,m,k,t;
    
    struct node {
    	double x,y,z,r;
    } a[maxn];
    
    double dx,dy,dr,dz;
    
    typedef struct point {
    	double x,y,z;
    	point() {}
    	point(double a, double b,double c) {
    		x = a;
    		y = b;
    		z = c;
    	}
    	point operator -(const point &b)const {     //返回减去后的新点
    		return point(x - b.x, y - b.y,z-b.z);
    	}
    	point operator +(const point &b)const {     //返回加上后的新点
    		return point(x + b.x, y + b.y,z+b.z);
    	}
    	//数乘计算
    	point operator *(const double &k)const {    //返回相乘后的新点
    		return point(x * k, y * k,z*k);
    	}
    	point operator /(const double &k)const {    //返回相除后的新点
    		return point(x / k, y / k,z/k);
    	}
    	double operator *(const point &b)const {    //点乘
    		return x*b.x + y*b.y+z*b.z;
    	}
    } point;
    
    double dist(point p1, point p2) {       //返回平面上两点距离
    	return sqrt((p1 - p2)*(p1 - p2));
    }
    
    typedef struct sphere {//球
    	double r;
    	point centre;
    } sphere;
    
    double res(sphere a, sphere b) {
    	double v=0;
    	double d = dist(a.centre, b.centre);//球心距
    	double t = (d*d + a.r*a.r - b.r*b.r) / (2.0 * d);//
    	double h = sqrt((a.r*a.r) - (t*t)) * 2;//h1=h2，球冠的高
    	double angle_a = 2 * acos((a.r*a.r + d*d - b.r*b.r) / (2.0 * a.r*d));  //余弦公式计算r1对应圆心角，弧度
    	double angle_b = 2 * acos((b.r*b.r + d*d - a.r*a.r) / (2.0 * b.r*d));  //余弦公式计算r2对应圆心角，弧度
    	double l1 = ((a.r*a.r - b.r*b.r) / d + d) / 2;
    	double l2 = d - l1;
    	double x1 = a.r - l1, x2 = b.r - l2;//分别为两个球缺的高度
    	double v1 = PI*x1*x1*(a.r - x1 / 3);//相交部分r1圆所对应的球缺部分体积
    	double v2 = PI*x2*x2*(b.r - x2 / 3);//相交部分r2圆所对应的球缺部分体积
    	return v = v1 + v2;//相交部分体积
    }
    
    int main() {
    	cin>>t;
    	int cas=1;
    	while(t--) {
    		scanf("%d",&n);
    		for(int i=1; i<=n; i++) {
    			scanf("%lf %lf %lf %lf",&a[i].x,&a[i].y,&a[i].z,&a[i].r);
    		}
    		scanf("%lf %lf %lf %lf",&dx,&dy,&dz,&dr);
    		sphere tp,b;
    		b.centre.x=dx;
    		b.centre.y=dy;
    		b.centre.z=dz;
    		b.r=dr;
    		double V=0;
    		for(int i=1; i<=n; i++) {
    			double len=abs(sqrt((a[i].x-dx)*(a[i].x-dx)+(a[i].y-dy)*(a[i].y-dy)+(a[i].z-dz)*(a[i].z-dz)));
    			if(len>dr+a[i].r) continue;
    			else if(len<=dr-a[i].r) {
    				V+=4*PI*a[i].r*a[i].r*a[i].r/3;
    				continue;
    			} else {
    				tp.centre.x=a[i].x;
    				tp.centre.y=a[i].y;
    				tp.centre.z=a[i].z;
    				tp.r=a[i].r;
    				V+=res(tp,b);
    			}
    		}
    		//   cout<<V<<endl;
    		printf("Case #%d: %.9lf\n",cas++,V);
    	}
    	return 0;
    }
    

