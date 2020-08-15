# 第一章 程序设计：综述

## 数学知识复习

1. 在计算机科学中，除非有特别的声明，所有的对数都是以2为底的。

2. 级数： 
   $$
   \sum_{i=1}^Ni^k \approx \frac{N^{k+1}}{|k+1|}， k \neq -1
   $$

3. 

   调和级数:($H_N$为调和数)
$$
H_N = \sum_{i=1}^N \frac{1}{i} \approx log_eN = lnN
$$

3. 模运算

   如果N整除A-B，那么我们就说A与B模N同余(congruent)，记为A≡B(mod N)。

## C++类

1. C++中的类(class)是由它的成员(members)组成的。类的每一个实例都是一个对象(object)。每一个对象都包含有由类所指定的数据成分（除了static型）。成员函数(member function)用于处理对象，也被称为方法(method)。

2. public成员可以被任何类中的任何方法访问，private只能被它所在类中的方法访问。

3. 范围for语句(range for)：每一项均需要访问且不需要下标

   ```c++
   int sum = 0;
   //squares 为数组或vecotr对象
   for(auto x : squares)//此时x 是 squares中每一个值的拷贝
   	sum += x;
   ```

## C++细节

1. 左值引用的用途：(补充:左值引用&, 右值引用&&)

   1. 用于给结构复杂的名称起别名(alias)

      ```c++
      auto &aliasname = structurename;
      ```

   2. 范围for循环-修改值

      ```c++
      for(auto &x : arr) //引用arr中的每个值并自增一
          ++x;
      ```

   3. 避免复制-减小内存占用

      ```cpp
      //findMax函数返回arr中的最大值
      //如果我们仅是需要这个返回值，不需要更改这个返回值，则用左值引用可以避免新开辟一个空间x用来存储findMax的返回值
      auto &x = findMax(arr);
      ```

2. 参数传递-函数调用

   1. 对于小的不应被函数改变的对象，采用**传值调用**。

      ```cpp
      double average(double a, double b);
      ```

   2. 对于大的不应被函数改变且复制代价昂贵的对象，采用**传常量引用调用**。

      ```c++
      string randomItem(const vector<string> &arr);
      ```

   3. 对于所有可以被函数改变的对象，采用**传引用调用**。

      ```cpp
      void swap(double &a, double &b);
      ```

3. std::swap和std::move-采用移动替换复制使得效率提高

   1. 正常的通过3次复制进行的值交换——效率低

      ```c++
      void swap(vector<string> &x, vector<string> &y)
      {
      	vector<string> tmp = x;
      	x = y;
      	y = tmp;
      }
      ```

   2. 通过移动(move)可以避免复制——效率提升。

      移动(move)有两个要求：1. 数据类型支持移动 2. 等号右边为右值。

      ```cpp
      //这里vector支持移动，将右边数据变为右值可使用两种方法
      //第一种方法：强制类型转换为右值
      void swap(vector<string> &x, vector<string> &y)
      {
          vector<string> tmp = static_cast<vector<string> &&>(x);
          x = static_cast<vector<string> &&>(y);
          y = static_cast<vector<string> &&>(tmp);
      }
      //不过不够简洁
      //第二种方法：利用std::swap函数，其作用是使一个值易于移动，将左/右值转换为右值
      void swap(vector<string> &x, vector<string> &y)
      {
      	vector<string> tmp = std::swap(x);
          x = std::swap(y);
          y = std::swap(tmp);
      }
      ```



## 模版与五大函数(big-five)

见c++-primer-plus.md

# 第二章 算法分析

