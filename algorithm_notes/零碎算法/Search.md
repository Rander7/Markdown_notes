# ��������

**dfsʹ�õݹ��ջ��bfsʹ�ö���**

**1. dfs�жϽ���ʹ����󵽴�ĳ��״̬return bfs�鿴�����Ƿ�Ϊ�ջ���ĳ�����������˳�**

**2. ���·����bfs���������dfs**

**3. bfsһ�㻹��һ��vis�жϣ�dfsһ��û��**

**bfs�������չ�ģ����Ե�һ�����ص�һ�������·����dfs�ҵ�һ���������ж��ǲ�����̣�Ҳ����˵������dfs��������**




## dfs

*����һ���Թ����⣺*

*��ͼ������һ���Թ���ƽ��ͼ�����б��Ϊ1��Ϊ�ϰ������Ϊ0��Ϊ����ͨ�еĵط���*
```
010000
000100
001001
110000
```
*�Թ������Ϊ���Ͻǣ�����Ϊ���½ǣ����Թ��У�ֻ�ܴ�һ��λ���ߵ��� �������ϡ��¡������ĸ�����֮һ������������Թ�������ڿ�ʼ�����԰� DRRURRDDDR ��˳��ͨ���Թ��� һ�� 10 �������� D��U��L��R �ֱ��ʾ���¡����ϡ����������ߡ� ����������������ӵ��Թ������ҳ�һ��ͨ���Թ��ķ�ʽ����ʹ�õĲ������٣��ڲ������ٵ�ǰ���£����ҳ��ֵ�����С��һ����Ϊ�𰸡�*
*��ע�����ֵ����� D<L<R<U*

```
#include<iostream>
#include<string>
#include<algorithm>
#define maxn 50

using namespace std;

const int dirx[]={1,0,0,-1};
const int diry[]={0,-1,1,0};
const char dir[]={'D','L','R','U'};

int best=1<<28;         //������һ���ܴ����
const int row=30,col=50;//ע��һ��ʹ��const
char a[row*col+5];      //������Ŷ�Ӧλ�ƶ�����
int mins[maxn][maxn];   //��Ŷ�Ӧλ����Сֵ
string ans;             //�������ƶ����

//�����ֱ�ʾ��ʽ��һ���Ƕ�ά�ַ����飨��һ����string
//ע��string�ж��Ƿ���1Ҫ��maze[x][y]=='1'�жϣ�

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
        //��֦
        return;
    }
    if(x==row-1&&y==col-1){
        //��������
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


*����2���ֿ���*

*n ���˲μ�ĳ�����⿼�ԡ�*
*Ϊ�˹�ƽ��Ҫ���κ�������ʶ���˲��ܷ���ͬһ����������������Ҫ�ּ���������������������*

```

#include<iostream>
#include<vector>

using namespace std;

const int N=110;
int n,m;
int best=1<<28;

bool maze[N][N];
vector<int> v[N];


bool judge(int i,int b)//���������дjudge�������������ϸ������������أ���Ȼ��ʱ
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
        //�����֦��>=��Ȼ��ʱ����=best��û�н�������������ǲ���best����������
		return;	
	}
	
	if(now==n+1){
        //�����������������˶����źÿ�����
		best=min(cnt,best);
		return;
	}
	 
	//����cnt�е�һ�� 
    //ע�⿼���Ǵ�1��ʼ��cnt

	for(int i=1;i<=cnt;i++){
		if(judge(i,now)){
			v[i].push_back(now);
			dfs(now+1,cnt);         //����û���¿���������cnt����
			v[i].pop_back();
		}		
	}
	
	//�¿�����
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
	dfs(1,0);                       //��ʼcnt����0����һ��dfsһ���Ƿ��俼��
	cout<<best<<endl;
	
	return 0;
}
```



*����3*

*С����һ������ߣ�һ����16 �ڣ������������ 1 �� 16.ÿһ�ڶ���һ�������ε���״�����ڵ����ڿ��Գ�ֱ�߻��߳�90�Ƚǡ�С������һ��4��4�ķ�����ӣ����ڴ������ߣ����ӵķ��������α�����ĸA��P��16����ĸ��С�������۵��Լ�������߷ŵ��������档�����֣��кܶ��ַ������Խ�����߷Ž�ȥ.���С������һ�£��ܹ��ж����ֲ�ͬ�ķ�����������������У���������ߵ�ĳһ�ڷ����˺��ӵĲ�ͬ���������Ϊ�ǲ�ͬ�ķ�����*


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
	maze[x][y]=1;       //ÿ�ν���ʱ���Ƿ��ʹ�
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


*��4*
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210312-1615527941739 "optional title")
*С��Ҫ���߶������������ʾһ�������������ͼ�������߶�������ܵ�һ��ͼʾ���������һ���� 7�ο��Է���Ķ� ���ܣ��ֱ���Ϊ a,b,c,d,e,f,g��С��Ҫѡ��һ���ֶ����ܣ�����Ҫ��һ��������������ַ���������ַ� �ı��ʱ��Ҫ�����з���Ķ�����������һƬ�ġ����磺b���⣬���������ܲ���������������һ���ַ������� c���⣬���������ܲ���������������һ���ַ������ַ������� һ�еķ�������������ʾ��ͬ���ַ������ܿ���ȥ�Ƚ����ơ����磺a,b,c,d,e ���⣬f,g ����������������һ���ַ������磺b,f ���⣬���������ܲ����������������һ���ַ�����Ϊ���� �Ķ�����û������һƬ�����ʣ�С���������߶�������ܱ������ֲ�ͬ���ַ���*

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


*��5 ����ָ�*
*6x6�ķ������Ÿ��ӵı��߼����������֡� Ҫ���������ֵ���״��ȫ��ͬ*
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210317-1615964222859 "optional title")
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210317-1615964217192 "optional title")
![Alt text](https://doc.shiyanlou.com/courses/uid1580206-20210317-1615964210676 "optional title")
*�Լ��㣺 ������3�ַַ����ڣ�һ���ж����ֲ�ͬ�ķָ���� ע�⣺��ת�ԳƵ�����ͬһ�ַָ��*

```
//ע�Ȿ���Ƿ����ڲ�ͼ����ͬ�����������������ǵ㣬ͨ�������м�ĵ�������ߵ���Ե����ָ
//��Ϊһ�β���ʱ���Ӧ�����ͬʱ��Ӧ�仯�����Բ������ӿ����ж��Ƿ����

//�����״ν�dfs���÷���ֵ��������һ�γɹ�
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
	//�˳���־���㵽������һ���߱�Ե������1����һ������
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


*��6 ȫ���ů*
*����һ��ĳ���� NxN ���ص���Ƭ��"."��ʾ����"#"��ʾ½�أ�������ʾ��*
```
.......

.##....

.##....

....##.

..####.

...###.

.......
```
*����"��������"�ĸ�����������һ���һƬ½�����һ�����졣������ͼ���� 2 �����졣����ȫ���ů�����˺�����������ѧ��Ԥ��δ����ʮ�꣬�����Եһ�����صķ�Χ�ᱻ��ˮ��û��������˵���һ��½�������뺣������(���������ĸ������������к���)�����ͻᱻ��û��*
*������ͼ�еĺ���δ�������������ӣ�*
```
.......

.......

.......

.......

....#..

.......

.......
```
*������㣺���տ�ѧ�ҵ�Ԥ�⣬��Ƭ���ж��ٵ���ᱻ��ȫ��û��*
*��������*
*��һ�а���һ������ (1��N��1000)������N��N�д���һ�ź�����Ƭ����Ƭ��֤�� 1 �С��� 1 �С���N �С��� N �е����ض��Ǻ���,���һ��������ʾ�𰸡�*

```
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<stdio.h>
using namespace std;
bool flag;//����Ƿ����û
char a[1010][1010];//��ͼ����
int cnt=0;//ͳ����Χ#����Ŀ
int n;//
int d[4][2]= {1,0,-1,0,0,1,0,-1};//ö�ٷ��������
int ans=0;//����û��ĵ������
int res_ans=0;//û����û�ĵ���ĸ���
void dfs(int x,int y)//����
{
    if(a[x][y]!='#')//�������㱻��ǳ��Ѿ�̽���ĵ������ݻ����Ǻ�����ô�ͷ���
        return ;
    if(x>=n||x<0||y>=n||y<0)//���Խ����Ҳ�Ƿ���
        return ;
    if(flag==false)//�����ûȷ�����ᱻ��û������ǰ�Ǳ���û״̬
    {cnt=0;//ͳ���������ĵ�ǰ������x��y���������ҵ��ĸ����ǲ��Ƕ���'#'�����ȷʵ�������ģ���ôΪ���ǾͿ���ȷ��������첻�ᱻ��û�����ǹ��ҽ������ĵ����������û�㣬����ôֻҪһ��������һ�������ĵ��һ�����ᱻ��û
        for(int i=0; i<4; i++)//ö���ĸ�����
        {
            int tx=x+d[i][0];
            int ty=y+d[i][1];
            if(tx<n&&tx>=0&&ty<n&&ty>=0&&a[tx][ty]!='.')//����ö�ٵ����ܵĵ㲻�ᳬ�粢�Ҳ��Ǻ�����ôcnt��Ŀ+1
            {
                cnt++;
            }
        }
        if(cnt==4)//������ܵĵ㶼����������ô�������ǲ�����û��
        {
            ans++;//��󲻻ᱻ��û�ĵ�+��
            flag=true;//����������Ѿ�ȷ������ǰ�����ǲ����Ա���û�ģ����Ե�������������������ʱ��Ͳ�����������ˣ���Ϊһ��������һ�������Ĳ�����û��͹���
        }
    }
    a[x][y]='*';//�����Ѿ�̽���ĵ�������ݱ�ǳ�'*'��
    for(int i=0; i<4; i++)
    {
        int tx=x+d[i][0];
        int ty=y+d[i][1];
            dfs(tx,ty);//�����ĸ�������л��ݵ�������е�
    }
}
int main()
{
    cin>>n;
    for(int i=0; i<n; i++)
    {
        scanf("%s",a[i]);
    }//��������
    for(int i=0; i<n; i++)
    {
        for(int j=0; j<n; j++)
        {//������ͼ����������
            if(a[i][j]=='#')//������ҵ�����ͬһ��������ô�ᱻ��ǳ�'*',���������ݣ�ֻ����'#'��˵����δ�������ĵ����һ��
            {
                res_ans++;//��ô������Ŀ��1
                flag=false;//�������û
                dfs(i,j);//���ݱ�������������е�����꣬����������ᱻ��ȫ����û����ô�ͱ��
            }
        }
    }
    cout<<res_ans-ans<<endl;//�����
}
```


*��7 С���ѳ��Ȧ*
*����N��С���ѣ�ÿ���˶����Լ����ݵ�һ��С���ѣ�Ҳ�������Լ�������һ����Ϸ�У���ҪС������һ��Ȧ��ÿ��С���Ѷ����Լ����ݵ�С�������������ֱߡ�������������Ȧ�������ˣ�С���ѱ��Ϊ1,2,3,?N��*

*����*
```
9
3 4 2 5 3 8 4 6 9
```
*��� 4*

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


*��8 ������ʽ*
*����һ�ּ򵥵�������ʽ��ֻ�� x ( ) | ��ɵ�������ʽС����������������ʽ�ܽ��ܵ���ַ����ĳ��ȡ����� ((xx|xxx)x|(x|xx))xx �ܽ��ܵ���ַ����ǣ� xxxxxx�������� 6*

```
#include<iostream>
using namespace std;

string s;
int ans=0;
int t=0;//��ǵ�ǰ���ĸ�λ��

int dfs()
{
	int sum=0;//���εݹ�x����Ŀ
	//ѭ�����������(,x,|,)
	while(t<s.length())
	{
		if(s[t]=='('){
			t++;
			sum+=dfs();//�����������ڵݹ������ۼ�
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
			return max(sum,dfs());//���ص�ǰ�����һ�������ֵ
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

*��9 ���������������*

```
#include<iostream>

using namespace std;
int n;
int vis[8];//��ʾ�Ƿ���ʹ� 
int path[8];//�洢·�� 

void dfs(int pos)
{
	if(pos>n){//��֤����n����Ȼ���ڽ������һ��path��¼���� 
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
	dfs(1);//�������Ĳ���1��ʼ���ǵ�һλ 

	return 0;
} 
```


*��10 �˻ʺ�����*

```
#include <iostream>
using namespace std;
const int N = 20; 

// bool���������ж���������һ��λ���Ƿ����
// col�У�dg�Խ��ߣ�udg���Խ���
// g[N][N]������·��

int n;
char g[N][N];
bool col[N], dg[N], udg[N];

void dfs(int u) {
    // u == n ��ʾ�Ѿ�����n�У����������·��
    if (u == n) {
        for (int i = 0; i < n; i ++ ) puts(g[i]);   // �ȼ���cout << g[i] << endl;
        puts("");  // ����
        return;
    }

    //��n��λ�ð�������
    for (int i = 0; i < n; i ++ )
        // ��֦(���ڲ�����Ҫ��ĵ㣬���ټ�����������)  
        // udg[n - u + i]��+n��Ϊ�˱�֤�±�Ǹ�
        if (!col[i] && !dg[u + i] && !udg[n - u + i]) {
            g[u][i] = 'Q';
            col[i] = dg[u + i] = udg[n - u + i] = true;
            dfs(u + 1);
            col[i] = dg[u + i] = udg[n - u + i] = false; // �ָ��ֳ� �ⲽ�ܹؼ�
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

*����һ��bfs����Թ�����*

```
#include<iostream>
#include<queue>
#include<cstring>
#include<algorithm>
#define maxn 50

using namespace std;

const int row=30,col=50;//һ��ʹ��const
int vis[maxn][maxn];    //���÷��ʱ�ʶ����

struct node
{
    int x,y,step;
    string path;
};
node now;
node nextpos;

const int dirx[]={1,0,0,-1};
const int diry[]={0,-1,1,0};
const char dir[]={'D','L','R','U'};//һ�������ֵ�����չ�������һ�����ֵ����

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
    //��ʼ����һ������Ԫ��
    queue<node> q;
    now.x=0;
    now.y=0;
    now.step=0;
    now.path="";        //pathΪ��
    q.push(now);
    vis[0][0]=1;        //ע��Ҫ��Ƿ��ʹ�


    while(!q.empty())//���в�Ϊ��
    {
        now=q.front();//�̶������Ȼ�ö��׽ڵ�����
        q.pop();

        //����������������
        if(now.x==29&&now.y==49){
            cout<<now.path;//���е�·��������ӽڵ�
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


*��2*

*����ͼ��ʾ����9ֻ���ӣ��ų�1��ԲȦ������8ֻ������װ��8ֻ���죬��һ���ǿ��̡����ǰ���Щ����˳ʱ����Ϊ1~ 8.ÿֻ���춼�����������ڵĿ����У� Ҳ�������õ�����Խ��һ�����ڵ��������������С��������һ�£����Ҫʹ�������ǵĶ��θ�Ϊ������ʱ�����У����ұ��ֿ��̵�λ�ò��䣨Ҳ����1?8��λ��2?7��λ,...��������Ҫ�������ٴ���Ծ?*




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



*��3*

*С������һ���Թ���Ϸ������Ϸ����Ҫ�����Լ��Ľ�ɫ�뿪һ���� N��N ��������ɵ� 2D �Թ���С������ʼλ�������Ͻǣ�����Ҫ�������½ǵĸ��Ӳ����뿪�Թ���ÿһ�����������ƶ��������������ڵĸ����У�ǰ����Ŀ����ӿ��Ծ��������Թ�����Щ����С�����Ծ����������� '.' ��ʾ����Щ������ǽ�ڣ�С�����ܾ����������� '#' ��ʾ�����⣬��Щ�����������壬������ 'X' ��ʾ������С�������޵�״̬�������ܾ�������Щ���������޵е��ߣ������� '%' ��ʾ����С����һ�ε���ø���ʱ���Զ�����޵�״̬���޵�״̬�����K����֮������ٴε���ø��Ӳ������޵�״̬�ˡ������޵�״̬ʱ�����Ծ���������ĸ��ӣ����ǲ�����/�ٻ����壬�������Ի���ֹû���޵�״̬�Ľ�ɫ�����������Թ����������С�����پ������������뿪�Թ�?*

*����������һ�а�����������(1��N��1000,1��K��10)������N �а���һ��N��N �ľ��󡣾���֤���ϽǺ����½��� '.'��*
*�������*
*һ��������ʾ�𰸡����С�������뿪�Թ������ -1*
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
	//x��yλ�ã�k��ǰʣ���޵�ʱ��
	//s����λ�õĲ���
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
		//��tox��toy�����޵е�����û�з��ʹ�(�޵е���û����)
		if(maze[tox][toy]=='%'&&!vis[tox][toy][k]){
			vis[tox][toy][k]=true;
			q.push(node(tox,toy,k,now.step+1));	
		}
		else{
			//��ǰ�޵У�ʲô·������ 
			if(now.k&&!vis[tox][toy][now.k-1]){
				vis[tox][toy][now.k-1]=true;
				q.push(node(tox,toy,now.k-1,now.step+1));
			}
			//��ǰ���޵У�����ǰ��Ϊ��· 
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


*��4 ��Խ����*
*X �ǵ�̹��ս������֣������뽻��ش�Խ�������������͸��������������ܱ���������ת�����򽫱��ϡ�ĳ̹����Ҫ�� A ���� B ��ȥ�� A��B �������ǰ�ȫ����û���������������������������߲���·����̣���֪�ĵ�ͼ��һ��������������ĸ����� A��B �������������������Ż򸺺ŷֱ��ʾ����������������*
*̹�˳�ֻ��ˮƽ��ֱ�������ƶ������ڵ�����*
*���磺*
```
A + - + -

- + - - +

- + + + -

+ - + - +

B + - + -
```
*��������*

*��һ����һ������ n����ʾ����Ĵ�С,4��n<100���������� n �У�ÿ���� n �����ݣ������� A��B��+��- �е�ĳһ�����м��ÿո�ֿ���A��B ��ֻ����һ�Ρ�*

*�������*
*���һ����������ʾ̹�˴� A ���� B ���������ƶ����������û�з���������� -1��*

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



*��5 ɨ��*
*��һ��n��m �еķ���ͼ����һЩλ���е��ף�����һЩλ��Ϊ�ա���Ϊÿ����λ�ñ�һ����������ʾ��Χ�˸����ڵķ������ж��ٸ�����*
*����*
```
3 4
0 1 0 0
1 0 1 0
0 0 1 0
```
*���*
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
		
		int sum=0;//ÿһ�δ�һ����sum��ʾ��ǰ��������
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