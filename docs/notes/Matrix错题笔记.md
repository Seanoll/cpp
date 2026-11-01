## 25程设Ⅱ-周1-理论题

### 面向对象 vs 面向过程

1. 核心对比

    | 维度 | 面向过程 (POP) | 面向对象 (OOP) |
    | :--- | :--- | :--- |
    | **思维** | **怎么做** (步骤/流程) | **谁来做** (对象/交互) |
    | **单元** | 函数 (Function) | 类/对象 (Class/Object) |
    | **数据** | 与函数分离，被动传递 | 与行为封装，主动处理 |
    | **特点** | 线性、高效、简单 | 模块化、安全、易扩展 |
    | **支柱** | 无 | 封装、继承、多态、抽象 |
    | **场景** | 嵌入式、脚本、底层驱动 | 大型系统、GUI、游戏、企业应用 |

2. 语言分类
      - **主要面向过程**：`C`, `Assembly`, `Fortran`, `Go` (偏过程)
      - **主要面向对象**：`Java`, `C#`, `Smalltalk`, `Ruby`
      - **多范式 (混合)**：`C++`, `Python`, `JavaScript`, `Swift`, `Kotlin`

3. 代码直觉
      - **POP**: `data` -> `function(data)` -> `result` (流水线)
      - **OOP**: `object.method()` (团队协作)

### 定点数表示法 浮点 vs 定点

1. 核心概念对比

    | 特性 | **浮点表示法 (Floating-Point)** | **定点表示法 (Fixed-Point)** |
    | :--- | :--- | :--- |
    | **定义** | 小数点位置**浮动**，由指数决定 | 小数点位置**固定**，通常在末尾 |
    | **本质** | 科学计数法 ($M \times 2^E$) | 整数缩放 (Integer $\times$ Scale) |
    | **C++ 操纵符** | `defaultfloat` (默认), `scientific` | `fixed` |

2. C++ 输出行为 (`setprecision(n)`)

    | 模式 | 代码示例 | `n` 的含义 | 示例输出 (值=12.34567) |
    | :--- | :--- | :--- | :--- |
    | **默认 (浮点)** | `cout << setprecision(6) << val;` | **总有效数字** (整数+小数) | `12.3457` (共6位) |
    | **科学 (浮点)** | `cout << scientific << setprecision(6) << val;` | **小数点后位数** | `1.234567e+01` |
    | **定点** | `cout << fixed << setprecision(6) << val;` | **小数点后位数** | `12.345670` (补零凑齐) |

3. 关键规则
      1.  **自动切换**：默认模式下，数值过大/过小会自动转为科学计数法 (`e+`)。
      2.  **强制定点**：使用 `fixed` 可禁止科学计数法，强制显示所有小数位。
      3.  **默认位数**：在 C++ 默认状态下（未使用 setprecision 修改），fixed 模式下的默认精度是 6 位小数。
### Q19
下述语句的输出是：
```cpp
cout << 1 + "20.26" << endl << 1 + 20.26;
```

- 陷阱：很多人以为 1 + "string" 会报错或进行字符串拼接，但实际上它执行的是指针偏移。  
- "20.26" 内存布局：['2', '0', '.', '2', '6', '\0']，指针 + 1：指向 '0'。  
cout 输出该指针指向的字符串："0.26"。

```text
0.26
21.26
```
### 命名冲突 变量隐蔽
下面代码在输入2以后，输出为？
```cpp
#include<iostream>
using namespace std; 
int main(){
	int cout;
	cin>>cout;
        std::cout<<(cout<<cout);
}
```

- 规则：局部变量的优先级高于命名空间中的名称。因此，在 main 函数的大括号 {} 内，标识符 cout 现在代表这个整型变量，而不是输出流对象。
- 如果你直接写 cout << ... 会报错，因为整数没有 << 重载用于输出。std::cout这里使用了作用域解析运算符 ::，明确指定使用标准库的 cout 输出流对象。这是合法的。
- 输出为8

## 25程设Ⅱ-周2-理论题

### 布尔类型
- **bool 类型的大小不一定为 1 字节**，在 C++ 标准中，bool类型的大小（sizeof(bool)）并没有明确规定必须是 1 个字节。虽然大多数现代编译器和平台上通常实现为 1 字节，但这并不是 C++ 语言标准强制要求的。其大小是实现定义的，可能因编译器、平台和优化设置而异。
- bool是 C++ 的基本数据类型，是语言的关键字之一，属于核心语言的一部分，**不需要包含任何头文件即可使用**。
### Q8
string::length()返回类型为 size_t，而不是 int。
### Q5
find的所有重载：

1. find (const string& str, size_t pos = 0)
2. find (const char* s, size_t pos = 0)
3. find (const char* s, size_t pos, size_t n)
4. find (char c, size_t pos = 0)

## 25程设Ⅱ-周4-理论题

### 构造析构函数个数
一个类可以拥有任意数量的构造函数，只要它们满足函数重载（Function Overloading）的规则。  
一个类只能有一个析构函数。
### 坑爹题
下列C++代码的输出是什么？
```cpp
#include <iostream>
using namespace std;
class TEST {
    int a, b, c; 
}
int main() {
    TEST test;
    cout << "In main()\n";
    return 0;
}
```
答案：编译错误
### 坑爹题2
下列C++代码的输出是什么？
```cpp
#include <iostream>
using namespace std;
class A{
private:
    int a, b, c; 
public:
    A(int a, int b, int c) {
        this->a = a;
        this->b = b;
        this->c = c;
        cout << "A" << endl;
    }
    ~A() {
        cout << "~A" << endl;
    }
};
int main() {
    A test;
    return 0;
}
```
答案：编译错误
### 坑爹题3
下列C++代码的输出是什么？
```cpp
#include <iostream>
using namespace std;
class A{
    int a, b, c; 
    A() {
        cout << "A1" << endl;
    }
    A(int a, int b, int c) {
        this->a = a;
        this->b = b;
        this->c = c;
        cout << "A2" << endl;
    }
    ~A() {
        cout << "~A" << endl;
    }
};
int main() {
    A test;
    return 0;
}
```
答案：编译错误
### Q6
在main函数结束时，对象会被销毁。析构的顺序与构造顺序相反。先构造的对象后销毁。
### Q12
- 析构函数的主要用途就是进行清理工作，最常见的情况就是释放对象在构造函数或生命周期内通过 `new` 等操作符动态分配的内存，以防止内存泄漏。它也可以用于关闭文件、断开网络连接等资源释放操作。
- 析构函数可以定义为 `private`。这通常用于实现单例模式或者禁止对象在栈上分配（强制使用智能指针或在堆上通过特定工厂方法创建），以防止对象被随意销毁。不过，如果析构函数是私有的，那么该类的对象就不能在全局作用域或普通局部作用域中正常销毁，需要友元类或静态成员函数来协助销毁。

## 25程设Ⅱ-周7-理论题
### Q12
```cpp
#include <iostream>
using namespace std;

class A {
public:
    A(int x) {}
};

int main() {
    A* p = new A[3];
    delete[] p;
    return 0;
}
```

- 在 C++ 中，当你使用 new A[n]（即 new[]）创建一个对象数组时，编译器会为数组中的每个元素调用默认构造函数（即无参构造函数或所有参数都有默认值的构造函数）。
- 但是，类 A 只定义了一个带参数的构造函数 A(int x)，这会导致编译器不再自动生成默认构造函数。
- 因此，new A[3] 会尝试调用 A()（默认构造函数），但该类没有默认构造函数，导致编译错误。

## 25程设Ⅱ-周8-理论题
### 重载`<<`运算符
在 C++ 中，我们希望自定义对象能像内置类型一样被输出，例如 `cout << obj`。

*   **左操作数是 `cout`**：`<<` 运算符的左侧是 `std::ostream` 对象（如 `cout`），右侧才是你的自定义对象。
*   **必须是全局函数**：因为左侧不是你的类对象，所以你**不能**将 `<<` 重载为成员函数（成员函数重载要求左侧必须是该类对象，那样会变成 `obj << cout`，不符合习惯）。
*   **需要访问私有成员**：既然必须在类外定义全局函数，如果该函数需要直接访问你类中的私有数据（如 `m_age`, `m_name`）来进行打印，它就没有权限。
*   **解决方案**：将该全局函数声明为类的**友元（friend）**，赋予它访问私有成员的“特权”。

所以重载`<<`用于输出自定义对象时，必须使用友元函数（或非成员函数）来重载运算符
### 检查自赋值

正确写法✅️

```cpp
if(this != &other)
```
不要写❌️

```cpp
if(*this != other)
```

| 比较方式 | 比较内容 | 性能 | 目的 |
| :--- | :--- | :--- | :--- |
| `if (this != &other)` | 对象的内存地址（身份） | 极快 | 检查是否为同一个对象，防止资源管理错误。 |
| `if (*this != other)` | 对象的成员变量值（内容） | 较慢 | 检查两个对象的逻辑值是否相等。 |

### Q7
在重载 operator+时，若将第二个参数声明为 const Integer& Int，这样做的主要目的是：

A.
允许传入右值（如 a + 3）

B.
防止函数内部意外修改 Int对象，并避免不必要的拷贝

正确答案是 **B. 防止函数内部意外修改 Int对象，并避免不必要的拷贝**

在 C++ 中重载运算符（特别是算术运算符如 `+`）时，将参数声明为 `const Integer&` 是一种标准的最佳实践，主要有两个核心目的：

1. **`const` 的作用（安全性）：** 加法操作 `a + b` 在逻辑上不应该改变操作数 `a` 或 `b` 的值。使用 `const` 修饰可以在编译阶段约束函数内部的行为，防止程序员在实现加法逻辑时意外修改了 `Int` 对象的值。如果尝试修改，编译器会直接报错。
2. **`&` 引用的作用（效率）：** 如果按值传递（即只写 `Integer Int`），在调用函数时会触发拷贝构造函数，产生一个临时的对象副本，这在对象较大或包含动态内存时会产生不必要的性能开销。使用引用 `&` 直接传递对象本身，避免了这次拷贝。

A 项：const T& 确实可以绑定右值，但这只是该语法的一个附带特性，并非主要目的。

### Q18
用户自定义的赋值运算符通常会将赋值运算符（`operator=`）的返回值设计为当前类的引用（`T&`）并返回 `*this`，这样设计的原因是

1. 支持链式赋值`a = b = c;` 根据右结合性，等价于 `a = (b = c);`。
2. 返回引用可以避免生成临时对象，从而提升性能。

## 25程设Ⅱ-周9-理论题
- 不仅全局函数可以做友元，别的类的成员函数也能声明为本类的友元。
```cpp
class A {
public:
    void accessB(B& b); 
};

class B {
public:
    // 将 A 类的 accessB 成员函数声明为友元
    friend void A::accessB(B& b); 
};
```

- 任何类型都可以作为运算符重载的返回类型，建议返回类型与内建运算符提供的返回类型相匹配。但`operator->`比较特殊，C++规定它必须返回指针 或 自带`operator->`的类对象。
- 重载 operator-> 时，不能带参数。因为编译器对 `->` 有**固定语法解析规则**：
    - 如果obj是指针，直接解引用并访问member
    - 如果obj是类对象且重载了operator->，则执行obj.operator->()，然后递归应用该规则
      ```cpp
      obj->member;
      ```
      会被自动翻译成：
      ```cpp
      obj.operator->()->member;
      ```
- 友元函数可以在类内部实现也可以在类外部实现，但它的本质依然是全局函数，而不是类的成员函数。

## 25程设Ⅱ-周12-理论题

### Q5

- 构造函数里可以调用普通成员函数，也能调用虚函数，但是不会触发派生类重写的虚函数，只会执行「当前正在构造的这个类」的版本。
- 静态成员函数不能是虚函数。静态函数不属于对象、没有`this`指针，无法通过对象虚表完成动态绑定。
  
### Q8

| 特性 | 编译时多态 (静态) | 运行时多态 (动态) |
| :--- | :--- | :--- |
| 实现方式 | 函数重载、模板函数 | 虚函数、重写(override) |
| 绑定时机 | 编译期（早期绑定） | 运行期（晚期绑定） |
| 核心机制 | 编译器根据参数/类型直接匹配 | 依靠虚函数表(vtable)和虚指针(vptr)动态查找 |

### Q10

- 函数覆盖/重写(override)必须函数签名完全一致，函数重载不允许签名一致。编译器不需要区分覆盖和被覆盖的函数。

### Q16

- 引用传递不会发生对象切片。例如
```cpp
class Base {
public:
    virtual void print() { std::cout << "Base\n"; }
};
class Derived : public Base {
public:
    void print() { std::cout << "Derived\n"; }
};
void call(Base& b) {
    b.print();
}
int main() {
    Derived d;
    call(d);
}
```
输出`Derived`

### Q17

- C++中实现运行时多态必须满足哪两个条件？

1. 基类函数声明为 **virtual 虚函数**
2. 调用时使用**基类指针 / 基类引用**访问

```cpp
Derived d;
Base *p = &d; // 指针只是指向原子类对象，没有新建对象、没有拷贝
p->print();

Base &r = d;  // 引用别名，依旧绑定原本的子类实体
r.print();
```

### 模板

```cpp
template<typename T>  // T 代表任意类型
T add(T a,T b){
    return a+b;
}
// 调用：自动匹配 int / double
add(1,2);
add(1.1,2.2);
```
属于编译时多态

### #pragma once

```
warning: #pragma once in main file
```
翻译：
**你在主文件（.cpp 源文件）里写了 #pragma once，编译器觉得没必要，但不会影响程序正常运行**


- `#pragma once` 是给**头文件 .h / .hpp** 用的
- 作用：**防止头文件被重复包含**
- 但 **.cpp 是源文件，不会被人 #include**

### 其他

- 派生类构造记得调用基类构造函数
- 拷贝构造记得用别人的公有函数调私有值
- 别忘了可以用using改变访问权限
- getinstance返回引用的时候如果是nullptr返回new instance，返回指针返回instance（应该无所谓）

## 25程设II-周14-课堂3

### to_string函数(C++11)

`to_string` 函数用于将数字转换为字符串。它可以处理多种数据类型，包括整数和浮点数。

```cpp
int num = 123;
std::string str = std::to_string(num); // "123"
double pi = 3.14159;
std::string str = std::to_string(pi); // "3.141590"
```

## 25程设Ⅱ-周14-理论题

### 接口风格抽象类

接口风格抽象类（类似于 Java 的 `interface` 在 C++ 中的映射）的核心设计意图是只定义行为规范，不携带状态。通常由纯虚函数组成，可以包含静态成员（静态成员属于类而非对象，不破坏"无状态"的接口语义），**不能有普通成员变量**，一旦有普通数据成员，就不再是 “接口风格”，而是普通抽象类。

接口风格抽象类因为没有数据成员，可以无歧义地被多重继承，也可以被虚继承。

## 25程设Ⅱ-周15-理论题

### 从属依赖类型

在 C++ 模板编程中，**从属依赖类型**（Dependent Type）是指**依赖于某个模板参数的类型**。

假设我们有一个模板参数 `T`，我们想使用 `T` 内部的一个类型 `iterator`：

```cpp
template <typename T>
void printFirst(T container) {
    // T::iterator 就是一个从属依赖类型
    // 因为 T 可能是 std::vector<int>（此时 T::iterator 是类型）
    // T 也可能是某个类，而 T::iterator 只是一个普通的静态成员变量
    T::iterator it = container.begin(); 
}
```

由于 `T` 是模板参数，编译器**在模板定义阶段无法确定** `T` 到底是什么类，因此默认按**变量/值**解析，导致把 `iterator` 当成静态变量，语法错误。

为了解决这种二义性，C++ 规定：当你在模板中访问一个从属依赖类型时，**必须在其前面显式加上 `typename` 关键字**。

```cpp
template <typename T>
void printFirst(T container) {
    typename T::iterator it = container.begin(); 
}
```

## 25程设Ⅱ-周15-课堂1

### 二进制字符串转数值

输入大端序（最高位先出现）可以采用逐位左移+或的方法：

```cpp
unsigned int val = 0;
for (char c : bits) {
    val = (val << 1) | (c - '0');
}
```

**类型双关**：需要将一段内存中的二进制数据，原封不动地解释为另一种类型（例如 uint32_t → float 或 int），而不改变位模式。

```cpp
float result;
memcpy(&result, &val, sizeof(result));
```

### 显式实例化

模板的定义通常需要放在头文件中，否则其他编译单元无法实例化。但有时出于编译速度或隐藏实现的目的，希望将模板定义放在 `.cpp` 文件中。

**头文件 (`utils.hpp`)**：
```cpp
template<typename T>
T decode_bits(const std::string& bits);

extern template int decode_bits<int>(const std::string&);
extern template float decode_bits<float>(const std::string&);
```

**源文件 (`utils.cpp`)**：
```cpp
#include "utils.hpp"
template<typename T>
T decode_bits(const std::string& bits) { /* 实现 */ }

// 显式实例化定义
template int decode_bits<int>(const std::string&);
template float decode_bits<float>(const std::string&);
```

原理：

- 当 `main.cpp` 调用 `decode_bits<int>(bits)` 时，编译器需要看到该函数的代码才能生成对应的机器指令。但由于定义在另一个 `.cpp` 文件中，`main.cpp` 编译时并不知道实现。
- `extern template` 告诉编译器**不要在此编译单元中隐式实例化**，该实例化在其他地方已存在。
- 在 `.cpp` 中提供普通实例化（无 `extern`），强制生成代码。

限制：失去对任意类型的自动适配能力，只能实例化**所有可能用到的模板参数组合**，否则链接时会缺少符号。适用于模板参数集合有限的情况（如本题仅 int, float, double）。

## 25程设Ⅱ-周15-课后2

C++ 标准规定，在类模板外部定义成员函数模板时，必须**先列出类模板的参数**，然后再列出函数模板自己的参数：

```cpp
// 正确：类模板参数（用于类） + 函数模板参数（用于成员函数）
template<typename T, int N>          // 对应 RangeFilter<T, N>
template<int Low, int High, int Window, int LevelID>   // 对应 isValid<...>
bool RangeFilter<T, N>::isValid(T val) { ... }
```

如果合并写成：
```cpp
template<typename T, int N, int Low, int High, int Window, int LevelID>
bool RangeFilter<T, N>::isValid(T val) { ... }
```

这会被编译器理解为：**这个 `isValid` 是一个普通函数模板（不属于 `RangeFilter` 类模板的成员）**，而左侧 `RangeFilter<T, N>::` 中的 `T, N` 并没有被定义（因为合并后的 `template` 中的 `T, N` 不会自动成为类模板参数），导致编译错误。

## 25程设Ⅱ-周15-课后1

在比较器是一个函数模板`template<typename T> bool cmp(const T& a, const T& b);`使用 `sort(data, data+count, cmp)` 时，编译器需要知道第三个参数的具体类型。`cmp` 是一个模板名，代表一组重载的函数（针对不同的 `T` 实例化）。编译器无法仅从上下文推断出应该实例化 `cmp` 的哪个版本（是 `cmp<int>` 还是 `cmp<double>` 等）时，报错 `unresolved overloaded function type`（无法解析的重载函数类型）。

而写成 `cmp<T>` 则明确告诉编译器：使用模板参数 `T` 来实例化 `cmp`，生成一个具体的函数 `cmp<T>`。总的来说，加上`<T>`是一个好习惯。

## 25程设Ⅱ-周16-课后4

### 字符串内部排序

```cpp
string s="listen";
sort(s.begin(),s.end()); //默认升序
```

## 25程设Ⅱ-周16-理论题

这一套选择题比较难。

### Q15

哪个 STL 容器在插入元素时可能导致迭代器失效？

正确答案是 **vector**。

`vector` 在内存中是连续分配的。当向其中插入元素且当前容量不足时，它会触发**扩容机制**（分配一块更大的新内存，将旧数据拷贝过去，然后释放旧内存）。由于底层物理内存地址发生了改变，这会导致 `vector` 中**所有的迭代器、指针和引用全部失效**。因此，`vector` 是选项中最容易因插入操作导致迭代器失效的容器。
