Table of Contents
=================

   * [第一章 绪论](#第一章-绪论)
      * [1.1 计算机与算法](#11-计算机与算法)
      * [1.2 复杂度度量](#12-复杂度度量)
      * [1.3 复杂度分析](#13-复杂度分析)
      * [1.4 递归与优化](#14-递归与优化)
   * [第二章 向量](#第二章-向量)

# 第一章 绪论
## 1.1 计算机与算法

 - 计算机科学核心在于**研究计算方法与过程的规律**。

 - 起泡排序：通过不断改善局部有序性实现整体的有序——不断交换相邻逆序对。
		
	即一般思路是对长度为n的序列，进行n-1趟局部有序性改善，这里可以通过一个检验变量记录一趟局部改善过程中是否存在逆序对，若不存在即已排序好。

	
	```cpp
	void bubblesort(int A[], int n)
	{
		bool sorted = false;//假定未排序
		while(!sorted)
		{
			sorted = true;//假定已经排序
			for(int i = 1; i < n; ++i)//从左至右检查当前范围A[0,n)内的各个相邻元素
			{
				if(A[i-1] > A[i])//发现逆序，则交换并标记未排序
				{
					swap(A[i-1], A[i]);
					sorted = false;
				}
			}
			--n;//末尾元素已就位，缩短检验序列长度
		}
	}
	```

- 算法：是指基于特定的计算模型，旨在解决某一信息处理问题而设计的一个指令序列。其具备的要素有：输入与输出、基本操作、确定性、可行性、有穷性与正确性
- 计算模型：图灵机、RAM模型，均具有无限的存储空间

## 1.2 复杂度度量
- 时间复杂度：特定算法处理规模为n的问题所需要的时间记为T(n)
	- 大O记号：上界
	- 大Ω记号：下界
	- 大Θ记号：同阶
## 1.3 复杂度分析

- 常数O(1)：一般不含转向算法（循环、调用、递归等）
- 常用级数：

	> 算数级数（与末项平方同阶）：T(n) =  1+2+...+n = O(n^2^)
	幂方级数（比幂次高一阶）：T(n) = 1+2^d^+3^d^+...+n^d^ = O(n^d+1^)
	几何级数（等比，a>1，与末项同阶）：T(n) = 1+a+a^2^+...+a^n^ = O(a^n^)
	收敛级数：T(n) = O(1)
	调和级数：T(n) = 1+1/2+1/3+...+1/n = O(log n)
	对数级数：T(n) = log(n!) = O(nlogn)

## 1.4 递归与优化

 - 递归改迭代：当算法是尾递归——算法（除平凡递归基外）任一实例都终止于该算法的递归调用 时，可以借助while语句改为迭代法
- 消除递归算法中的重复递归实例，可采取两种策略：
	> 制表(tabulation)或记忆(memoization)策略：从原问题出发，自顶而下，每当遇到一个子问题，都首先检查它是否计算过，以期望通过直接调阅记录获得解答
	动态规划(dynamic programming)策略：从递归基出发，自底而上递推地得出各个子问题的解，直至最终原问题的解。

# 第二章 向量
最基本的线性结构成为**序列(sequence)**。而序列又可分为**向量(vector)**和**列表(list)**。

- 在向量中，所有数据项的物理存放位置与其逻辑次序完全吻合，其逻辑次序也称为**秩(rank)**。
- 在列表中，逻辑上相邻的数据项在物理上未必相邻，采用间接定址的方式通过封装后的**位置(postion)** 相互引用。
