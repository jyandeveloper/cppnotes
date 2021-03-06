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

### 1.2 静态局部变量
在局部变量前加关键字static，该变量就被定义为一个静态局部变量。
```cpp
#include <iostream>
void f();
void main()
{
    f();
}

void f()
{
    static int n = 10;
    cout << n << endl;
    ++n;
}
```
静态局部变量保存在全局数据区，而不是保存在栈中，每次的值保持到下一次调用，直到下次赋新值。
静态局部变量有以下特点：

* 该变量在``全局数据区``分配内存；
* 静态局部变量在``程序执行到该对象的声明处时被首次初始化``，即以后的函数调用不再进行初始化；
* 静态局部变量一般在声明处初始化，如果没有显式初始化，会被程序``初始化为0``；
* 它始终驻留在全局数据区，直到程序运行结束。但其作用域为``局部作用域``，当定义它的函数或语句块结束时，其作用域随之结束。

### 1.3 静态函数
在函数的返回类型前加static关键字编程静态函数。静态函数与普通函数不同，它只能在声明它的文件中可见，不能被其它文件使用。
```cpp
#include <iostream>
static void f();    //声明静态函数

void main()
{
    f();
}

void f()    //定义静态函数
{
    int n = 10;
    cout << n << endl;
}
```
定义静态函数的``好处``：

* 静态函数不能被其它文件所用；
* 其它文件中可以定义相同名字的函数，不会发生冲突。

## 2、面向对象程序设计中的static
### 2.1 静态数据成员
在类内数据成员的声明前加static，该数据成员就是类内的静态数据成员。
```cpp
#include <iostream>
class myClass {
public:
    myClass(int a, int b, int c);
    void getSum();
private:
    int a, b, c;
    static int sum; //声明静态数据成员
};

int myClass::sum = 0;   //定义并初始化静态数据成员
myClass::myClass(int a, int b, int c)
{
    this->a = a;
    this->b = b;
    this->c = c;
    sum += a + b + c;
}
void myClass::getSum()
{
    cout << "SUM = " << sum << endl;
}

void main()
{
    myClass M(1, 2, 3);
    M.getSum();
    myClass N(4, 5, 6);
    N.getSum();
    M.getSum();
}
```
静态数据成员有以下特点：

* 对于非静态数据成员，每个类对象都有自己的拷贝。而``静态数据成员被当作是类的成员``。无论这个类的对象被定义了多少个，静态数据成员在程序中也``只有一份拷贝``，由该类型的所有对象``共享访问``。也就是说，静态数据成员是该类的所有对象所共有的。对该类的多个对象来说，静态数据成员``只分配一次内存``，供所有对象共用。因此，``静态数据成员的值对每个对象都是一样的，值可以更新``；
* 静态数据成员存储在``全局数据区``。静态数据成员``定义时要分配空间，所以不能在类声明中定义``。在上例中int myClass::sum = 0;是定义静态数据成员；
* 静态数据成员和普通数据成员一样遵从public, protected, private访问规则；
* 静态数据成员在全局数据区分配内存，属于本类的所有对象共享，所以它``不属于特定的类对象，在没有产生类对象时其作用域就可见，即在没产生类的实例时，我们就可以操作它``；
* 静态数据成员初始化与一般数据成员初始化不同。静态成员初始化的格式：<数据类型><类名>::<静态数据成员名> = <值>；
* 类的静态数据成员有``两种访问形式``。如果静态数据成员的访问权限允许的话（即public成员），可在程序中，按下列格式来引用静态数据成员：
    * <类对象名>.<静态数据成员名>
    * <类类型名>::<静态数据成员名>
* 静态数据成员主要用在各个对象都有相同某项属性的时候；
* 同全局变量相比，使用静态数据成员有两个优势：
    * 静态数据成员没有进入程序的全局名字空间。因此不存在与程序中其它全局名字冲突的可能性；
    * 可以实现信息隐藏。静态数据成员可以是private成员，而全局变量不能。
    
### 2.2 静态成员函数
与静态数据成员一样，我们也可以创建一个静态成员函数，它为类的全部服务而不是为某一个类的具体对象服务。静态成员函数与静态数据成员一样，都是类的内部实现，属于类定义的一部分。普通的成员函数一般隐含一个this指针，this指针指向类的对象本身，因为普通成员函数总是具体额属于某个类的具体对象。通常情况下，this是缺省的。如函数f()实际上是this->f()。但与普通函数相比，``静态成员函数``由于不是与任何对象相联系，因此它``不具有this指针``。从这个意义上讲，它``无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，它只能调用其余静态成员函数``。
```cpp
include <iostream>
class myClass
{
public:
    myClass(int a, int b, int c);
    static void getSum();   //声明静态成员函数
private:
    int a, b, c;
    static int sum; //声明静态数据成员
};
int myClass::sum = 0;   //定义并初始化静态数据成员

myClass::myClass(int a, int b, int c)
{
    this->a = a;
    this->b = b;
    this->c = c;
    sum += a + b + c;   //非静态成员函数可以访问静态数据成员
}
void myClass::getSum()  //静态成员函数的实现
{
    //cout << a << endl;    //错误代码，a是非静态数据成员
    cout << sum << endl;
}

void main()
{
    myClass M(1, 2, 3);
    M.getSum();
    myClass N(4, 5, 6);
    N.getSum();
    myClass::getSum();
}
```
静态成员函数有以下特点：

* ``出现在类体外的函数定义不能指定关键字static``；
* 静态成员之间可以互相访问，包括静态成员函数访问静态数据成员和访问静态成员函数；
* ``非静态成员函数可以任意访问静态成员函数和静态数据成员``；
* ``静态成员函数不能访问非静态成员函数和非静态数据成员``；
* 由于没有this指针的额外开销，静态成员函数与类的其余函数相比``速度``上会有少许``增长``；
* 调用静态成员函数，可以用成员访问符(.)和(->)为一个类的对象或指向类对象的指针调用静态成员函数，也可以直接使用如下格式调用静态成员函数：
    * <类名>::<静态成员函数名>(<参数表>)