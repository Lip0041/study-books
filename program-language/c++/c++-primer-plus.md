Table of Contents
=================

   * [C++预备概念](#c++预备概念)
   * [初入C++](#初入c++)
   * [复合类型： array &amp; vector &amp; string](#复合类型-array--vector--string)
   * [循环、分支语句 &amp; 关系、逻辑运算符](#循环分支语句--关系逻辑运算符)
   * [函数](#函数)

# C++预备概念

1. C++融合了三种不同的编程方式：C语言代表的过程性语言、类代表的面向对象语言、C++模版支持的泛型编程。

2. OOP强调是编程的数据方面，泛型编程强调的是独立于特定的数据类型。

# 初入C++

1. 变量命名规则推荐
   
    **str或sz**表示一空字符结束的字符串，**b**表示布尔值，**p**表示指针，**c**表示单个字符
    
2. 头文件`climits`中包含关于整型限制的信息（以符号常量形式定义）
	
	![截屏2020-08-10 09.43.59](https://tva1.sinaimg.cn/large/007S8ZIlly1ghlm3wiso7j30ln08p0vw.jpg)
	
3. C++特殊的初始化语法(syntax)——大括号初始化器（**列表初始化**）

    ```cpp
    type name{value};
    ```

4. sizeof 运算符可对类型名（放置括号中）或变量名（直接跟在`sizeof`）后面进行计算

    ![test1](https://tva1.sinaimg.cn/large/007S8ZIlly1ghlm6q16baj30f305zmxp.jpg)

    ![res1](https://tva1.sinaimg.cn/large/007S8ZIlly1ghlm71cjlyj30a302gaa0.jpg)

5. `cout << hex;` 不在屏幕上显示任何内容，只修改`cout`显示整数的方式（此为以十六进制方式显示）

6. 通常`cout`会删除结尾的零，使用`cout.setf()`可以覆盖这种行为，添加一条语句	

   ```cpp
   cout.setf(ios_base::fixed, ios_base::floatfield)；
   ```

   ![test2](https://tva1.sinaimg.cn/large/007S8ZIlly1ghlm7nuqwqj30ex075wf9.jpg)

   ![res2](https://tva1.sinaimg.cn/large/007S8ZIlly1ghlm7w6m0qj309l033mx5.jpg)

7. 求余操作%满足，(a/b) * b + (a%b) = a	

# 复合类型： array & vector & string

1. 只有在定义数组时才可以初始化整个数组，其他时刻不可，但可以使用下标分别给数组中的元素赋值
2. C++中可读取一行的函数有`cin.get()`和`cin.getline()`。
	
	> 二者区别：碰到空白——换行符，`getline()` 读取并用 '\0' 替换，而` get() `不读取不替换，留在输入队列中。
	
3. C++的string类——表示字符串的实体

    ```cpp
    //赋值、拼接操作
    string a, b;
    a = b; // 可以实现将一个string对象 b 赋给另一个string对象 a
    a += b; // 可以实现string对象 b 附加到string对象 a 的末尾
    //获取大小
    string str;
    len = str.size();	// 此语句就相当于C语言中的len = strlen(str1)
    //读入输入的字符串，根据输入字符串的长度自动调整自己的大小
    getline(cin, str); //可以获取一行字符串到string类 str 中
    //判断是否不一样	
    a != b; // a, b 至少有一个是 string 对象
    ```

4. C++允许在声明**结构变量（不是结构体）**的时候省略关键字`struct`，两个相同结构的结构变量可以直接赋值，提倡使用**外部结构声明**，不提倡使用外部变量

5. 使用 new 和 delete 时应当遵守的规则( malloc 和 free )

    > 1. 不要使用 delete 释放不是 new 分配的内存。
    >    2. 不要使用 delete 释放同一个内存块两次。
    >  3. 如果使用 new []为数组分配内存，则应使用 delete []来释放。
    >  4. 如果使用 new []为一个实体分配内存，则应使用 delete（没有方括号）来释放。
    >  5. 对空指针使用 delete 是安全的。

6. 数组名被解释为其第一个元素的地址，而对数组名应用地址运算符(&)时，得到的是整个数组的地址。

7. 使用数组声明来创建数组，`int data[10]; `将采用静态联编，即数组的长度在编译时设置
   使用 new[] 运算符创建数组时，`int *pz = new int [size]; `将采用动态联编，即在运行时为数组分配空间，其长度也在运行时设置，对应的用 delete [] 释放其占用的内存。  

8. 模板类`vector`是**动态数组**的替代品，`array`是**定长数组**的替代品。
   - 模板类`vector`: 声明创建一个名为vt的vector对象，可存储n_elem（可以是变量）个类型为typeName的元素

   		 ```cpp
   	vector<typeName> vt(n_elem);
   	```
   	
   - 模板类`array`: 声明创建一个名为arr的array对象，包含n_elem（不能是变量）个类型为typeName的元素
	
   		```cpp
   array<typeName, n_elem> arr;
   	```
   	
   - `array`对象和数组存储在相同的内存区域（栈），`vector`对象存储在自由存储区或堆中；可以将一个array对象赋给另一个array对象，而对于数组必须逐元素复制数据。

# 循环、分支语句 & 关系、逻辑运算符



1. 前缀格式（如 ++x ）比后缀格式（如 x++ ）效率高：
	- 前缀++：将值加1，然后返回结果
	- 后缀++：先赋值一个副本，将其加1，然后将入职的副本返回
	
2. 优先级：前缀递增、前缀递减和解除引用运算符优先级相同，以从右到左方式结合；后缀递增和后缀递减的优先级相同，但比前缀运算符优先级高，以从左到右方式结合。

3. 基于范围的 for 循环（简化了一种常见的循环任务：对数组或者容器类，如 vector 和 array 的每个元素执行相同的操作。

    ```cpp
    double prices[5] {4.99, 10.99, 6.87, 7.99, 8.49};
    for (double x : prices) //x最初表示prices的第一个元素,然后循环显示数组中的每个值
    	cout << x << std::endl;
    for (double &x : prices) //修改数组中的元素
    	x = x * 0.80;
    ```

    

4. `cin.get(ch)` 与 `cin.get()`
	
	|            属性            |             cin.get(ch)              |   ch = cin.get()    |
	| :------------------------: | :----------------------------------: | :-----------------: |
	|     传递输入字符的方式     |             赋给参数 ch              | 将函数返回值赋给 ch |
	| 用于字符输入时函数的返回值 | istream对象（执行bool转换后为true）  | int 类型的字符编码  |
	|   到达EOF时函数的返回值    | istream对象（执行bool转换后为false） |         EOF         |
	
5. 头文件` < cctype > `中的字符函数
	| 函数名称  |                            返回值                            |
	| :-------: | :----------------------------------------------------------: |
	| isalnum() |               如果参数是字母或者数字，返回true               |
	| isalpha() |                   如果参数是字母，返回true                   |
	| iscntrl() |                 如果参数是控制字符，返回true                 |
	| isdigit() |                   如果参数是数字，返回true                   |
	| isgraph() |          如果参数是除了空格之外的打印字符，返回true          |
	| islower() |                 如果参数是小写字母，返回true                 |
	| isprint() |           如果参数是打印字符（包括空格），返回true           |
	| ispunct() |                 如果参数是标点符号，返回true                 |
	| isspace() | 如果参数是标准空白字符，如空格、进纸、回车、制表符，返回true |
	| isupper() |                 如果参数是大写字母，返回true                 |

6. 文件 I/O 的主要步骤：（使用完文件用`close()` 方法关闭）

    >   1. 包含头文件 fstream
    >   2. 创建 ifstream 和 ofstream 对象; // ofstream outFile;
    >   3. 将 ifstream 和ofstream 对象同文件夹关联起来; // outFile.open("xxx.txt");
    >   4. 像使用 cin 和 cout 那样使用该 ofstream 对象
    > 5. 有一个 char 类型变量 ch，++ch的类型是 char ， ch + 1 是 int 类型

# 函数
1. 函数返回值类型不能是数组，但可以是其他任何类型——整数、浮点数、指针、结构、对象，其中可以将包含数组的结构或对象当作返回值。

2. 内联函数：以内存占用减少函数反复调用的开销，内联函数不能递归，一般用于小函数

   > 使用方法：在函数声明/定义前加上关键字inline.

3. 

