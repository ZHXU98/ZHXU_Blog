## 龙曲线_【分形】_从_DFS暴力_到_规律_实现

龙曲线  
龙曲线是以简单的数学规则画出一种曲线，它具有以下形态。曲线从一个简单的线段起始，按照一定规则变换此线段完成整个曲线。每形成一次变换称为“完成了一次变换代”，而每完成一代，曲线会进化到更复杂的形式。像这种“放大其一小部分的形状时，表现出与整个形状极为相似构造的图形”，就是分形。  
画出龙曲线的方法暂且就称为龙曲线字符串吧！龙曲线字符串由X、Y、F、+、-组成。  
那么，要画出龙曲线就从一个点起始画出如下曲线即可。

F：向前方移动一格并画线。  
+：向左旋转90度。  
-：向右旋转90度。  
X、Y：忽略。  
画出第0代龙曲线的字符串是FX。从下一代开始，按照如下方式利用前一代字符串进行字符替换，从而获得当前一代的龙曲线字符串。  
X-> X+YF  
Y-> FX+Y

根据上面的替换式，就有如下的1、2代龙曲线字符串。  
第一代：FX+YF  
第二代：FX+YF+FX-YF  
我们想要求出第n代龙曲线字符串。不过，考虑到答案有可能很长，所以只想计算出第p个字符起始长度为l个字符的字符串。请编写程序实现这种功能。

输入  
第一行输入测试用例的个数C（C<=50）。各测试用例的第一行分别输入3个整数，即龙曲线的世代n（0<=n<=50）、p以及l（1<=p<=1 000 000
000、1<=l<=50）。第n代龙曲线字符串的长度可假设成总是大于等于p+l的数值。  
输出  
每个测试用例在1行内输出第n代龙曲线字符串的第p个字符开始，输出l个字符。

示例输入  
4  
0 1 2  
1 1 5  
2 6 5  
42 764853475 30  
示例输出  
FX  
FX+YF  
+FX-Y  
FX-YF-FX+YF+FX-YF-FX+YF-FX-YF-  
  
首先是暴力 代码

    
    
    #include<iostream>
    using namespace std;
    
    void dfs(string str, int N) {
        if(N == 0) {
            cout << str;
            return;
        }
        for(int i = 0; i < str.size(); ++i) {
            if(str[i] == 'X')
                dfs("X+YF", N - 1);
            else if(str[i] == 'Y')
                dfs("FX-Y", N - 1);
            else
                cout << str[i];
        }
    }
    
    int main() {
        int n;
        cin >> n;
        dfs("FX", n);
    }
    

这里我们爆出 1 到 5 的数据表

    
    
    第一代：FX+YF // 5
    第二代：FX+YF+FX-YF // 11
    第三代：FX+YF+FX-YF+FX+YF-FX-YF // 23
    第四代：FX+YF+FX-YF+FX+YF-FX-YF+FX+YF+FX-YF-FX+YF-FX-YF // 47
    五:	    FX+YF+FX-YF+FX+YF-FX-YF+FX+YF+FX-YF-FX+YF-FX-YF  \
    		+FX+YF+FX-YF+FX+YF-FX-YF-FX+YF+FX-YF-FX+YF-FX-YF
    

字符数：sum[i] = (sum[i-1] << 1) + 1;

字母规律：循环节 FXYFFXYF  
符号规律：

    
    
    1.    +
    2.    + + -
    3.    + + - + + - -
    4.    + + - + + - - + + + - + + - -
    5. 	  + + - + + - - + + + - + + - - + + + - + + - - + + + - + + - -
    

发现 每个字符 前后的空隙 轮着插 + -;  
所以 再倍增上一个的同时 把 + - 依次插入  
这也是为什么 字符数：sum[i] = (sum[i-1] << 1) + 1; 是这样规律

那么 我们选择 每次将 非 0， 2， 4 位置的F X Y以外 的下表 首先 / 3  
然后不断 / 2 只到达到奇数位  
例如 第 3 代

    
    
      1 2 3 4 5 6 7
      + + - + + - - 
      第一位 + 1 mod 4  = 2 :+
      第二位 / 2 之后 + 1 mod 4 = 2：+
      第三位 +1 mod 4 = 0 ：-
      第四位 / 2 / 2 + 1 mod 4 = 2 ：+
      第五位 + 1 mod 4 = 2 ： +
      第六位 / 2 + 1 mod 4 =  0 ： -
      第七位 + 1 mod 4 = 0 ： -
    
    	大体上是这样的一般规律 其实mod 2在搞规律也行 我找的时候当4的循环节了 同时mod 4也是因为我前面没有处理好
    
    
    
    #include<bits/stdc++.h>
    using namespace std;
    
    void get_c(int &x) {
        while(x % 2 == 0) {
            x /= 2;
        }
    } // 确定第 x 位 属于O, E层次
    
    void output(int x) {
        if(x % 6 == 2) // 循环节 2p
                cout << "X";
            else if(x % 6 == 4) // 循环节 3p
                cout << "Y";
            else if(x % 3 == 0) {
                x = x / 3;
                get_c(x); // 降 k
                if((x + 1) % 4 == 2) {
                    cout << "+";
                } else
                    cout << "-";
            } else
                cout << "F"; // 循环节 1p
    }
    
    void sol(int p, int l) {
        for(int i = 0; i < l; i++) {
            int k = p + i;
            output(k);
        }
        cout << endl;
    }
    
    int main() {
        int cas;
        for(cin >> cas; cas--;) {
            int n, p, l;
            cin >> n >> p >> l;
            sol(p, l);
        }
        return 0;
    }
    

