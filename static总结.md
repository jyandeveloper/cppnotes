### C++的static有两种用法：``面向过程程序设计``中的static和``面向对象程序设计``中的static。前者应用于普通变量和函数，不涉及类；后者主要说明static在类中的作用。

##1、面向过程程序设计中的static
###1.1 静态全局变量
在静态全局变量前加关键字static，该变量就被定义为一个静态全局变量。
```cpp
#include <iostream>
void f();
void main() {
    n = 20;
    cout << n << endl;
    f();
}
void f() {
    ++n;
    cout << n << endl;
}
```

静态全局变量有以下特点：

* 该变量在``全局数据区``分配内存；
* 未经初始化的静态全局变量会被程序``自动初始化为0``（自动变量的值是随机的，除非被显式初始化）；
* 静态全局变量在声明它的``整个文件都是可见的，而在文件外是不可见的``（静态全局变量不能被其它文件所用）；
* 其他文件中可以定义相同名字的变量，``不会发生冲突``。

```cpp
//file1
#include <iostream>
void f();
static int n;
void main()
{
    n = 20;
    cout << n << endl;
    f();
}

//file2
#include <iostream>
extern int n;
void f()
{
    ++n;
    cout << n << endl;
}
//编译运行通过，但运行时出错。将"static int n"改为int n；"再次编译运行程序正确，即使有extern也不行。
```