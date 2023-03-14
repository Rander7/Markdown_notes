# 快速幂

## 使用快速幂解决大型幂运算问题

```
#include<iostream>
using namespace std;

long long fastpower(long long base,long long power)
{
	long long result=1;
	while(power)
	{
		if(power&1)
		{
			result=result*base;
		}
		power>>=1;
		base=(base*base);	
	} 
	return result;
}

```

**典型例题**

求A^B的最后三位数表示的整数

*注意求余的基本原理*

`(a + b) % p = (a % p + b % p) % p`

`(a - b) % p = (a % p - b % p ) % p `

`(a * b) % p = (a % p * b % p) % p `

使用第三条原理：
```
#include <iostream>
#include <cmath>
 
using namespace std;
 
/**
 * 普通的求幂函数
 * @param base 底数
 * @param power  指数
 * @return  求幂结果的最后3位数表示的整数
 */
long long fastPower(long long base, long long power) {
    long long result = 1;
    while (power > 0) {
        if (power & 1) {//此处等价于if(power%2==1)
            result = result * base % 1000;
        }
        power >>= 1;//此处等价于power=power/2
        base = (base * base) % 1000;
    }
    return result;
}
 
int main() {
    long long base, power;
    while (true) {
        cin >> base >> power;
        if (base == 0 && power == 0) break;
        cout << fastPower(base, power) << endl;
    }
    return 0;
 
}
```

## 详细原理附录：

3^10=3*3*3*3*3*3*3*3*3*3

//尽量想办法把指数变小来，这里的指数为10

3^10=(3*3)*(3*3)*(3*3)*(3*3)*(3*3)

3^10=(3*3)^5

3^10=9^5

//此时指数由10缩减一半变成了5，而底数变成了原来的平方，求3^10原本需要执行10次循环操作，求9^5却只需要执行5次循环操作，但是3^10却等于9^5,我们用一次（底数做平方操作）的操作减少了原本一半的循环量，特别是在幂特别大的时候效果非常好，例如2^10000=4^5000,底数只是做了一个小小的平方操作，而指数就从10000变成了5000，减少了5000次的循环操作。

//现在我们的问题是如何把指数5变成原来的一半，5是一个奇数，5的一半是2.5，但是我们知道，指数不能为小数，因此我们不能这么简单粗暴的直接执行5/2，然而，这里还有另一种方法能表示9^5

9^5=（9^4）*（9^1）

//此时我们抽出了一个底数的一次方，这里即为9^1，这个9^1我们先单独移出来,剩下的9^4又能够在执行“缩指数”操作了，把指数缩小一半，底数执行平方操作

9^5=（81^2）*(9^1)

//把指数缩小一半，底数执行平方操作

9^5=（6561^1）*(9^1)

//此时，我们发现指数又变成了一个奇数1，按照上面对指数为奇数的操作方法，应该抽出了一个底数的一次方，这里即为6561^1，这个6561^1我们先单独移出来，但是此时指数却变成了0，也就意味着我们无法再进行“缩指数”操作了。

9^5=（6561^0）*(9^1)*(6561^1)=1*(9^1)*(6561^1)=(9^1)*(6561^1)=9*6561=59049

我们能够发现，最后的结果是9*6561，而9是怎么产生的？是不是当指数为奇数5时，此时底数为9。那6561又是怎么产生的呢？是不是当指数为奇数1时，此时的底数为6561。所以我们能发现一个规律：最后求出的幂结果实际上就是在变化过程中所有当指数为奇数时底数的乘积

*[csdn快速幂]https://blog.csdn.net/qq_19782019/article/details/85621386*
