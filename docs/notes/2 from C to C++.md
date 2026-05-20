
# string类

## 初识类
- 类是现实世界或抽象世界中概念，是关于同类事物定义的描述
- 计算机语言中类是模板，它是关于成员对象数据成员和函数成员的描述
- class是一种数据类型
## 字符串
- C++提供了两种类型的字符串表示形式：
  - C风格字符串
  - C++的string类 类型
- 为什么要引入string类型的对象？
  - C风格字符串定义时，必须事先知道保留多大空间存储字符串
  - 使用malloc动态空间处理字符串，对一般程序员来说太复杂，非常容易出现bug
- 在C++中，**string类**​ 是标准库（\<string\>头文件）提供的一个字符串类，用于方便、安全地处理和操作字符串。
## 创建string对象
- `string s1`是创建对象，不是声明变量。
- `string s2 = "c++"`**不是赋值运算**，它等价于`string s2("c++")`，它不会自动加`\0`，因为string类里存储了它的长度
## string对象的运算
- string类实现了以下运算符的重载：
  - 联接运算：operator+，连接两个字符串或者一个字符串和一个字符
  - 比较运算：operator==,!=,<,<=,>,>=，以字典序比较两个字符串
  - 下标运算：operator[]，访问指定字符，返回指定字符引用，即**可作为左值**
  - 赋值运算：operator=,+=，右操作数可以使字符串，字符，C字符串，字符数组
## string对象操作
- 基本操作：length/size, c_str
  -  `length()` 返回类型 size_t 的字符串的长度
  - `c_str()` 返回类型 const char * 的 C 风格字符串。以null结束
- 查找：find, rfind
  - `find(string|char*|char s [,int pos=0])` 寻找首个等于给定字符序列的子串。搜索始于  pos，返回类型 size_t 的位置。rfind 从右边开始搜索
- 添加，插入，删除：append, insert, erase, clear 
  - `append(string)` 后附 string str
  - `append(int count, char ch)` 后附 count 个字符 ch
  - `insert(int index, int count, char ch)` 在 index 位置插入 count 个 ch
  - `insert(int index,  string|char* s)` 在 index 位置插入 s
  - `erase(int   index, int   count)` 删 除 从 index 开始的 count 个字符
  - `clear()` 清空
- 提取子字符串：substr
  - `substr(int pos,  int  count)` 返回子串对象
- 比较：compare
  - `compare(string|char* str)` 与str比较，返回 1，0，-1之一
- 替换：replace
  - `replace(int pos, int count, string|char*|char s)` 替换指定范围内容，实际操作时发现：如果用的是char来替换，还需要在count后增加一个参数count2，为用来替换的长度
- 复制与交换：copy, swap
  - `copy(string|char*  dest,  int  pos,  int count)` 将子串复制到目标对象
  - `swap(string other)` 与 other 交换内容 
- 数值转换（C++11）：stoi, stod
## 补充
```cpp
for(int i = 0; i < s.length() - 2; i++){
    cout<< s[i];
}
```  
- 如果只输入了一个字符，输出将会出现很长的乱码，因为s.length()返回的是无符号整数，如果在减2之后变成负数，会自动转换成对应的无符号整数（值很大），所以会输出乱码。
```cpp
for (auto c : s) {
    // 对 c 进行操作
}
//等价于
for (size_t i = 0; i < s.size(); ++i) {
    char c = s[i];
    // 使用 c
}
```
- for (auto c : s)是 C++11 引入的基于范围的 for 循环，用于遍历容器或数组中的所有元素。
## 函数重载
- 在C语言中，一个函数只能定义一个函数名以及相关形参。C++ 则允许**多个函数拥有相同的名字**，只要它们的**参数列表不同**就可以，这就是**函数的重载（Function Overloading）**。借助重载，一个函数名可以有多种用途。
  1. 函数名相同。
  2. 参数列表必须不同（这是重载的关键）。
  3. 返回类型可以相同也可以不同，但仅返回类型不同不足以构成重载。
  4. 重载发生在同一作用域（通常是同一个类或同一个命名空间内）。
- 参数列表不同的三种情况
    1. 参数类型不同
    2. 参数个数不同
    3. 参数顺序不同（当类型不同时）
## 封装
- 通过类这个概念分离了开发者和使用者的世界，类是关于事物的外部属性和行为特征接口的描述。
- 指的是将数据和操作数据的方法捆绑在一起，并隐藏对象的内部实现细节，仅对外暴露有限的、受控的接口。封装的核心目的是信息隐藏。
# 使用C标准库
- 原本C的库以xxx.h命名的，C++把它们放在std空间里，改名为cxxx
- C 标准库，即不仅可以用 g++ 编译 c 风格的 cpp 程序，也可以使用 C 库函数
- 使用 getline 读入字符串到 s：`getline(cin,s)`可以读入一行，而`cin>>s`遇到空格就停止了
# const和constexpr
```cpp
#include <iostream>
#include <array>
using namespace std;
void dis_1(const int x){
    //错误，x是只读的变量
    array <int,x> myarr{1,2,3,4,5};
    cout << myarr[1] << endl;
}
void dis_2(){
    const int x = 5;
    array <int,x> myarr{1,2,3,4,5};
    cout << myarr[1] << endl;
}
int main()
{
    dis_1(5);
    dis_2();
}
```
- C语言中 const 关键字限定类型变量不可修改。在上示例代码中，dis_1() 和 dis_2() 函数中都包含一个 const int x，但 dis_1() 函数中的 x 无法完成初始化 array 容器的任务，而 dis_2() 函数中的 x 却可以。因为 const 语义有两个：
  - 限定为不可修改（只读）变量
  - 限定作为常量或字面量
- C++11 标准新添加的关键字 constexpr，声明编译时可以对函数或变量求值。即
  - 限定为常量表达式
  - 限定为编译时可优化执行的函数
- C++ 11标准中，为了解决 const 关键字的双重语义问题，保留了 const 表示“只读”的语义，而将“常量”的语义划分给了新添加的 constexpr 关键字。因此 C++11 标准中，建议将 const 和 constexpr 的功能区分开，即凡是表达“只读”语义的场景都使用 const，表达“常量”语义的场景都使用 constexpr。
# auto
- auto的原理就是根据初始化的表达式，在编译期间根据表达式类型自动推导出对象的类型。auto的作用就是为了简化变量初始化，如果这个变量有初始化表达式，就可以用auto代替类型声明。例如：
```cpp
auto x1 = 5; //等价于 int x1 = 5;
```
- 注意：
    1. 用auto声明的变量必须初始化
    2. 函数和模板参数不能被声明为auto
    3. 因为auto是一个占位符，并不是一个他自己的类型，因此不能用于类型转换或其他一些操作，如sizeof和typeid
    4. 定义在一个auto序列的变量必须始终推导成同一类型
```cpp
auto x1 = 5,x2 = 5.0,x3='r'; //这样写是不合法的，因为各个变量类型不同 
```