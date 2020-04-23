## 2018_PAT_CCCC【暂时弃坑_下次C4前补】

我补完 l1和l2 截至2018年所有题了orz l3做了快一般 现在看这个博客 只能说 弃坑了 (原来 19年更新算这博客19年发啊
我之前写了一篇口胡数据结构的博客 主要是关于 树 的
<https://blog.csdn.net/qq_40831340/article/details/88081617> 欢迎大家斧正)

这几天有时间 慢慢补完这次c4的题  
L1 后面2个测试点没有过  
考完意识到原来队伍数量是可以一样的 ：）  
L2 迷；

L3~L8  
6连-水题

    
    
    L1-003. 打折
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    去商场淘打折商品时，计算打折以后的价钱是件颇费脑子的事情。例如原价 ￥988，标明打 7 折，则折扣价应该是 ￥988 x 70% = ￥691.60。本题就请你写个程序替客户计算折扣价。 
    输入格式： 
    输入在一行中给出商品的原价（不超过1万元的正整数）和折扣（为[1, 9]区间内的整数），其间以空格分隔。 
    输出格式： 
    在一行中输出商品的折扣价，保留小数点后 2 位。 
    输入样例：
    988 7
    输出样例：
    691.60
    
    
    #include <iostream>
    #include <cstdio>
    using namespace std;
    
    int main(){
    	int n,m;
    	while(cin>>n>>m){
    		printf("%.2lf\n",1.0*n*m/10);
    	}
    	return 0;
    } 
    
    
    
    L1-004. 2018我们要赢
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    2018年天梯赛的注册邀请码是“2018wmyy”，意思就是“2018我们要赢”。本题就请你用汉语拼音输出这句话。 
    输入格式： 
    本题没有输入。 
    输出格式： 
    在第一行中输出：“2018”；第二行中输出：“wo3 men2 yao4 ying2 !”。 
    输入样例：
    本题没有输入。
    输出样例：
    2018
    wo3 men2 yao4 ying2 !
    
    
    
    #include <iostream>
    #include <cstdio>
    using namespace std;
    
    int main(){
    	printf("2018\nwo3 men2 yao4 ying2 !");
    	return 0;
    } 
    
    
    
    
    L1-005. 电子汪
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    据说汪星人的智商能达到人类4岁儿童的水平，更有些聪明汪会做加法计算。比如你在地上放两堆小球，分别有1只球和2只球，聪明汪就会用“汪！汪！汪！”表示1加2的结果是3。 
    本题要求你为电子宠物汪做一个模拟程序，根据电子眼识别出的两堆小球的个数，计算出和，并且用汪星人的叫声给出答案。 
    输入格式： 
    输入在一行中给出两个[1, 9]区间内的正整数A和B，用空格分隔。 
    输出格式： 
    在一行中输出A+B个“Wang!”。 
    输入样例：
    2 1
    输出样例：
    Wang!Wang!Wang!
    
    
    #include <iostream>
    #include <cstdio>
    using namespace std;
    
    int main(){
    	int n,m;
    	while(cin>>n>>m){
    		for(int i=0;i<n+m;i++){
    			cout<<"Wang!";
    		}
    		cout<<endl;
    	}
    	return 0;
    } 
    
    
    
    
    L1-006. 福到了
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    “福”字倒着贴，寓意“福到”。不论到底算不算民俗，本题且请你编写程序，把各种汉字倒过来输出。这里要处理的每个汉字是由一个 N x N 的网格组成的，网格中的元素或者为字符“@”或者为空格。而倒过来的汉字所用的字符由裁判指定。 
    输入格式： 
    输入在第一行中给出倒过来的汉字所用的字符、以及网格的规模 N （不超过100的正整数），其间以 1 个空格分隔；随后 N 行，每行给出 N 个字符，或者为“@”或者为空格。 
    输出格式： 
    输出倒置的网格，如样例所示。但是，如果这个字正过来倒过去是一样的，就先输出“bu yong dao le”，然后再用输入指定的字符将其输出。 
    输入样例 1：
    $ 9
     @  @@@@@
    @@@  @@@ 
     @   @ @ 
    @@@  @@@ 
    @@@ @@@@@
    @@@ @ @ @
    @@@ @@@@@
     @  @ @ @
     @  @@@@@
    输出样例 1：
    $$$$$  $ 
    $ $ $  $ 
    $$$$$ $$$
    $ $ $ $$$
    $$$$$ $$$
     $$$  $$$
     $ $   $ 
     $$$  $$$
    $$$$$  $ 
    输入样例 2：
    & 3
    @@@
     @ 
    @@@
    输出样例 2：
    bu yong dao le
    &&&
     & 
    &&&
    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    
    const int maxn = 105;
    
    char mp[maxn][maxn];
    char rmp[maxn][maxn];
    
    int main(){
    	int n;
    	char ch;
    	while(cin>>ch>>n){
    		getchar();			//迷 一般的存在 
    		int flag=1;
    		memset(mp,0,sizeof(mp));
    		memset(rmp,0,sizeof(rmp));
    		for(int i=0;i<n;i++){
    			gets(mp[i]);
    		}
    		for(int i=0;i<n;i++){
    			for(int j=0;j<n;j++){
    				if(mp[i][j]!=' '){
    					rmp[n-1-i][n-1-j]=ch;
    				}
    				else rmp[n-i-1][n-j-1]=' ';
    			}
    		}
    		for(int i=0;i<n&&flag;i++){
    			for(int j=0;j<n&&flag;j++){
    				if(mp[i][j]!=' '){
    					if(rmp[i][j]==' ')
    						flag=0;
    						break;
    				}
    			}
    		}
    		if(flag) cout<<"bu yong dao le\n";
    		for(int i=0;i<n;i++){
    			cout<<rmp[i]<<endl;
    		}
    	}
    	return 0;
    } 
    
    
    
    
    L1-007. 谁是赢家
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    某电视台的娱乐节目有个表演评审环节，每次安排两位艺人表演，他们的胜负由观众投票和3名评委投票两部分共同决定。规则为：如果一位艺人的观众票数高，且得到至少1名评委的认可，该艺人就胜出；或艺人的观众票数低，但得到全部评委的认可，也可以胜出。节目保证投票的观众人数为奇数，所以不存在平票的情况。本题就请你用程序判断谁是赢家。 
    输入格式： 
    输入第一行给出 2 个不超过 1000 的正整数 Pa 和 Pb，分别是艺人 a 和艺人 b 得到的观众票数。题目保证这两个数字不相等。随后第二行给出 3 名评委的投票结果。数字 0 代表投票给 a，数字 1 代表投票给 b，其间以一个空格分隔。 
    输出格式： 
    按以下格式输出赢家： 
    The winner is x: P1 + P2 
    其中 x 是代表赢家的字母，P1 是赢家得到的观众票数，P2 是赢家得到的评委票数。 
    输入样例：
    327 129
    1 0 1
    输出样例：
    The winner is a: 327 + 1
    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    
    int a[5];
    int b[5];
    
    int main(){
    	int n;
    	while(cin>>a[0]>>a[1]){
    	//	cout<<a[0]<<"  "<<a[1]<<endl;
    		memset(b,0,sizeof(b));
    		for(int i=0;i<3;i++){
    			cin>>n;
    			b[n]++;
    		}
    	//	cout<<b[0]<<" b  "<<b[1]<<endl;
    		//cout<<a[0]<<" a  "<<a[1]<<endl;
    		if(b[0]==3){
    			printf("The winner is a: %d + %d\n",a[0],b[0]);
    		}
    		else if(b[1]==3){
    			printf("The winner is b: %d + %d\n",a[1],b[1]);
    		}
    		else if(a[0]+b[0]<a[1]+b[1]){
    			printf("The winner is b: %d + %d\n",a[1],b[1]);
    		}
    		else{
    			printf("The winner is a: %d + %d\n",a[0],b[0]);
    		}
    	}
    	return 0;
    } 
    
    
    
    L1-008. 猜数字
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    一群人坐在一起，每人猜一个 100 以内的数，谁的数字最接近大家平均数的一半就赢。本题就要求你找出其中的赢家。 
    输入格式： 
    输入在第一行给出一个正整数N（<= 104）。随后 N 行，每行给出一个玩家的名字（由不超过8个英文字母组成的字符串）和其猜的正整数（<= 100）。 
    输出格式： 
    在一行中顺序输出：大家平均数的一半（只输出整数部分）、赢家的名字，其间以空格分隔。题目保证赢家是唯一的。 
    输入样例：
    7
    Bob 35
    Amy 28
    James 98
    Alice 11
    Jack 45
    Smith 33
    Chris 62
    输出样例：
    22 Amy
    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    using namespace std;
    
    const int maxn = 10005;
    
    struct p{
    	int c;
    	string name;
    	int cj;
    }s[maxn];
    
    bool cmp(p x,p y){
    	return x.cj<y.cj;
    }
    
    int main(){
    	int n;
    	while(cin>>n){
    		int sum=0;
    		for(int i=0;i<n;i++){
    			cin>>s[i].name>>s[i].c;
    			sum+=s[i].c;
    		}
    		int ping=sum/n;
    		ping/=2;
    		for(int i=0;i<n;i++){
    			s[i].cj=abs(ping-s[i].c);
    		} 
    		sort(s,s+n,cmp);
    		cout<<ping<<" "<<s[0].name<<endl;
    	}
    	return 0;
    } 
    
    
    
    L2-002. 小字辈
    时间限制 
    400 ms
    内存限制 
    65536 kB
    代码长度限制 
    8000 B
    判题程序 
    Standard 
    作者 
    陈越
    本题给定一个庞大家族的家谱，要请你给出最小一辈的名单。 
    输入格式： 
    输入在第一行给出家族人口总数 N（不超过 100 000 的正整数） —— 简单起见，我们把家族成员从 1 到 N 编号。随后第二行给出 N 个编号，其中第 i 个编号对应第 i 位成员的父/母。家谱中辈分最高的老祖宗对应的父/母编号为 -1。一行中的数字间以空格分隔。 
    输出格式： 
    首先输出最小的辈分（老祖宗的辈分为 1，以下逐级递增）。然后在第二行按递增顺序输出辈分最小的成员的编号。编号间以一个空格分隔，行首尾不得有多余空格。 
    输入样例：
    9
    2 6 5 5 -1 5 6 4 7
    输出样例：
    4
    1 9
    

比赛的时候 没有记忆化 全超时…看开了  
非AC代码 测试点6 未过

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    using namespace std;
    
    const int maxn = 100050;
    int a[maxn];
    int b[maxn];
    int an[maxn];
    
    int finds(int x){
    	if(x==-1||b[x]!=0){
    		return b[x];
    	}
    	else {
    	//	cout<<x<<"  "<<finds(a[x])<<"  "<<a[x]<<endl;
    		return b[x]=finds(a[x])+1;
    	}
    }
    
    int main(){
    	int n;
    	ios::sync_with_stdio(false);
    	while(cin>>n){
    		for(int i=1;i<=n;i++){
    			cin>>a[i];
    			b[i]=0;
    			an[i]=0;
    		}
    		for(int i=1;i<=n;i++){
    			b[i]=finds(i);
    		//	cout<<"   "<<i<<"     "<<b[i]<<"   "<<endl;
    		}
    		int ans=-1,pos=0;
    		for(int i=1;i<=n;i++){
    			if(ans<=b[i]){
    				ans=b[i];
    			}
    		}
    		for(int i=1;i<=n;i++){
    			if(ans<=b[i]){
    				an[pos++]=i;
    			}
    		}
    		cout<<ans<<endl;
    		for(int i=0;i<pos;i++){
    			printf(i!=(pos-1)?"%d ":"%d\n",an[i]);
    		}
    	} 
    	return 0;
    } 
    

L2比赛时只拿了15 分 下这次重写莫名ac了 无奈…

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    using namespace std;
    
    const int maxn = 10005;
    
    struct stu{
    	string name;
    	int cj;
    }s[maxn];
    
    bool cmp(stu x,stu y){
    	if(x.cj==y.cj){
    		return x.name<y.name;
    	}
    	else return x.cj>y.cj;
    }
    
    int main(){
    	int n,g,k;
    	while(cin>>n>>g>>k){
    		int sum=0;
    		for(int i=0;i<n;i++){
    			cin>>s[i].name>>s[i].cj;
    			if(s[i].cj>=g){
    				sum+=50;
    			}
    			else if(s[i].cj>=60&&s[i].cj<g){
    				sum+=20;
    			}
    		}
    		cout<<sum<<endl;
    		sort(s,s+n,cmp);
    		int pos=1;
    		int tmp=s[0].cj;
    		int pw=0;
    		for(int i=0;i<n;i++){
    			if(tmp!=s[i].cj){
    				pos=i+1;
    				tmp=s[i].cj;
    				if(pw>=k){
    					break;
    				}
    			}
    			cout<<pos<<" "<<s[i].name<<" "<<s[i].cj<<endl;
    			pw++;
    		}
    	}
    	return 0;
    } 
    

