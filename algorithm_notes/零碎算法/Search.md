# 搜索问题

**dfs使用递归和栈，bfs使用队列**

**1. dfs判断结束使用最后到达某个状态return bfs查看队列是否为空或者某种条件主动退出**

**2. 最短路径用bfs，求次数用dfs**

**3. bfs一般还有一个vis判断，dfs一般没有**

**bfs是逐层扩展的，所以第一个返回的一定是最短路径，dfs找到一条还必须判断是不是最短，也就是说等所有dfs完才能输出**




## dfs

*例题一：迷宫问题：*

*下图给出了一个迷宫的平面图，其中标记为1的为障碍，标记为0的为可以通行的地方。*
```
010000
000100
001001
110000
```
*迷宫的入口为左上角，出口为右下角，在迷宫中，只能从一个位置走到这 个它的上、下、左、右四个方向之一。对于上面的迷宫，从入口开始，可以按 DRRURRDDDR 的顺序通过迷宫， 一共 10 步。其中 D、U、L、R 分别表示向下、向上、向左、向右走。 对于下面这个更复杂的迷宫，请找出一种通过迷宫的方式，其使用的步数最少，在步数最少的前提下，请找出字典序最小的一个作为答案。*
*请注意在字典序中 D<L<R<U*

```
#include<iostream>
#include<string>
#include<algorithm>
#define maxn 50

using namespace std;

const int dirx[]={1,0,0,-1};
const int diry[]={0,-1,1,0};
const char dir[]={'D','L','R','U'};

int best=1<<28;         //先设置一个很大的数
const int row=30,col=50;//注意一定使用const
char a[row*col+5];      //用来存放对应位移动符号
int mins[maxn][maxn];   //存放对应位置最小值
string ans;             //最后输出移动结果

//有两种表示方式，一种是二维字符数组（，一种是string
//注意string判断是否是1要用maze[x][y]=='1'判断！

string maze[30]={
"01010101001011001001010110010110100100001000101010",
"00001000100000101010010000100000001001100110100101",
"01111011010010001000001101001011100011000000010000",
"01000000001010100011010000101000001010101011001011",
"00011111000000101000010010100010100000101100000000",
"11001000110101000010101100011010011010101011110111",
"00011011010101001001001010000001000101001110000000",
"10100000101000100110101010111110011000010000111010",
"00111000001010100001100010000001000101001100001001",
"11000110100001110010001001010101010101010001101000",
"00010000100100000101001010101110100010101010000101",
"11100100101001001000010000010101010100100100010100",
"00000010000000101011001111010001100000101010100011",
"10101010011100001000011000010110011110110100001000",
"10101010100001101010100101000010100000111011101001",
"10000000101100010000101100101101001011100000000100",
"10101001000000010100100001000100000100011110101001",
"00101001010101101001010100011010101101110000110101",
"11001010000100001100000010100101000001000111000010",
"00001000110000110101101000000100101001001000011101",
"10100101000101000000001110110010110101101010100001",
"00101000010000110101010000100010001001000100010101",
"10100001000110010001000010101001010101011111010010",
"00000100101000000110010100101001000001000000000010",
"11010000001001110111001001000011101001011011101000",
"00000110100010001000100000001000011101000000110011",
"10101000101000100010001111100010101001010000001000",
"10000010100101001010110000000100101010001011101000",
"00111100001000010000000110111000000001000000001011",
"10000001100111010111010001000110111010101101111000"
};

void dfs(int x,int y,int pos)
{
    if(pos>best){
        //剪枝
        return;
    }
    if(x==row-1&&y==col-1){
        //正常结束
        string temp;
        for(int i=1;i<pos;i++){
            temp+=a[i];
        }
        if(pos<best){
            ans=temp;
            best=pos;
        }
        else if(pos==best&&temp<ans){
            ans=temp;
        }
        return;
    }
    for(int i=0;i<4;i++){
        int tox=x+dirx[i];
        int toy=y+diry[i];
        if(tox<0||tox>row-1||toy<0||toy>col-1||maze[tox][toy]=='1'||pos+1>mins[tox][toy]){
            continue;
        }
        maze[tox][toy]=1;
        mins[tox][toy]=pos+1;
        a[pos]=dir[i];
        dfs(tox,toy,pos+1);
        maze[tox][toy]=0;
    }
}

int main()
{
    for(int i=0;i<row;i++){
        for(int j=0;j<col;j++){
            mins[i][j]=9999;
        }
    }
    maze[0][0]=1;
    dfs(0,0,1);
    cout<<ans<<endl<<best-1<<endl;

    return 0;
}

```


*例题2：分考场*

*n 个人参加某项特殊考试。*
*为了公平，要求任何两个认识的人不能分在同一个考场。求是少需要分几个考场才能满足条件。*

```

#include<iostream>
#include<vector>

using namespace std;

const int N=110;
int n,m;
int best=1<<28;

bool maze[N][N];
vector<int> v[N];


bool judge(int i,int b)//最好在外面写judge，而且遇到不合格的情况就立马返回，不然超时
{
	int s=v[i].size();
	for(int j=0;j<s;j++){
		if(maze[b][v[i][j]]){
			return false;
		}
	}
	return true;
}


void dfs(int now,int cnt)
{
	if(cnt>=best){
        //这里剪枝是>=不然超时，到=best还没有结束不管最后结果是不是best都不用做了
		return;	
	}
	
	if(now==n+1){
        //正常结束，当所有人都安排好考场后
		best=min(cnt,best);
		return;
	}
	 
	//插入cnt中的一个 
    //注意考场是从1开始到cnt

	for(int i=1;i<=cnt;i++){
		if(judge(i,now)){
			v[i].push_back(now);
			dfs(now+1,cnt);         //这里没有新开考场所以cnt不加
			v[i].pop_back();
		}		
	}
	
	//新开考场
	if(cnt+1<=n){
		v[cnt+1].push_back(now);
		dfs(now+1,cnt+1);
		v[cnt+1].pop_back();	
	} 		
}

int main()
{
	cin>>n>>m;
	for(int i=1;i<=m;i++){
		int a,b;
		cin>>a>>b;
		maze[a][b]=maze[b][a]=1;
	}
	dfs(1,0);                       //开始cnt等于0，第一步dfs一定是分配考场
	cout<<best<<endl;
	
	return 0;
}
```



*例题3*

*小蓝有一条玩具蛇，一共有16 节，上面标着数字 1 至 16.每一节都是一个正方形的形状。相邻的两节可以成直线或者成90度角。小蓝还有一个4×4的方格盒子，用于存放玩具蛇，盒子的方格上依次标着字母A到P共16个字母。小蓝可以折叠自己的玩具蛇放到盒子里面。他发现，有很多种方案可以将玩具蛇放进去.请帮小蓝计算一下，总共有多少种不同的方案。如果两个方案中，存在玩具蛇的某一节放在了盒子的不同格子里，则认为是不同的方案。*


```
#include<iostream>
#include<cstring>

using namespace std;

bool maze[16][16];

const int dirx[]={0,0,1,-1};
const int diry[]={-1,1,0,0};

int ans=0;

void dfs(int x,int y,int cnt)
{
	maze[x][y]=1;       //每次进入时候标记访问过
	if(cnt==16){
		ans+=1;
		return;
	}
	for(int i=0;i<4;i++){
		int tox=x+dirx[i];
		int toy=y+diry[i];
		if(tox<0||tox>=4||toy<0||toy>=4||maze[tox][toy]==1){
			continue;
		}
		dfs(tox,toy,cnt+1);
		maze[tox][toy]=0;
	}	
}

int main()
{
	int a,b;
	for(a=0;a<4;a++){
		for(b=0;b<4;b++){
			memset(maze,0,sizeof(maze));
			dfs(a,b,1);
		}
	}
	cout<<ans<<endl;
	
	
	return 0;
}
```


*例4*
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210312-1615527941739 "optional title")
*小蓝要用七段码数码管来表示一种特殊的文字上图给出了七段码数码管的一个图示，数码管中一共有 7段可以发光的二 极管，分别标记为 a,b,c,d,e,f,g。小蓝要选择一部分二极管（至少要有一个）发光来表达字符。在设计字符 的表达时，要求所有发光的二极管是连成一片的。例如：b发光，其他二极管不发光可以用来表达一种字符。例如 c发光，其他二极管不发光可以用来表达一种字符。这种方案与上 一行的方案可以用来表示不同的字符，尽管看上去比较相似。例如：a,b,c,d,e 发光，f,g 不发光可以用来表达一种字符。例如：b,f 发光，其他二极管不发光则不能用来表达一种字符，因为发光 的二极管没有连成一片。请问，小蓝可以用七段码数码管表达多少种不同的字符？*

```
#include<iostream>
#include<cstring>
#include<algorithm>


using namespace std;

const int N=7;

int g[7][7]=
{
	{0,1,0,0,0,1,0},
	{1,0,1,0,0,0,1},
	{0,1,0,1,0,0,1},
	{0,0,1,0,1,0,0},
	{0,0,0,1,0,1,1},
	{1,0,0,0,1,0,1},
	{0,1,1,0,1,1,0}
};

int bright[7];
int vis[7];

void dfs(int stick)
{
	for(int i=0;i<7;i++){
		if(g[stick][i]&&bright[i]&&!vis[i]){
			vis[i]=1;
			dfs(i);
		}
	}	
}

int main()
{
	int i,j,stick,x,ans=127;
	for(int i=1;i<=127;i++){
		memset(bright,0,sizeof(bright));
		memset(vis,0,sizeof(vis));
		x=i,j=0;
		while(x){
			if(x%2){
				bright[j++]=1;
			}
			else{
				bright[j++]=0;
			}
			x>>=1;
		}
		stick=0;
		while(!bright[stick])
			stick++;
		vis[stick]=1;
		dfs(stick);
		
		for(int j=0;j<7;j++){
			if(bright[j]&&vis[j]==0){
				ans--;
				break;
			}
		}	
	}
	cout<<ans;
	
	return 0;
}

```


*例5 方格分割*
*6x6的方格，沿着格子的边线剪开成两部分。 要求这两部分的形状完全相同*
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210317-1615964222859 "optional title")
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210317-1615964217192 "optional title")
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210317-1615964210676 "optional title")
*试计算： 包括这3种分法在内，一共有多少种不同的分割方法。 注意：旋转对称的属于同一种分割法。*

```
//注意本题是方块内部图形相同，但是我们搜索的是点，通过从最中间的点出发划线到边缘将其分割开
//因为一次操作时候对应坐标点同时相应变化，所以不用增加考虑判断是否相等

//这题首次将dfs设置返回值用来代表一次成功
#include<iostream>

using namespace std;

const int dirx[]={0,0,1,-1};
const int diry[]={1,-1,0,0};

bool maze[7][7];

bool judge(int x,int y)
{
	if(x>=0&&x<=6&&y>=0&&y<=6){
		return true;
	}
	return false;
}

int dfs(int x,int y)
{
	//退出标志：点到达任意一个边边缘，返回1代表一条方法
	if(x==0||y==0||x==6||y==6){
		return 1;
	}
	
	int res=0;
	
	for(int i=0;i<4;i++){
		int tox=x+dirx[i];
		int toy=y+diry[i];
		if(judge(tox,toy)&&judge(6-tox,6-toy)&&!maze[tox][toy]&&!maze[6-tox][6-toy]){
			maze[tox][toy]=true;
			maze[6-tox][6-toy]=true;
			res+=dfs(tox,toy);
			maze[tox][toy]=false;
			maze[6-tox][6-toy]=false;
		}
		
	return res;
}

int main()
{
	maze[3][3]=true;
	cout<<dfs(3,3)/4<<endl;

	return 0;

}
```


*例6 全球变暖*
*你有一张某海域 NxN 像素的照片，"."表示海洋、"#"表示陆地，如下所示：*
```
.......

.##....

.##....

....##.

..####.

...###.

.......
```
*其中"上下左右"四个方向上连在一起的一片陆地组成一座岛屿。例如上图就有 2 座岛屿。由于全球变暖导致了海面上升，科学家预测未来几十年，岛屿边缘一个像素的范围会被海水淹没。具体来说如果一块陆地像素与海洋相邻(上下左右四个相邻像素中有海洋)，它就会被淹没。*
*例如上图中的海域未来会变成如下样子：*
```
.......

.......

.......

.......

....#..

.......

.......
```
*请你计算：依照科学家的预测，照片中有多少岛屿会被完全淹没。*
*输入描述*
*第一行包含一个整数 (1≤N≤1000)。以下N行N列代表一张海域照片。照片保证第 1 行、第 1 列、第N 行、第 N 列的像素都是海洋,输出一个整数表示答案。*

```
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<stdio.h>
using namespace std;
bool flag;//标记是否岛屿沉没
char a[1010][1010];//地图保存
int cnt=0;//统计周围#的数目
int n;//
int d[4][2]= {1,0,-1,0,0,1,0,-1};//枚举方向的数组
int ans=0;//被淹没后的岛屿个数
int res_ans=0;//没被淹没的岛屿的个数
void dfs(int x,int y)//回溯
{
    if(a[x][y]!='#')//如果这个点被标记成已经探索的岛屿内容或者是海洋，那么就返回
        return ;
    if(x>=n||x<0||y>=n||y<0)//如果越界了也是返回
        return ;
    if(flag==false)//如果还没确定不会被淹没，即当前是被淹没状态
    {cnt=0;//统计这个岛屿的当前点坐标x，y的上下左右的四个点是不是都是'#'，如果确实是这样的，那么为我们就可以确定这个岛屿不会被淹没，我们姑且叫这样的点叫做不可淹没点，，那么只要一个岛屿有一个这样的点就一定不会被淹没
        for(int i=0; i<4; i++)//枚举四个方向
        {
            int tx=x+d[i][0];
            int ty=y+d[i][1];
            if(tx<n&&tx>=0&&ty<n&&ty>=0&&a[tx][ty]!='.')//假设枚举的四周的点不会超界并且不是海洋，那么cnt数目+1
            {
                cnt++;
            }
        }
        if(cnt==4)//如果四周的点都是这样的那么这个点就是不可淹没点
        {
            ans++;//最后不会被淹没的点+！
            flag=true;//这个岛屿标记已经确定，当前岛屿是不可以被淹没的，所以当回溯这个岛屿其他点的时候就不用再来检查了，因为一个岛屿有一个这样的不可淹没点就够了
        }
    }
    a[x][y]='*';//对于已经探索的岛屿的内容标记成'*'，
    for(int i=0; i<4; i++)
    {
        int tx=x+d[i][0];
        int ty=y+d[i][1];
            dfs(tx,ty);//对于四个方向进行回溯岛屿的所有点
    }
}
int main()
{
    cin>>n;
    for(int i=0; i<n; i++)
    {
        scanf("%s",a[i]);
    }//输入数据
    for(int i=0; i<n; i++)
    {
        for(int j=0; j<n; j++)
        {//遍历地图的所以坐标
            if(a[i][j]=='#')//如果被找到的是同一个岛屿那么会被标记成'*',不会进入回溯，只有是'#'才说明是未被搜索的岛屿的一角
            {
                res_ans++;//那么岛屿数目加1
                flag=false;//假设会淹没
                dfs(i,j);//回溯标记这个岛屿的所有点的坐标，并且如果不会被完全的淹没，那么就标记
            }
        }
    }
    cout<<res_ans-ans<<endl;//输出答案
}
```


*例7 小朋友崇拜圈*
*班里N个小朋友，每个人都有自己最崇拜的一个小朋友（也可以是自己）。在一个游戏中，需要小朋友坐一个圈，每个小朋友都有自己最崇拜的小朋友在他的右手边。求满足条件的圈最大多少人？小朋友编号为1,2,3,?N。*

*输入*
```
9
3 4 2 5 3 8 4 6 9
```
*输出 4*

```
#include <iostream>
using namespace std;
int a[2000000];
int vis[2000000];
int m, N, t;
void dfs(int x, int ans){
  if(vis[x]){
      if(a[x] == a[t]){
          if(ans > m){
              m = ans;
          }
      }
    return;
  }
      vis[x] = 1;
      dfs(a[x], ans + 1);
      vis[x] = 0;
}
int main()
{
   cin>>N;
   for(int i = 1; i <= N; i++){
     cin>>a[i];
   }
   for(int i = 1; i <= N; i++){
           t = i;
        dfs(i, 0);
  }
  cout<<m;
  return 0;
}
```


*例8 正则表达式*
*考虑一种简单的正则表达式：只由 x ( ) | 组成的正则表达式小明想求出这个正则表达式能接受的最长字符串的长度。例如 ((xx|xxx)x|(x|xx))xx 能接受的最长字符串是： xxxxxx，长度是 6*

```
#include<iostream>
using namespace std;

string s;
int ans=0;
int t=0;//标记当前在哪个位置

int dfs()
{
	int sum=0;//单次递归x的数目
	//循环分四种情况(,x,|,)
	while(t<s.length())
	{
		if(s[t]=='('){
			t++;
			sum+=dfs();//进入左括号内递归运算累加
		}
		else if(s[t]=='x'){
			t++;
			sum++;
		}
		else if(s[t]==')'){
			t++;
			return sum;
		}
		else{
			t++;
			return max(sum,dfs());//返回当前层和下一层中最大值
		}
	}
	return sum;
}

int main()
{
	cin>>s;
	cout<<dfs()<<endl;

	return 0;
}


```

*例9 输出所有数字排列*

```
#include<iostream>

using namespace std;
int n;
int vis[8];//表示是否访问过 
int path[8];//存储路径 

void dfs(int pos)
{
	if(pos>n){//保证大于n，不然等于结束最后一个path收录不到 
		for(int i=1;i<=n;i++){
			cout<<path[i]<<" ";
		}
		cout<<endl;
		return;
	}
	for(int i=1;i<=n;i++){
		if(vis[i]){
			continue;
		}
		path[pos]=i;
		vis[i]=1;
		dfs(pos+1);
		vis[i]=0;
	}
	
}

int main()
{
	cin>>n;
	dfs(1);//这里代表的不是1开始而是第一位 

	return 0;
} 
```


*例10 八皇后问题*

```
#include <iostream>
using namespace std;
const int N = 20; 

// bool数组用来判断搜索的下一个位置是否可行
// col列，dg对角线，udg反对角线
// g[N][N]用来存路径

int n;
char g[N][N];
bool col[N], dg[N], udg[N];

void dfs(int u) {
    // u == n 表示已经搜了n行，故输出这条路径
    if (u == n) {
        for (int i = 0; i < n; i ++ ) puts(g[i]);   // 等价于cout << g[i] << endl;
        puts("");  // 换行
        return;
    }

    //对n个位置按行搜索
    for (int i = 0; i < n; i ++ )
        // 剪枝(对于不满足要求的点，不再继续往下搜索)  
        // udg[n - u + i]，+n是为了保证下标非负
        if (!col[i] && !dg[u + i] && !udg[n - u + i]) {
            g[u][i] = 'Q';
            col[i] = dg[u + i] = udg[n - u + i] = true;
            dfs(u + 1);
            col[i] = dg[u + i] = udg[n - u + i] = false; // 恢复现场 这步很关键
            g[u][i] = '.';

        }
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < n; j ++ )
            g[i][j] = '.';

    dfs(0);

    return 0;
}   

```



## bfs

*例题一：bfs解决迷宫问题*

```
#include<iostream>
#include<queue>
#include<cstring>
#include<algorithm>
#define maxn 50

using namespace std;

const int row=30,col=50;//一定使用const
int vis[maxn][maxn];    //设置访问标识数组

struct node
{
    int x,y,step;
    string path;
};
node now;
node nextpos;

const int dirx[]={1,0,0,-1};
const int diry[]={0,-1,1,0};
const char dir[]={'D','L','R','U'};//一定按照字典序扩展，最后结果一定是字典序的

string s[30]={
"01010101001011001001010110010110100100001000101010",
"00001000100000101010010000100000001001100110100101",
"01111011010010001000001101001011100011000000010000",
"01000000001010100011010000101000001010101011001011",
"00011111000000101000010010100010100000101100000000",
"11001000110101000010101100011010011010101011110111",
"00011011010101001001001010000001000101001110000000",
"10100000101000100110101010111110011000010000111010",
"00111000001010100001100010000001000101001100001001",
"11000110100001110010001001010101010101010001101000",
"00010000100100000101001010101110100010101010000101",
"11100100101001001000010000010101010100100100010100",
"00000010000000101011001111010001100000101010100011",
"10101010011100001000011000010110011110110100001000",
"10101010100001101010100101000010100000111011101001",
"10000000101100010000101100101101001011100000000100",
"10101001000000010100100001000100000100011110101001",
"00101001010101101001010100011010101101110000110101",
"11001010000100001100000010100101000001000111000010",
"00001000110000110101101000000100101001001000011101",
"10100101000101000000001110110010110101101010100001",
"00101000010000110101010000100010001001000100010101",
"10100001000110010001000010101001010101011111010010",
"00000100101000000110010100101001000001000000000010",
"11010000001001110111001001000011101001011011101000",
"00000110100010001000100000001000011101000000110011",
"10101000101000100010001111100010101001010000001000",
"10000010100101001010110000000100101010001011101000",
"00111100001000010000000110111000000001000000001011",
"10000001100111010111010001000110111010101101111000"
};

void bfs()
{
    //初始化第一个队列元素
    queue<node> q;
    now.x=0;
    now.y=0;
    now.step=0;
    now.path="";        //path为空
    q.push(now);
    vis[0][0]=1;        //注意要标记访问过


    while(!q.empty())//队列不为空
    {
        now=q.front();//固定程序，先获得队首节点后出列
        q.pop();

        //特殊条件结束程序
        if(now.x==29&&now.y==49){
            cout<<now.path;//所有的路径存放在子节点
            break;
        }    

        for(int i=0;i<4;i++){
        	int tox=now.x+dirx[i];
        	int toy=now.y+diry[i];
            if(tox<0||tox>row-1||toy<0||toy>col-1||vis[tox][toy]==1||s[tox][toy]=='1'){
                continue;
            }
            vis[tox][toy]=1;
            nextpos.x=tox;
            nextpos.y=toy;
            nextpos.step=now.step+1;
            nextpos.path=now.path+dir[i];
            q.push(nextpos);
        }
    }
}


int main()
{
    bfs();

    return 0;
}

```


*例2*

*如下图所示：有9只盘子，排成1个圆圈。其中8只盘子内装着8只蚱蜢，有一个是空盘。我们把这些蚱蜢顺时针编号为1~ 8.每只蚱蜢都可以跳到相邻的空盘中， 也可以再用点力，越过一个相邻的蚱蜢跳到空盘中。请你计算一下，如果要使得蚱蜢们的队形改为按照逆时针排列，并且保持空盘的位置不变（也就是1?8换位，2?7换位,...），至少要经过多少次跳跃?*




```
#include<iostream>
#include<cstring>
#include<string>
#include<queue>
#include<unordered_map>

using namespace std;

string bg="012345678";
string ed="087654321";

unordered_map<string,int> m;


const int dir[]={1,-1,2,-2};

string now;

int bfs()
{
	queue<string> q;
	q.push(bg);
	m[bg]=0;
	
	
	while(!q.empty())
	{
		now=q.front();
		q.pop();
	
		int k=now.find('0');
		for(int i=0;i<4;i++){
			string str=now;
			swap(str[(k+dir[i]+9)%9],str[k]);
			if(m.count(str)){
				continue;
			}
			m[str]=m[now]+1;
			if(str==ed){
				return m[str];
			}
			q.push(str);
		} 		
	}
	return -1;	
}

int main()
{
	cout<<bfs();

	return 0;
}
```



*例3*

*小明在玩一款迷宫游戏，在游戏中他要控制自己的角色离开一间由 N×N 个格子组成的 2D 迷宫。小明的起始位置在左上角，他需要到达右下角的格子才能离开迷宫。每一步，他可以移动到上下左右相邻的格子中（前提是目标格子可以经过）。迷宫中有些格子小明可以经过，我们用 '.' 表示。有些格子是墙壁，小明不能经过，我们用 '#' 表示。此外，有些格子上有陷阱，我们用 'X' 表示。除非小明处于无敌状态，否则不能经过。有些格子上有无敌道具，我们用 '%' 表示。当小明第一次到达该格子时，自动获得无敌状态，无敌状态会持续K步。之后如果再次到达该格子不会获得无敌状态了。处于无敌状态时，可以经过有陷阱的格子，但是不会拆除/毁坏陷阱，即陷阱仍会阻止没有无敌状态的角色经过。给定迷宫，请你计算小明最少经过几步可以离开迷宫?*

*输入描述第一行包含两个整数(1≤N≤1000,1≤K≤10)。以下N 行包含一个N×N 的矩阵。矩阵保证左上角和右下角是 '.'。*
*输出描述*
*一个整数表示答案。如果小明不能离开迷宫，输出 -1*
```
5 3
...XX
##%#.
...#.
.###.
.....
```


```
#include<iostream>
#include<string>
#include<algorithm>
#include<queue>

const int N=1005;
int n,k;

using namespace std;



char maze[N][N];
bool vis[N][N][11];
int ans;
int dirx[4]={0,1,0,-1};
int diry[4]={1,0,-1,0};

struct node{
	//x，y位置，k当前剩余无敌时间
	//s到这位置的步数
	int x,y,k,step;
	node(int cx,int cy,int ck,int cs){
		x=cx,y=cy,k=ck,step=cs;
	} 
	
};


int bfs()
{
	queue<node> q;
	vis[0][0][0]=true;
	q.push(node(0,0,0,0));
	
	while(!q.empty())
	{
		node now=q.front();
		q.pop();
		
		if(now.x==n-1&&now.y==n-1){
			return now.step;
		}
		
		for(int i=0;i<4;i++){
			int tox=now.x+dirx[i];
			int toy=now.y+diry[i];
			if(tox<0||toy<0||tox>=n||toy>=n||maze[tox][toy]=='#'){
				continue;	
			}	
		}
		//在tox，toy处有无敌道具且没有访问过(无敌道具没被拿)
		if(maze[tox][toy]=='%'&&!vis[tox][toy][k]){
			vis[tox][toy][k]=true;
			q.push(node(tox,toy,k,now.step+1));	
		}
		else{
			//当前无敌，什么路都能走 
			if(now.k&&!vis[tox][toy][now.k-1]){
				vis[tox][toy][now.k-1]=true;
				q.push(node(tox,toy,now.k-1,now.step+1));
			}
			//当前不无敌，而且前方为道路 
			else if(maze[tox][toy]=='.'&&!now.k&&!vis[tox][toy][0]){
				vis[tox][toy][0]=true;
				q.push(node(tox,toy,0,now.step+1));			
			}	
		}	
	}
	return -1;
}




int main()
{
	cin>>n>>k;
	for(int i=0;i<n;i++){
		cin>>maze[i];
	} 
	ans=bfs();
	cout<<ans<<endl;
	
	return 0; 
} 
```


*例4 穿越雷区*
*X 星的坦克战车很奇怪，它必须交替地穿越正能量辐射区和负能量辐射区才能保持正常运转，否则将报废。某坦克需要从 A 区到 B 区去（ A，B 区本身是安全区，没有正能量或负能量特征），怎样走才能路径最短？已知的地图是一个方阵，上面用字母标出了 A，B 区，其它区都标了正号或负号分别表示正负能量辐射区。*
*坦克车只能水平或垂直方向上移动到相邻的区。*
*例如：*
```
A + - + -

- + - - +

- + + + -

+ - + - +

B + - + -
```
*输入描述*

*第一行是一个整数 n，表示方阵的大小,4≤n<100。接下来是 n 行，每行有 n 个数据，可能是 A，B，+，- 中的某一个，中间用空格分开。A，B 都只出现一次。*

*输出描述*
*输出一个整数，表示坦克从 A 区到 B 区的最少移动步数。如果没有方案，则输出 -1。*

```
#include<iostream>
#include<queue>


using namespace std;
const int maxn=105;

int n;

char maze[maxn][maxn];


const int dirx[]={1,-1,0,0};
const int diry[]={0,0,1,-1};

int vis[maxn][maxn];


struct node{
	int x,y,step;
	char flag;
};

node now,nextpos;

char arr[maxn][maxn];

int ans=0;

bool judge(int x,int y)
{
	if(x>=0&&y>=0&&x<n&&y<n){
		return true;
	}
	return false;
}

void bfs(int x,int y)
{
	queue<node> q;
	now.x=x;
	now.y=y;
	now.step=0;
	now.flag='A';
	q.push(now);
	vis[x][y]=1;
	
	while(!q.empty())
	{
		now=q.front();
		q.pop();
		
		if(now.flag=='B'){
			ans=now.step;
			break;
		}
		
		for(int i=0;i<4;i++){
			int tox=now.x+dirx[i];
			int toy=now.y+diry[i];
			char s1=maze[tox][toy];
			if(judge(tox,toy)&&s1!=now.flag&&!vis[tox][toy]){
				vis[tox][toy]=1;
				nextpos.x=tox;
				nextpos.y=toy;
				nextpos.step=now.step+1;
				nextpos.flag=s1;
				q.push(nextpos);
			}
			
		}
		
		
	}
	
}


int  main()
{
	cin>>n;
	
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			cin>>maze[i][j];
		}
	} 
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			if(maze[i][j]=='A'){
				bfs(i,j);
				break;
			}
		}
	}
	cout<<ans;
	
	return 0;
}
```



*例5 扫雷*
*在一个n行m 列的方格图上有一些位置有地雷，另外一些位置为空。请为每个空位置标一个整数，表示周围八个相邻的方格中有多少个地雷*
*输入*
```
3 4
0 1 0 0
1 0 1 0
0 0 1 0
```
*输出*
```
2 9 2 1
9 4 9 2
1 3 9 2
```

```
#include<iostream>
#include<queue>

const int maxn=105;
using namespace std;

int n,m;


int maze[maxn][maxn];
int newmaze[maxn][maxn];

int vis[maxn][maxn];

struct node{
	int x,y,sum;
};

node now,nextp;

void bfs(int x,int y)
{
	queue<node> q;
	now.x=x;
	now.y=y;
	now.sum=0;
	vis[x][y]=1;
	q.push(now);
	
	while(!q.empty())
	{
		now=q.front();
		q.pop();
		
		int sum=0;//每一次创一个新sum表示当前的总雷数
		for(int i=now.x-1;i<=now.x+1;i++){
			for(int j=now.y-1;j<=now.y+1;j++){
				if(i<0||i>=n||j<0||j>=m){
					continue;
				}
				if(maze[i][j]){
					sum+=1;
				}	
			}
		}
		now.sum=sum;
		newmaze[now.x][now.y]=now.sum;
		
		for(int i=0;i<n;i++){
			for(int j=0;j<m;j++){
				if(maze[i][j]||vis[i][j]){
					continue;
				}
				vis[i][j]=1;
				nextp.x=i;
				nextp.y=j;
				nextp.sum=0;
				q.push(nextp);
			}
		}
			
	}
	
	
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if(maze[i][j]==1){
				newmaze[i][j]=9;
			}
			cout<<newmaze[i][j]<<" ";
		}
		cout<<endl;
	}
	
}

int main()
{
	cin>>n>>m;
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			cin>>maze[i][j];
		}
	}
	bfs(0,0);
	return 0;
}
```