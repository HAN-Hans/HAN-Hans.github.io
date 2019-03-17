## C++基础

### 基本内置类型

有符号和无符号一起运算的时候，会转换成无符号数。`char`和`signed char`并不一样，具体由编译器决定是首位是有符号还是无符号

**避免无法预知和依赖于实现环境的行为**

### 变量

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

### 复合类型

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

### const 限定符

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
```



