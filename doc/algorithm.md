
# 动态规划

## 例1  矩阵连乘问题

### 引入

**例如矩阵连乘积**
A<sub>1</sub>A<sub>2</sub>A<sub>3</sub>A<sub>4</sub>
**可以有下列5中完全加括号的方式：**
（A<sub>1</sub>(A<sub>2</sub>(A<sub>3</sub>A<sub>4</sub>)), (A<sub>1</sub>(A<sub>2</sub>A<sub>3</sub>)A<sub>4</sub>) , ((A<sub>1</sub>A<sub>2</sub>)(A<sub>3</sub>A<sub>4</sub>)) , ((A<sub>1</sub>(A<sub>2</sub>A<sub>3</sub>))A<sub>4</sub>) , (((A<sub>1</sub>A<sub>2</sub>)A<sub>3</sub>)A<sub>4</sub>)



**这里考察3个矩阵**
A<sub>1</sub>A<sub>2</sub>A<sub>3</sub>
**连乘的例子，设3个矩阵的维度分别为**


10\*100，100\*5 , 5\*50

| 加括号方式 | A<sub>1</sub>（A<sub>2</sub>A<sub>3</sub>） | （A<sub>1</sub>A<sub>2</sub>）A<sub>3</sub> |
| :--------: | ------------------------------------------- | ------------------------------------------- |
|   计算量   | 10\*100\*5+10\*5\*50                        | 100\*5\*50+10\*100\*50                      |
|    结果    | 7500                                        | 75000                                       |



**由此可见，计算矩阵连乘乘积的时候，加括号方式即计算次序对计算量有很大影响，于是，人们提出矩阵连乘积的最优计算次序问题，对于给定的n个矩阵**

A<sub>1</sub>A<sub>2</sub>A<sub>3</sub>.......A<sub>n</sub>

**如何确定计算次序使得矩阵连乘需要的数乘次数最少**



### 如何用动态规划解这道题

#### 分析最优解结构

**我们用A[ 1 : n ]来表示**

A<sub>1</sub>A<sub>2</sub>A<sub>3</sub>.......A<sub>n</sub>

**考察计算A[ 1 : n ]的最优计算次序，设这个计算次序在矩阵A<sub>k</sub>(1<=k<n),则其相应的完全加括号的方式为**

((A<sub>1</sub>.....A<sub>k</sub>)(A<sub>k+1</sub>An))

**依次次序，先计算A[1 : k]和A[k+1 : n]，再将计算结果相乘，得到A[1 ：n]。**

**能这么做的原因是最优计算次序的子结构也是最优子计算次序：我们可以假设最优计算次序A[1:n]的子结构A[1:k]不是最优计算次序，A1[1:k]才是最优计算次序，那么用A1[1:k]替换A[1:n]中的A[1:k]，会得到更少的次数，与A[1:n]最优矛盾**。



#### 建立递推关系

![](https://github.com/luoshiyong/CSinterview/tree/master/image/1.PNG)



**我们先看一看计算矩阵连乘积（用m数组表示A[i:j]数乘次数，P<sub>i</sub>表示A<sub>i</sub>的列数，也就是A<sub>i+1</sub>的行数）**

A<sub>1</sub>A<sub>2</sub>A<sub>3</sub>A<sub>4</sub>A<sub>5</sub>A<sub>6</sub>

**其中各矩阵的维数分别为**![2](https://github.com/luoshiyong/CSinterview/tree/master/image/2.PNG)

![3](https://github.com/luoshiyong/CSinterview/tree/master/image/3.PNG)

**由以上可知：(len为每一条对角线代表几个矩阵连乘，i,j为i,j角标变化范围)**

| len  |  i   |  j   |
| :--: | :--: | :--: |
|  2   | 1->5 | 2->6 |
|  3   | 1->4 | 3->6 |
|  4   | 1->3 | 4->6 |
|  5   | 1->2 | 5->6 |
|  6   | 1->1 | 6->6 |



#### 代码

**计算上述6个矩阵连乘**

```c++
#include<iostream>
using namespace std;
int  inf = 9999999999;
int p[7] = {30,35,15,5,10,20,25};    //A1到A6行列值
int n = 6;   //6个矩阵相乘
int m[7][7]={0};	//最小数乘次数
int s[7][7] = {0};	//m[i][j]在哪个位置截断
int main()
{
	for(int i = 0 ;i <= n;i++)
		for(int j = 0;j <= n;j++)
		{
				m[i][j] = 0;
		}
	for(int len = 2;len<=6;len++)
	{
		cout<<"len = "<<len<<endl;
		for(int i = 1;i <= n-1; i++ )  //计算5条斜对角线
		{
			int j = i+len-1;
			if(j>n)
				continue;
			cout<<"i = "<<i<<"   j = "<<j<<endl;
			if(i+1>j)continue;
				m[i][j] = m[i+1][j] + p[i-1]*p[i]*p[j];
			int k;
			for(int k = i; k<j; k++)//求何处分裂数乘次数最少
			{
				//在k处裂成两项
				int cur = m[i][k] + m[k+1][j] + p[i-1]*p[k]*p[j];
				if(cur<m[i][j])
				{
					m[i][j] = cur;
					s[i][j] = k;
				}
			}
		}
	}
	for(int i = 0 ;i <= n;i++)
	{
		cout<<endl;
		for(int j = 0 ;j <= n;j++)
		{
			cout<<m[i][j]<<"  ";
		}
	}
	return 0;
}

```

