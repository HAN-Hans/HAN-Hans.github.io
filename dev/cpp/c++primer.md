# C++Primer

## 第1章 开始

## 第2章 变量和基本类型

### 2.1 基本内置类型

有符号和无符号一起运算的时候，会转换成无符号数。`char`和`signed char`并不一样，具体由编译器决定是首位是有符号还是无符号

**避免无法预知和依赖于实现环境的行为**

### 2.2 变量

变量的定义包括一个基本数据类型和一组声明符，C++的对象是指一块能存储数据并具有某种数据类型的内存空间

1、初始化：

定义到函数体内的内置类型的对象如果没有初始化，则其值未定义。累的对象如果没有显示的初始化，则其值由类确定
``` c++
int globalInt; // 0
std::string glocalStr;  // ""

int main(int argc, char const *argv[])
{
    int localInt;  // 未初始化，随机数
    std::string localStr;  // ""
}
```

列表初始化,C++11新引入

``` c++
int a = 0;
int a = {0};  // C++11
int a{0};
int a(0);

// 使用列表初始化可以初始化岑仔丢失信息的风险，编译器会报错
long double ld = 3.1415926536;
int a{ld}, b = {ld} // 错误：转换未执行，应为存在信息丢失的危险
int c(ld), d = ld;  // 正确：转换执行，且却是丢失了部分值
```

**初始化每一个内置类型的变量**

２、声明与定义

声明是的名字为程序所知，定义负责创建和名字相关实体，实现分离式编译。声明规定了变量的类型和名字，这点与定义相同。除此之外，定义还申请了存储空间，也可能为变量赋一个初始值
``` c++
extern int i;   // 声明未定义ｉ
int j;          // 声明并定义ｊ
extern double pi = 3.14;    // 定义pi，任意的包含显示初始化的声明即成为定义
```

**在第一次使用变量时再定义它**

３、作用域

可以使用作用域操作符来提取覆盖默认的作用域规则，如　`::` 操作符

### 2.3 复合类型

基于其他类型定义的类型。

１、引用

引用(reference)给对象另起一个名字，引用类型引用另一种类型，通过讲声明符写成`&d`形式定义引用类型。引用类型不是对象，定义时候需要初始化


``` c++
int a = 1024;
int &b = a;     // ｂ指向ａ，是ａ的另一个名字
int &c;         //　报错：引用必须被初始化
```

２、指针

指针(pointer)实现了对其他对象的间接访问，本身就是一个对象


``` c++
int a = 1024;
int *p = &a;     // p指向ａ
// 解引用符操作仅适用于确定指向某个对象的有效指针
std::cout << *p << std::endl;   // 1024

double b = 3.14;
p = &b;         //　报错：指针指向的对象类型和定义的类型需要一致

// 空指针，一下三种等价
int *p1 = nullptr;  // c++11标准，优先选用
int *p1 = 0;
int *p1 = NULL;     // 尽量避免

// 在定义时将类型名、修饰符、变量标识符关系
int* pn, pm;    // 应该尽量避免这种，容易将pm看成指针类型
int* pl;    // 如果要强调是复合类型，每个变量定义一行 
```

**初始化所有指针，**

3、指针与引用的区别：

- 引用不是对象，引用必须定义的时候初始化，并且不能更改；指针是对象，指针可以赋值和改变
- 无法定义指向引用的指针，但是可以定义指针的引用

**面对一条复杂的指针或引用的声明语句时，从右向左阅读有助于弄清楚他的真实含义**

### 2.4 const 限定符

const定义的常量的值不能改变，一旦创建后其值就不能改变，必须初始化

``` c++
const int i = get_size();   // 运行时初始化，不是常量表达式
const int j = 42;           // 编译时候初始化
const int k;                // 错误，未初始化

// const默认设定仅在文件内有效，如果要跨文件使用添加extern
extern const int bufSize = f(); // file.cpp定义一个常量，必须用extern被其他文件使用  
extern const int bufSize;   // file.h声明常量，extern限定并非本文件所独有

const int &rc = i;  // 常量引用
const int *pc1; // 常量指针
int const *pc2; // 指针常量
const int const *pc3;   // 指向常量的指针常量

// 常量表达式值不会改变并且在编译过程中就能得到计算结果的表达式
constexpr int mf = 20;  // c++11声明constexpr类型表示常量表达式
constexpr int limit = mf + 1;
```

### 2.5 处理类型

１、类型别名

``` c++
// typedef 类型别名，using　别名声明
typedef unsigned int uint;
using SI = Sales_item;

typedef char *pstring;
const pstring cstr = 0; // cstr指向char的常量指针
const pstring *ps;      // ps是一个指针，指向char常量指针
// 在理解用类型别名的声明语句时，千万不要用类型别名替换
const char *cstr = 0;   // 错误，变成指向常量的指针了
```

２、auto类型

C++11新标准引入auto类型说明符

``` c++
auto item = vall + val2;    // 由相加结果推断item类型
int i = 0, &r = i;
auto a = r;     // a是一个整数，i
// auto会忽略顶层const，保留底层const
const int ci = i;
auto d = ci;    // d是一个整数
auto e = &ci;   // e是一个指向常量的指针
const auto f = ci;  // 需要明确指明const
auto &h = 42;   // 错误，非常量引用不能绑定字面值
const auto &j = 42；   
```

3、decltype类型指示符

``` c++
decltype(f()) sum = x;    // 由相加结果推断item类型
int i = 0, &r = i;
auto a = r;     // a是一个整数，i
// auto会忽略顶层const，保留底层const
const int ci = i;
auto d = ci;    // d是一个整数
auto e = &ci;   // e是一个指向常量的指针
const auto f = ci;  // 需要明确指明const
auto &h = 42;   // 错误，非常量引用不能绑定字面值
const auto &j = 42；   
```

### 2.6 自定义数据结构

1、类

``` c++
struct Sales_data {
    std::string bookNo; // 默认初始化为对应类型零值
    unsigned units_sold = 0;    // c++11类内初始化
    double revenue = 0.0;
};
```

## 第3章 字符串、向量和数组

### 3.1 命名空间的 using 声明

using声明可以可函数的域操作符简化，直接使用声明符

``` c++
using namespace::name;
```

这样我们就可以直接使用name变量名，而不需要命名空间的前缀，且每个名字都需要单独的using声明

``` c++
#include <iostream>
using std::cin;
int main() 
{
    int i;
    cin >> i;       // cin和std::cin含义相同
    cout << i;      // 错误，没有using声明，必须完整名字
    std::cout <<i;  // 显示使用std中的cout
    std::cin >> i;  // 显示使用std中的cin
    return 0;
}
```

**头文件不应包含using声明**

接下来的的代码中的 `#include` 和 `using` 都省略

### 3.2 标准库类型 string

标准库类型 string 表示可变长字符序列，使用前必须包含 string 头文件。作为标准库一部分，string 定义在命名空间std中

``` c++
#include <string>
using std::string;
```

1、定义和初始化

``` c++ 
string s1;              // 空字符串
string s2 = s1;         // s2是s1的副本，拷贝初始化
string s2(s1);          // 同上，直接初始化
string s3 = "value";    // s3是字符串字面值的副本，拷贝初始化
string s3("value");     // 同上，直接初始化
string s4(10, 'c');     // cccccccccc，直接初始化
```

2、操作

- 读写 string 对象

string 对象会自动忽略开头的空白，直到下一处空白为止

``` c++ 
string word;
while (cin >> word) 
{
    cout << word << endl;
}
```

- getline 读取一整行

getline 从输入流中读入内容，直到换行符为止（注意换行符也读进来了），然后所读内容存到string对象中（注意不存换行符）

``` c++ 
string line;
while (getline(cin, line)) 
{
    cout << line << endl;
}
```

- empty 和 size

empty函数根据stirng对象是否为空返回一个布尔值，size函数返回string对象长度，返回类型是string::size_type是一种无符号类型，size最好不要使用int

- 比较 string 对象

string 类定义了几种比较字符串运算符，并且大小写敏感，依照字典顺序：

1. 如果两个string对象长度不同，而且较短string对象每个字符都相同，较短的小于较长的
2. 如果两个string对象对应位置不一致，则string对象第一对相异字符比较结果

- string 相加

将左侧对象与右侧对象拼接而成，是一个新的string对象

``` c++ 
string s1 = "hello", s2 = "world";
string s3 = s1 + s2;                // 两个string对象想加
string s4 = s1 + ", ";              // string对象和字面量相加
string s5 = "hello" + ", ";         // 错误，必须确保至少一个是string对象
string s6 = (s1 + ", ") + "world";  // s1+", "返回的是一个string对象
stirng s7 = "hello" + ", ", + s2;   // 错误，字面量不能直接相加
```

**为了与c兼容，c++的字符串字面值并不是string对象**

3、处理 string 对象中的字符

使用for循环处理sttring对象每个字符，使用下标运算符`[]`访问特定位置，接收参数也是string::size_type，也可以使用迭代器。同时配合cctype库进行处理

### 3.3 标准库类型 vector

标准库类型 vector 表示对象的集合，其中所有对象的类型都相同。也被称为容器

``` c++ 
#include <vector>
using std::vector;
```

1、定义和初始化

``` c++ 
vector<T> v1            // 空的vector，元素T类型，默认初始化
vector<T> v2(v1)        // v2包含v1所有元素副本
vector<T> v2 = v1       // 同上
vector<T> v3(n, val)    // v3包含n个重复元素，每个值都是val
vector<T> v4(n)         // v4包含n个重复执行初始化元素
vector<T> v5{a,b,c....} // v5包含初始值元素，每个元素赋予初始值
vector<T> v5={a,b,c...} // 同上

vector<int> ivec;           
vector<int> ivec2(ivec);
vector<string> svec(ivec2); // 错误，svec元素是string，ivec2是int

// 如果初始化使用列表初始化，但是又不能用来列表初始化，
// 就要考虑这样的值来构造vector对象
vector<string> v6{10};          // v6有10个默认初始化的元素
vector<string> v7{10， "hi"};   // v7有10个值为"hi"的元素
```

2、操作

- 添加元素

``` c++ 
vector<int> v2;
for (int i = 0; i != 100; ++i) 
    v2.push_back(i); 
```

**vector对象能高效增长，最好创建空的vector对象，运行时动态添加元素

- 其他 vector 操作

``` c++ 
vector<int> v;
v.empty()       // 判断是否为空
v.size()        // 元素的个数
v.push_back(t)  // 添加值为 t 的元素
v[n]            // 返回第 n 个位置上元素的引用
v1 = v2         // 用 v2 中的元素拷贝替换 v1 中元素
v1 = {a,b,c...} // 用列表中的元素拷贝替换 v1 中元素
v1 == v2        // v1 和 v2 相等且仅当他们元素数量相同对应位置元素值相同
<, <=, >, >=    // 以字典顺序进行比较
// 访问 vector 对象中元素
vector<int> v2{1,2,3,4,5,6,7,8,9};
for (auto &i: v)
    i *= i;
for (auto i: v)
    cout << i << " " << endl;
```

vector 和 string 很多功能都是相似的, 其中size返回的是vector中的size_type类型: vector<int>::size_type. vector 对象 以及 string 对象的下标运算符只能用于访问以及存在的元素, 而不能添加元素, 这种错误不会被编译器发现, 而是在运行时产生一个不可预知的值.    
 