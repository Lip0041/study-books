[题解参考网站](https://walkccc.github.io/CLRS/)：https://walkccc.github.io/CLRS/

Table of Contents
=================

   * [第一部分 基础知识](#第一部分-基础知识)
      * [第1章 算法在计算中的作用](#第1章-算法在计算中的作用)
      * [第2章 算法基础](#第2章-算法基础)
         * [插入排序](#插入排序)
            * [伪代码](#伪代码)
            * [C   实现](#c-实现)
            * [插入排序分析——Θ(n^2^)](#插入排序分析θn2)
            * [2.1-4 考虑把两个n位二进制整数加起来的问题，A B=C, C为n 1元数组。](#21-4-考虑把两个n位二进制整数加起来的问题abc-c为n1元数组)
         * [分析算法](#分析算法)
         * [设计算法](#设计算法)
            * [分治方法](#分治方法)
            * [归并排序](#归并排序)
               * [伪代码](#伪代码-1)
               * [C   实现](#c-实现-1)
               * [归并算法分析——Θ(n lgn)](#归并算法分析θn-lgn)
            * [2.3-5 二分查找迭代和递归伪代码——Θ(lgn)](#23-5-二分查找迭代和递归伪代码θlgn)
      * [第3章 函数的增长](#第3章-函数的增长)
      * [第4章 分治策略](#第4章-分治策略)
         * [最大子数组问题](#最大子数组问题)
            * [使用分治策略求解——Θ(n lgn)](#使用分治策略求解θn-lgn)
               * [求跨越中点最大子数组伪代码](#求跨越中点最大子数组伪代码)
               * [分治思想求数组最大子数组伪代码](#分治思想求数组最大子数组伪代码)
            * [4.1-5 最大子数组的迭代版本 伪代码实现——Θ(n)](#41-5-最大子数组的迭代版本-伪代码实现θn)
            * [最大子数组问题 C   实现（分治 迭代）](#最大子数组问题-c-实现分治迭代)
         * [矩阵乘法的Strassen算法](#矩阵乘法的strassen算法)
            * [常规的矩阵乘法：](#常规的矩阵乘法)
            * [一种简单的分治算法](#一种简单的分治算法)
            * [Strassen方法](#strassen方法)

# 第一部分 基础知识

## 第1章 算法在计算中的作用

+ **算法**简单来说就是能够正确将某一输入转换成特定输出的一系列计算步骤。

> **排序问题**形式定义：
> **输入：** n个数的一个序列<a1, a2, ..., an>。
> **输出：** 输入序列的一个排列<a1', a2', ..., an'>，满足a1' <= a2' <= ... <= an'。
> **影响因素：** 项数n、预先排序的程度、项值的限制、计算机的体系结构、存储设备的种类（主存、磁盘或者磁带）  

## 第2章 算法基础
### 插入排序
问题类型如上面引用的排序问题的形式定义，将待排序的数称为**关键词**。
+ 排序算法基本思想： 从待排序数组中拿出一个数（关键词），将该关键词跟已排序数组中的数进行比较，将其插入适当的位置。

#### 伪代码
```cpp
//按非降序排序
INSERTION-SORT(A)
	for j = 2 to A.length
		key = A[j]
		//Insert A[j] into the sorted sequence A[1..j-1]
		i = j-1
		while i>0 and A[i]>key
			//后移
			A[i+1] = A[i]
			i = i - 1
		//the key 入位
		A[i+1] = key
```
#### C++ 实现

```cpp
#include <iostream>
#include <vector>
using namespace std;

void InsertionSort(vector<int> &A)
{
    int n = A.size();
    int key;
    for(int j=1; j<n; ++j)
    {
        key = A[j];
        int i = j-1;
        while(i>=0 && A[i]>key)
        {
            A[i+1] = A[i];
            --i;
        }
        A[i+1] = key;
    }
}

int main()
{
    int n;
    cout << "Input the size of A:\n";
    cin >> n;
    vector<int> A(n);
    cout << "Input the n numbers of A:\n";
    for(int i=0; i<n; ++i)
        cin >> A[i];
    InsertionSort(A);
    cout << "After InsertionSort, the sequence of A:\n";
    for(int i=0; i<n; ++i)
        cout << A[i] << " ";
    return 0;
}
```
**运行结果**
![插排实现](https://img-blog.csdnimg.cn/20200408091723233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xpcDA0MQ==,size_16,color_FFFFFF,t_70)

#### 插入排序分析——Θ(n^2^)
**循环不变式与插入排序的正确性** 

> **循环不变式的证明**
>  1. **初始化：** 循环的第一次迭代前，循环不变式为真。
>  2. **保持：** 如果循环的某次迭代前循环不变式为真，那么下次迭代前循环不变式仍为真。
>  3. **终止：** 在循环终止时，不变式为我们提供一个有助于证明算法正确性的性质。

+ 插入排序的证明：
**初始化：**  证明第一次迭代(j=2)前，循环不变式成立。此时已排序数组A[1..j-1]仅由A[1]组成，该数组当然满足已排序的性质，即第一次循环迭代前循环不变式成立。
**保持：** 证明每次迭代保持循环不变式。在第j次循环迭代前，A[1..j-1]为已排序序列，此为真，当第j次循环迭代后（第j+1次循环迭代前）A[j]已经通过while语句找到了在A[1..j-1]中的恰当位置，并通过后移插入到合适位置，即A[1..j]为已排序序列，循环不变式成立。
**终止：** for循环终止条件是 j>A.length 即 j = n+1, 由循环不变式得，第n+1次循环迭代前，A[1..n]为已排序序列，即整个数组已被排序，插入排序算法正确。

#### 2.1-4 考虑把两个n位二进制整数加起来的问题，A+B=C, C为n+1元数组。

```cpp	
//伪代码实现
ADD_BINARY(A, B)
	C = new int[A.length + 1]
	carry = 0
	for i = 1 to A.length
		C[i] = (A[i] + B[i] + carry) % 2
		carry = (A[i] + B[i] + carry) / 2
	C[i] = carry
	return C
```

### 分析算法
**RAM模型** —— 包含计算机中的常见指令：算术指令（如加法、减法、乘法、除法、取余、向下取整、向上取整）、数据移动指令（装入、存储、复制）和控制指令（条件与无条件转换、子程序调用与返回。每个这种指令所需时间为常量。
一般考虑最坏时间复杂度Θ记号。
### 设计算法
- 插入排序算法使用了**增量**方法：在排序子数组A[1..j-1]后，将单个元素A[j]插入子数组的适当位置，产生排序号的子数组A[1..j]
#### 分治方法
**分治法**的思想：将原问题分解为几个规模较小但类似于原问题的子问题，递归地求解这些子问题，然后再合并这些子问题的解来建立原问题的解。

>分治模式再每层递归的三个步骤：
> 1. 分解原问题为若干子问题，为原问题规模较小的实例。
> 2. 解决这些子问题，递归地求解各个子问题。
> 3. 合并这些子问题的解成原问题的解。

 #### 归并排序

##### 伪代码
```cpp
//合并操作,p<=q<r, A[p..q]与A[q+1..r]均已排序
MERGE(A, p, q, r)
	n1 = q - p + 1
	n2 = r - q
	let L[1..n1+1] and R[1..n2+1] be new arrays
	for i = 1 to n1
		L[i] = A[p+i-1]
	for j = 1 to n2
		R[j] = A[q+j]
	//哨兵
	L[n1+1] = ∞
	R[n2+1] = ∞
	i = 1
	j = 1
	for k = p to r
		if L[i] <= R[j]
			A[k] = L[i]
			i = i + 1
		else
			A[k] = R[j]
			j = j + 1
```


```cpp
MERGE-SORT(A, p, r)
	//若p >= r 表示子数组中最多有一个元素，即已经排序好
	if p < r
		q = floor( (p+r) / 2 )//向下取整
		MERGE-SORT(A, p, q)
		MERGE-SORT(A, q+1, r)
		MERGE(A, p, q, r)
```

##### C++ 实现

```cpp
#include <iostream>
#include <vector>
using namespace std;
const int MAX = 1e6;
void Merge(vector<int> &A, int p, int q, int r)
{
    int n1 = q-p+1, n2 = r-q;
    vector<int> L(n1+1);
    vector<int> R(n2+1);
    for(int i=0; i<n1; ++i)
        L[i] = A[p+i];
    for(int j=0; j<n2; ++j)
        R[j] = A[q+j+1];
    L[n1] = R[n2] = MAX;
    int i = 0,  j = 0;
    for(int k=p; k<=r; ++k)
    {
        if(L[i] <= R[j])
            A[k] = L[i++];
        else
            A[k] = R[j++];
    }
}
void MergeSort(vector<int> &A, int p, int r)
{
    if(p < r)
    {
        int q = (p+r) / 2;
        MergeSort(A, p, q);
        MergeSort(A, q+1, r);
        Merge(A, p, q, r);
    }
}

int main()
{
    int n;
    cout << "Input the size of A:\n";
    cin >> n;
    vector<int> A(n);
    cout << "Input the n numbers of A:\n";
    for(int i=0; i<n; ++i)
        cin >> A[i];
    MergeSort(A, 0, n-1);
    cout << "After MergeSort, the sequence of A:\n";
    for(int i=0; i<n; ++i)
        cout << A[i] << " ";
    return 0;
}
```
**运行结果**
![归并排序](https://img-blog.csdnimg.cn/20200408120722587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xpcDA0MQ==,size_16,color_FFFFFF,t_70)
##### 归并算法分析——Θ(n lgn)
**循环不变式：** 再最后一个for循环的每次迭代时，子数组A[p..k-1]按照从小到大顺序包含L[1..n1+1]和R[1..n2+1]中的k-p个最小元素。进而，L[i]和R[j]是各自所在数组中未被复制回数组A的最小元素。	
>**证明：**
>1. 初始化：循环的第一次迭代前，k = p, 所以子数组A[p..k-1]为空，整个空的子数组包含L和R的 k-p = 0个最小元素，且i = j = 1, L[i], R[j]都是各自数组中未被复制回数组A的最小元素。循环不变式成立
>2. 保持：假设L[i] <= R[j]. 此时L[i] 为未被复制回A的最小元素，A[p..k-1]包含k-p个最小元素，L[i]复制后，子数组A[p..k]将包含k-p+1个最小元素。更新k和i后为下次迭代重新建立了循环不变式
>3. 终止：k = r + 1。由循环不变式，子数组A[p..k-1]即A[p..r]是按从小到大的循序包含L和R中k-p = r-p+1个最小元素。除了两个∞的哨兵外所有元素均复制回A中。算法正确

#### 2.3-5 二分查找迭代和递归伪代码——Θ(lgn)
在A[1..n]中找元素v，没有返回NIL
- 迭代版本
```cpp
ITERATIVE-BINARY-SEARCH(A, v, low, high)
	while(low <= hight)
		mid = floor((low + high)/2)
		if v == A[mid]
			return mid
		else if v > A[mid]
			low = mid + 1
		else
			high = mid - 1
		return NIL
```
+ 递归版本
```cpp
RECURISIVE-BINARY-SEARCH(A, v, low, high)
	if low > high
		return NIL
	mid = floor((low + high)/2)
	if v == A[mid]
		return mid
	else if v > A[mid]
		return RECURISIVE-BINARY-SEARCH(A, v, mid+1, high)
	else
		return RECURISIVE-BINARY-SEARCH(A, v, low, mid-1)
```

## 第3章 函数的增长
Θ： 渐近紧确界； O：渐近上界；Ω：渐近下界
> f(n) = O(g(n)) 类似于 a<=b
> f(n) = Ω(g(n)) 类似于 a>=b
> f(n) = Θ(g(n)) 类似于 a=b
> f(n) = o(g(n)) 类似于 a<b
> f(n) = ω(g(n)) 类似于 a>b

## 第4章 分治策略
### 最大子数组问题
#### 使用分治策略求解——Θ(n lgn)
分治求解子数组A[low..high]的最大子数组A[i..j]，满足 low <= i<= j <= high。若将A[low..high] 二分，取 mid = floor((low+high)/2)，那么i, j 的位置有三种情况：

1. 完全位于A[low..mid]中，low <= i <= j <= mid
2. 完全位于A[mid+1..high]中，mid+1 <= i <= j <= high
3.  跨越了中点mid，low <= i <= mid < j <= high

而求跨越中点的最大子数组，等于A[i..mid] + A[mid+1..j]
##### 求跨越中点最大子数组伪代码

 - 线性时间内完成Θ(n)

```cpp
FIND-MAX-CROSSING-SUBARRAY(A, low, mid, high)
	left_sum = -∞
	sum = 0
	for i = mid downto low
		sum = sum + A[i]
		if sum > left_sum
			left_sum = sum
			max_left = i
	right_sum = -∞
	sum = 0
	fir j = mid+1 to high
		sum = sum + A[j]
		if sum > right_sum
			right_sum = sum
			max_right = j
	return (max_left, max_right, left_sum+right_sum)
```
##### 分治思想求数组最大子数组伪代码

```cpp
FIND-MAXIMUM-SUBARRAY(A, low, high)
	if high == low
		return (low, high, A[low]) // only one element
	else 
		mid = floor((low+high)/2)
		(left_low, left_high, left_sum) = FIND-MAXIMUM-SUBARRAY(A, low, mid)
		(right_low, right_high, right_sum) = FIND-MAXIMUM-SUBARRAY(A, mid+1, high)
		(cross_low, cross_high, cross_sum) = FIND-MAX-CROSSING-SUBARRAY(A, low, mid, high)
		return max(left_sum, right_sum, cross_sum);
```

#### 4.1-5 最大子数组的迭代版本 伪代码实现——Θ(n)

```cpp
ITERATIVE-FIND-MAXIMUM-SUBARRAY(A)
	n = A.length
	max_sum = -∞
	sum = -∞
	for j = 1 to n
		current_high = j
		if sum > 0
			sum = sum + A[j]
		else
			current_low = j
			sum = A[j]
		if sum > max_sum
			max_sum = sum
			low = current_low
			high = current_high
	return (low, high, max_sum)
```
#### 最大子数组问题 C++ 实现（分治+迭代）

```cpp
#include <iostream>
#include <vector>
using namespace std;
const int MIN = -1e6;
void Find_crosssubarray(vector<int> arr, int low, int mid, int high, int &cross_low, int &cross_high, int &cross_sum)
{
    int left_sum = MIN, sum = 0;
    for(int i=mid; i>=low; --i)
    {
        sum += arr[i];
        if(sum > left_sum)
        {
            left_sum = sum;
            cross_low = i;
        }
    }
    int right_sum = MIN;
    sum = 0;
    for(int i=mid+1; i<=high; ++i)
    {
        sum += arr[i];
        if(sum > right_sum)
        {
            right_sum = sum;
            cross_high = i;
        }
    }
    cross_sum = left_sum + right_sum;
}
void DCmaxsubarray(vector<int> arr, int low, int high, int& low1, int& high1, int& max1)
{
    if(low == high)
    {
        low1 = high1 = low;
        max1 = arr[low];
        return;
    }
    else
    {
        int mid = (low+high) / 2;
        int left_low, right_low, cross_low, left_high, right_high, cross_high, left_sum, right_sum, cross_sum;
        DCmaxsubarray(arr, low, mid, left_low, left_high, left_sum);
        DCmaxsubarray(arr, mid+1, high, right_low, right_high, right_sum);
        Find_crosssubarray(arr, low, mid, high, cross_low, cross_high, cross_sum);
        if(left_sum >= right_sum && left_sum >= cross_sum)
        {
            low1 = left_low;
            high1 = left_high;
            max1 = left_sum;
        }
        else if(right_sum >= left_sum && right_sum >= cross_sum)
        {
            low1 = right_low;
            high1 = right_high;
            max1 = right_sum;
        }
        else
        {
            low1 = cross_low;
            high1 = cross_high;
            max1 = cross_sum;
        }
    }
    
}
void Itemaxsubarray(vector<int> arr, int &low, int &high, int &max)
{
    int n = arr.size();
    int max_sum = MIN;
    int sum = MIN;
    int current_low, current_high;
    for(int i=0; i<n; ++i)
    {
        current_high = i;
        if(sum > 0)
            sum += arr[i];
        else
        {
            current_low = i;
            sum = arr[i];
        }
        if(sum > max_sum)
        {
            max_sum = sum;
            low = current_low;
            high = current_high;
        }
    }
    max = max_sum;
}
int main()
{
    cout << "Input the size of the array and the array:\n";
    int n;
    cin >> n;
    vector<int> arr(n);
    for(int i=0; i<n; ++i)
        cin >> arr[i];
    int low1, low2, high1, high2, max1, max2;
    cout << "The max subarray:\n";
    cout << "\nDivide and Conquer:\n";
    DCmaxsubarray(arr, 0, n-1, low1, high1, max1);
    cout << "low:" << low1 << "\t" << "high:" << high1 << "\t" << " max:" << max1;
    cout << "\nIterative:\n";
    Itemaxsubarray(arr, low2, high2, max2);
    cout << "low:" << low2 << "\t" << "high:" << high2 << "\t" << " max:" << max2;
    return 0;
}
```
**运行结果**
![最大子数组](https://img-blog.csdnimg.cn/2020041315524791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xpcDA0MQ==,size_16,color_FFFFFF,t_70)
### 矩阵乘法的Strassen算法
#### 常规的矩阵乘法：

> C(n * n) = A(n * m) · B(m * n) 
	>  计算方法：$$c_{ij} = \sum_{k=1}^m a_{ik} * b_{kj}$$

伪代码：
```cpp
SQUARE-MATRIX-MULTIPLY(A, B)
	n = A.rows
	let C be a new n*n matrix
	for i = 1 to n
		for j = 1 to n
			cij = 0
			for k = 1 to m
				cij = cij + aik * bkj
	return C		
```
时间复杂度：O(n * n * m)，一般情况下m与n相差无几，所以时间复杂度为O(n^3^)

#### 一种简单的分治算法

为简化问题研究，这里我们假设A, B 均为方阵，即 m = n
> 假定n为2的幂，那么可以将n*n 分为4个n/2 * n/2 的子矩阵。计算C的公式变为
$$
\begin{gathered}
\begin{bmatrix} C_{11} & C_{12} \\ C_{21} & C_{22}  \end{bmatrix}\quad=
\begin{bmatrix} A_{11} & A_{12} \\ A_{21} & A_{22}  \end{bmatrix}\quad\cdot 
\begin{bmatrix} B_{11} & B_{12} \\ B_{21} & B_{22}  \end{bmatrix}
\end{gathered}
$$
等价于 
$$
	C_{11} = A_{11}\cdot B_{11} + A_{12}\cdot B_{21} \\
	C_{12} = A_{11}\cdot B_{12} + A_{12}\cdot B_{22}\\
	C_{21} = A_{21}\cdot B_{11} + A_{22}\cdot B_{21}\\
	C_{22} = A_{21}\cdot B_{12} + A_{22}\cdot B_{22}
$$

伪代码：
```cpp
SQUARE-MATRIX-MULTIPLY-RECURSIVE(A, B)
	n = A.rows
	let C be a new n*n matrix
	if n == 1
		c11 = a11 * b11
	else partition A, B, and C to 4 n/2 * n/2 matrixs
		C11 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A11, B11) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A12, B21)
		C12 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A11, B12) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A12, B22)
		C21 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A11, B11) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A22, B21)
		C22 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A12, B22) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A22, B22)
	return C
```
时间复杂度：
$$
T(n) = 
\begin{cases}
\Theta(1), & n = 1\\
8T(n/2) + \Theta(n^2), & n > 1
\end{cases}
$$
得出 T(n) = Θ(n^3^)
#### Strassen方法

