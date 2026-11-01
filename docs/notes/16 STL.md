## 容器适配器

容器适配器（Container Adaptor）提供顺序容器之上的不同功能接口（界面），对底层容器的接口进行封装和限制。

| 适配器 | 功能特点 | 头文件 |
| --- | --- | --- |
| `stack` | 栈，后进先出（LIFO） | `<stack>` |
| `queue` | 队列，先进先出（FIFO） | `<queue>` |
| `priority_queue` | 带优先级管理的队列，堆实现 | `<queue>` |

### stack

底层容器必须提供 `back()`、`push_back()`、`pop_back()`，标准容器 `vector`、`deque` 和 `list` 满足要求，默认使用 `deque`。

| 使用形式 | 操作效果 |
| --- | --- |
| `s.push(item)` | 压入栈顶，调用底层容器的 `push_back` |
| `s.pop()` | 删除栈顶，调用底层容器的 `pop_back` |
| `s.top()` | 返回栈顶元素，调用底层容器的 `back` |
| `s.empty()` | 判空 |
| `s.size()` | 返回元素数目 |

构造函数示例：

```cpp
std::stack<int> c1;            // 空栈
c1.push(5);
std::stack<int> c2(c1);       // 拷贝构造
std::deque<int> deq {3, 1, 4, 1, 5};
std::stack<int> c3(deq);      // 用底层容器构造
std::stack<char, std::vector<char>> cstack;  // 指定底层容器类型
```

### queue

| 使用形式 | 操作效果 |
| --- | --- |
| `q.push(item)` | 插入队尾，调用 `push_back` |
| `q.pop()` | 删除队首，调用 `pop_front` |
| `q.front()` | 返回队首元素 |
| `q.back()` | 返回队尾元素 |
| `q.empty()` | 判空 |
| `q.size()` | 返回元素数目 |

### priority_queue

| 使用形式 | 操作效果 |
| --- | --- |
| `pq.push(item)` | 按优先级插入，先 `push_back` 再 `push_heap` |
| `pq.pop()` | 删除队首（优先级最高），先 `pop_heap` 再 `pop_back` |
| `pq.top()` | 返回队首元素 |
| `pq.empty()` | 判空 |
| `pq.size()` | 返回元素数目 |

### 适配器与底层容器

| 适配器 | 可用底层容器 | 默认底层容器 |
| --- | --- | --- |
| `stack` | `vector`、`list`、`deque` | `deque` |
| `queue` | `list`、`deque` | `deque` |
| `priority_queue` | `vector`、`deque` | `vector` |

> `queue` 要求底层容器提供 `pop_front`，因此不能基于 `vector`。
> `priority_queue` 需要随机访问能力进行堆操作，因此不能基于 `list`。

## 迭代器类别

STL 迭代器按功能从弱到强分为五类：

| 类别 | 读写能力 | 支持的操作 | 典型容器 |
| --- | --- | --- | --- |
| 输入迭代器（input iterator） | 只读 | `*`（只读）、`->`、`++`、`==`、`!=` | `istream_iterator` |
| 输出迭代器（output iterator） | 只写 | `*`（只写）、`->`、`++` | `ostream_iterator` |
| 正向迭代器（forward iterator） | 读/写 | `++`、`==`、`!=` | `forward_list` |
| 双向迭代器（bidirectional iterator） | 读/写 | `++`、`--`、`==`、`!=` | `list`、`set`、`map` |
| 随机访问迭代器（random access iterator） | 读/写 | `++`、`--`、`+`、`-`、`+=`、`-=`、`[]`、比较运算 | `vector`、`deque`、`array` |

> `iter[n]` 等价于 `*(iter + n)`

### 各容器对应的迭代器类别

| 容器 | 迭代器读写 | 迭代器类别 |
| --- | --- | --- |
| `array` | 读/写 | 随机访问 |
| `vector` | 读/写 | 随机访问 |
| `deque` | 读/写 | 随机访问 |
| `list` | 读/写 | 双向 |
| `set` / `multiset` | 只读 | 双向 |
| `map` / `multimap` | 只读 | 双向 |
| `forward_list` | 读/写 | 正向 |

> `const` 容器（如 `const vector<int>`）的迭代器解引用结果只能作为右值（只读）。
> 关联容器（`set`/`map`）的迭代器即使对非 `const` 容器也是只读的，因为修改元素会破坏有序性。

### 迭代器遍历

```cpp
// 传统遍历
for (auto it = c.begin(); it != c.end(); ++it) {
    auto x = *it;
    cout << x << ",";
}

// C++11 range-based for
for (auto x : c) {
    cout << x << ",";
}

// 推荐范式：const引用避免拷贝
// 需要修改容器元素时需要加引用，不要加const
for (const auto& x : c) {
    cout << x << ",";
}
```

### 迭代器失效

**只读方法决不非法化迭代器或引用。修改容器内容的方法可能非法化迭代器和/或引用。**

典型错误——删除 `vector` 中所有值为 3 的元素：

```cpp
vector<int> ivv1 {1, 2, 3, 4, 3, 3};
for (auto it = ivv1.begin(); it != ivv1.end(); it++) {
    if (*it == 3) ivv1.erase(it);   // erase 后 it 失效！
}
```

正确写法：利用 `erase` 返回的指向下一元素的迭代器：

```cpp
for (auto it = ivv1.begin(); it != ivv1.end(); ) {
    if (*it == 3)
        it = ivv1.erase(it);   // erase 返回下一元素的迭代器
    else
        ++it;
}
```

## 可调用类型

可调用（Callable）类型是可以像函数一样被调用的对象，包括：

1. **普通函数**（含类的静态成员函数）及其指针
2. **类的非静态成员函数**及其指针
3. **函数对象**（重载了 `operator()` 的类）
4. **Lambda 表达式**（匿名函数）

### 函数指针回调

用函数指针实现回调：

```cpp
typedef int (*FP)(int, int);  // 函数指针类型

int Add(int lhs, int rhs) { return lhs + rhs; }
int Mul(int lhs, int rhs) { return lhs * rhs; }

void callback_client(FP f, int a, int b) {
    std::cout << f(a, b) << std::endl;
}

int main() {
    callback_client(Add, 2, 3);  // 输出 5
    callback_client(Mul, 2, 3);  // 输出 6
}
```

### 类成员函数回调

**静态成员函数**可以像普通函数一样用函数指针回调；**非静态成员函数**需要使用成员函数指针，且必须绑定到具体对象：

```cpp
class Foo {
public:
    int Add(int lhs, int rhs) { return lhs + rhs; }       // 非静态
    static int Mul(int lhs, int rhs) { return lhs * rhs; } // 静态
};

// 非静态成员函数指针：需要传入对象
void callback_client(int (Foo::*f)(int, int), Foo &foo, int a, int b) {
    std::cout << (foo.*f)(a, b) << std::endl;
}
```

> 成员函数指针语法较为繁琐，且不同类的成员函数指针类型不同，难以统一处理。

### std::function 和 std::bind

`std::function` 是通用的函数包装器，可以包装任何可调用对象。`std::bind` 可以将成员函数与对象绑定，生成可调用对象。

```cpp
#include <functional>

typedef std::function<int(int, int)> FP;

class Foo {
public:
    int Add(int lhs, int rhs) { return lhs + rhs; }
    static int Mul(int lhs, int rhs) { return lhs * rhs; }
    int operator()(int lhs, int rhs) { return this->Add(lhs, rhs); }  // 函数对象
};

void callback_client(FP f, int a, int b) {
    std::cout << f(a, b) << std::endl;
}

int main() {
    using namespace std::placeholders;  // _1, _2, ...

    Foo foo;
    callback_client(Foo::Mul, 2, 3);              // 静态成员：直接用
    auto f = std::bind(&Foo::Add, &foo, _1, _2);  // bind 绑定非静态成员
    callback_client(f, 2, 3);
    callback_client(foo, 2, 3);                    // 函数对象，更方便
}
```

### Lambda 表达式

Lambda 表达式（C++11）是一种**匿名函数**，可以在代码中就地定义可调用对象。

**语法格式：**

```cpp
[捕获列表](参数列表) -> 返回类型 { 函数体 }
```

- **返回类型**可以省略，由编译器自动推导
- **捕获列表**决定如何使用外部变量

**捕获方式：**

| 捕获方式 | 含义 |
| --- | --- |
| `[]` | 不捕获任何外部变量 |
| `[=]` | 按值捕获所有外部变量 |
| `[&]` | 按引用捕获所有外部变量 |
| `[x]` | 按值捕获变量 `x` |
| `[&x]` | 按引用捕获变量 `x` |
| `[=, &x]` | 按值捕获所有，但 `x` 按引用 |
| `[&, x]` | 按引用捕获所有，但 `x` 按值 |

示例：

```cpp
auto f1 = []{ return "hello lambda!"; };

int i = 3;
auto f2 = [&i](int a) { i++; return a + i; };  // 按引用捕获 i
std::cout << i << "-" << f2(5) << std::endl;   // 3-9（i 变为 4）
```

> 按引用捕获时，Lambda 内部可以修改外部变量；按值捕获时，Lambda 内部修改的是副本，不影响外部变量。

## STL算法

STL 算法定义在 `<algorithm>` 和 `<numeric>` 头文件中，通过迭代器对容器元素进行操作，与容器类型解耦。

### sort

对区间 `[first, last)` 内的元素排序，默认升序。

```cpp
#include <algorithm>

vector<int> vec = {13219, 1203, 3213, 4132};
sort(vec.begin(), vec.end());  // 升序
```

自定义排序规则：传入比较函数（可调用对象），返回 `true` 表示第一个参数应排在前面。

```cpp
sort(vec.begin(), vec.end(), [](int a, int b) { return a > b; });  // 降序
```

### count_if

统计区间 `[first, last)` 中满足谓词的元素个数。

```cpp
vector<int> vec = {1, 10, 20, 15, 30};
auto greater_than_20 = count_if(vec.begin(), vec.end(), [](int i) { return i > 20; });  // 1
auto greater_than_15 = count_if(vec.begin(), vec.end(), [](int i) { return i > 15; });  // 2
```

### accumulate

对区间 `[first, last)` 中的元素进行累积操作，初始值为 `init`。

```cpp
#include <numeric>

vector<int> v = {1, 2, 3, 4};
// 默认累加
auto sum = accumulate(v.begin(), v.end(), 0);  // 0 + 1 + 2 + 3 + 4 = 10

// 自定义累积操作：累乘
auto product = accumulate(v.begin(), v.end(), 1,
    [](decltype(v[0]) i, decltype(v[0]) j) { return i * j; });  // 1*2*3*4 = 24
```

> `accumulate` 的第三个参数（初始值）决定了运算的返回类型和初始状态。累乘时初始值应为 `1` 而非 `0`。
> `decltype(v[0])` 用于推导元素类型，确保 Lambda 参数类型与容器元素类型一致。

## 编译建议

STL 基于模板实现，以头文件方式分发，`#include` 会导致大量代码展开。

- **绝对不要在工程项目中使用 `#include <bits/stdc++.h>`**（这是非标准的 GCC 扩展）
- 只引入需要的头文件，减少编译时间
- C++20 引入了 Module 系统（`import std.core;`），可解决头文件编译速度问题，目前 MSVC 已率先支持
