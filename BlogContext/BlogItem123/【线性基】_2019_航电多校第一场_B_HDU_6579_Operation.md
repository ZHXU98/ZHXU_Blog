## 【线性基】_2019_航电多校第一场_B_HDU_6579_Operation

![唉 菜](https://img-blog.csdnimg.cn/20190724194937814.png)  
There is an integer sequence a of length n and there are two kinds of
operations:  
0 l r: select some numbers from al…ar so that their xor sum is maximum, and
print the maximum value.

1 x: append x to the end of the sequence and let n=n+1.

0 l r 要和之前的ans 异或%m + 1  
1 x 输出 之前的 ans ^ 现在的答案

强制在线了 难题

> 1、线性基：  
>  若干数的线性基是一组数a1,a2,…an，其中ax的最高位的1在第x位。  
>  通过线性基中元素xor出的数的值域与原来的数xor出数的值域相同。  
>  2、线性基的构造法：  
>  对每一个数p从高位到低位扫，扫到第x位为1时，若ax不存在，则ax=p并结束此数的扫描，否则令p=p xor ax。  
>  3、查询：  
>  用线性基求这组数xor出的最大值：从高往低扫ax，若异或上ax使答案变大，则异或。  
>  4、判断：  
>
> 用线性基求一个数能否被xor出：从高到低，对该数每个是1的位置x，将这个数异或上ax（注意异或后这个数为1的位置和原数就不一样了），若最终变为0，则可被异或出。当然需要特判0（在构造过程中看是否有p变为0即可）。例子：(11111,10001)的线性基是a5=11111，a4=01110，要判断11111能否被xor出，11111
> xor a5=0，则这个数后来就没有是1的位置了，最终得到结果为0，说明11111能被xor出。  
>  
>  第k小异或和啊 其实就是 k 按二进制 去异或这个矩阵的值 看 0 决定时候 k + 1 处理  
>  k大 2 ^ (r 线性基的秩) - k +( 1 看实际情况)；

这题 上三角矩阵  
考虑放高位的时候尽可能屯(优先)靠后面的 贪心 所以 查的时候位置 > l

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    const int maxn = 5*1e5 + 5;
    int p[maxn][32], pos[maxn][32];
    
    /*线性基//////////*
    bool insert123321(LL val){
        for(int i = 63; i >= 0; i --){
            if(val &(1LL << i)){
                if(!b[i]){
                    b[i] = val;
                    break;
                }
               val ^= b[i];
            }
        }
        return val > 0;
    }
    */
    //////////// 前缀线性基
    void Insert(int x,int val) {
    	for(int i=31; i>=0; i--) { //前缀线性基
    		p[x][i]=p[x-1][i];
    		pos[x][i]=pos[x-1][i];
    	}
    	int temp=x;
    	for(int i=31; i>=0; i--) {
    		if((val>>i)&1) {
    			if(!p[x][i]) {
    				p[x][i]=val;
    				pos[x][i]=temp;
    				break;
    			}
    			/////// 带上三角矩阵
    			if(pos[x][i]<temp) { //尽量将编号为x,temp的放到最高位
    				swap(p[x][i],val);
    				swap(pos[x][i],temp);
    			}
    			val^=p[x][i];
    			/////////
    		}
    	}
    	// return val > 0;
    }
    
    int main() {
    	int t;
    	scanf("%d",&t);
    	while(t--) {
    		int n, m;
    		memset(p,0,sizeof(p));
    		memset(pos,0,sizeof(pos));
    		scanf("%d %d",&n, &m);
    		for(int i = 1, a; i <= n; i ++) {
    			scanf("%d",&a);
    			Insert(i, a);
    		}
    		int last = 0, a;
    		while(m -- ) {
    			int cmd;
    			scanf("%d",&cmd);
    			if(cmd) {
    				scanf("%d",&a);
    				n ++;
    				a ^= last;
    				Insert(n, a);
    			} else {
    				int l, r;
    				scanf("%d %d",&l,&r);
    				l = (l ^ last) % n + 1;
    				r = (r ^ last) % n + 1;
    				if(l > r) swap(l, r);
    				int ans = 0;
    				for(int i = 31; i >= 0; i--) {
    					if(p[r][i] &&pos[r][i] >= l) {
    						ans = max(ans, ans ^ p[r][i]);
    					}
    				}
    				printf("%d\n",ans);
    				last = ans;
    			}
    		}
    	}
    }
    

