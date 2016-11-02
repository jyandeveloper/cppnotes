## const
1、有时我们希望定义了一个变量后该值不能改变，用关键字``const``可以限定。const对象一旦创建后其值就不能再改变，所以<font color = red>const对象必须初始化</font>。<br>
2、``默认状态下，const对象仅在文件内有效``，编译器在编译过程中把用到该变量的地方都替换成对应的值。若要在其它地方使用，则对于const变量不管是声明还是定义都添加extern关键字即可。如：
```cpp
//file_1.cpp定义并初始化了一个常量，该常量能被其它文件访问
extern const int sizeBuffer = func();
//file_1.h头文件
extern const int sizeBuffer;  //与file_1.cpp中定义的sizeBuffer是同一个。
```
3、const引用<br>
（1）把引用绑定到const对象上，称之为``对常量的引用``（或``对const的引用``），简称<font color = red>常量引用</font>。
（2）引用的类型必须与其所引用对象类型一致。例外：初始化常量引用时允许任意表达式作为初始值，只要该表达式的结果能转换成引用类型即可，尤其允许为一个常量引用绑定非常量的对象、字面值，甚至是一个表达式。如：
```cpp
int i = 42;
const int &r1 = i;  //允许将const int&绑定到一个普通int对象上
const int &r2 = 42;  //正确：r2是一个常量引用
const int &r3 = r1 * 2; //正确：r3是一个常量引用
int &r4 = r1 * 2; //错误：r4是一个普通的非常量引用

double dval = 3.14;
const int &ri = dval; //正确
//编译器把上面两行代码变成如下形式：
const int temp = dval;  //由双精度浮点生成一个临时的整型常量
const int &ri = temp; //ri绑定了一个临时量对象
```
（3）const的引用可能引用一个非const的对象。

4、const指针
（1）``顶层const``表示指针本身是个常量（定向）；``底层const``表示指针所指对象是一个常量。更一般地，顶层const可以表示任意对象是常量，底层const则与指针和引用等符合类型的基本型部分有关。<br>
（2）<font color = red>常量指针必须初始化</font>，且一旦初始化完成，它的值（存放在指针中的那个地址）就不能改变了。<br>
（3）const左定值，右定向。<br>
（4）<font color = red>当执行对象的拷贝操作时，拷入和拷出的对象必须具有相同的底层const资格，或者两个对象的数据类型必须能够转换，一般来说，非常量可以转换成常量。</font>
