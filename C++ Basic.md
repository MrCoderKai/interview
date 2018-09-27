# Declaration & Definition

![delcaration & definition](figure/declaration_definition.png)

对于自定义类型，包括类（class）和结构体（struct），它们的定义都是放在.h文件中。其成员的声明和定义就比较复杂了，不过看上边的表格，还是比较清晰的。

**函数成员**

函数成员无论是否带有static限定符，其声明都放在.h文件的类定义内部。

对于要inline的函数成员其定义放在.h文件；其他函数的实现都放在.cpp文件中。

**数据成员**

数据成员的声明与定义都是放在.h文件的类定义内部。对于数据类型，关键问题是其初始化要放在什么地方进行。

1. 对于只含有static限定符的数据成员，它的初始化要放在**.cpp文件**中。因为它是所有类对象共有的，因此必须对它做合适的初始化。**这里的初始化，严格来说是为static数据成员分配内存。**

2. 对于只含有const限定符的数据成员，它的初始化**只能在构造函数的初始化列表**中完成。因为它是一经初始化就不能重新赋值，因此它也必须进行合适的初始化。

3. 对于既含有static限定符，又含有const限定符的数据成员，**它的初始化和定义同时进行**。它也是必须进行合适的初始化

4. 对于既没有static限定符，又没有const限定符的数据成员，它的值只针对本对象可以随意修改，因此我们并不在意它的初始化什么时候进行。



# static与const的区别
static

static局部变量 将一个变量声明为函数的局部变量，那么这个局部变量在函数执行完成之后不会被释放，而是继续保留在内存中

static 全局变量 表示一个变量在当前文件的全局内可访问

static 函数 表示一个函数只能在当前文件中被访问

static 类成员变量 表示这个成员为全类所共有

static 类成员函数 表示这个函数为全类所共有，而且只能访问静态成员变量

const

const 常量：定义时就初始化，以后不能更改。

const 形参：func(const int a){};该形参在函数里不能改变

const修饰类成员函数：该函数对成员变量只能进行只读操作

static关键字的作用：

（1）函数体内static变量的作用范围为该函数体，该变量的内存只被分配一次，因此其值在下次调用时仍维持上次的值； 

（2）在模块内的static全局变量和函数可以被模块内的函数访问，但不能被模块外其它函数访问； 

（3）在类中的static成员变量属于整个类所拥有，对类的所有对象只有一份拷贝； 

（4）在类中的static成员函数属于整个类所拥有，这个函数不接收this指针，因而只能访问类的static成员变量。

const关键字的作用：

（1）阻止一个变量被改变 

（2）声明常量指针和指针常量 

（3）const修饰形参，表明它是一个输入参数，在函数内部不能改变其值； 

（4）对于类的成员函数，若指定其为const类型，则表明其是一个常函数，不能修改类的成员变量； 

（5）对于类的成员函数，有时候必须指定其返回值为const类型，以使得其返回值不为”左值”。

# 不能重载的运算符只有5个：

.             （成员访问运算符）

.*            （成员指针访问运算符）

::             （域运算符）

sizeof    （长度运算符）

?:            （条件运算符）

# C++ STL中，基于红黑树实现的数据结构
set和multiset

# union和struct的区别
union的空间大小是最大元素的大小，而struct的空间大小是所有元素并考虑字节对齐的大小

# 线程和进程通信的方式

# OSI七层网络模型
应用层
表示层
会话层
传输层
网络层
数据链路层
物理层

# linux下安装g++的命令
sudo yum install g++

# overload, override和overwrite
1. overload（重载）: 发生在同一个作用域内，比如同一个类内部，或者同一个源文件内部，因此派生类函数重载基类函数的说法错误。其特点是，函数名相同，参数不同。注意参数有无const也视为不同
2. override（覆盖）: 发生在不同的作用域内，比如父类与子类之间。其特点是：函数名相同，参数也相同，基类函数需要virtual关键字修饰，子类有没有无所谓。
3. overwrite（隐藏）: 发生在不同的作用域内，比如父类与子类之间。其特点是：1. 同名不同参，无论有无virtual关键字；2. 同名同参，无virtual关键字。

覆盖的结果是根据对象类型决定调用哪个函数（多态）；而隐藏则表现为根据指针类型决定调用哪个函数，这是覆盖和隐藏最直观的区别。

# 操作符重载
1. operator<<重载。一般而言，要声明成类的友元函数，返回类型为ostream&

	friend ostream& operator<<(ostream&os, myClass &cls);

2. ->重载需要返回对象的指针，因为编译器会自动动绑定其成员。即(sp->())->action()。因此sp->()返回的是指针才可以。

3. 解引用与乘法重载的区别是，解引用是单目运算符，乘法是双目运算符。可通过是否含参来判断。

# 【C语言】数据结构和内存中的堆和栈
[【C语言】数据结构和内存中的堆和栈](https://blog.csdn.net/qq_41035588/article/details/81953424)

# git
rebase, checkout, -amend

# 线程同步机制
临界区，互斥量，事件对象，信号量

里面有更多、更详细的解答：[线程同步的四种方式](https://blog.csdn.net/guoxiang3538/article/details/79376191)

# 父类、子类之间的函数覆盖、隐藏问题

需要注意的事项

1. 析构函数需要使用virtual关键字
2. 主函数中使用智能指针，以防止忘释放new申请的空间导致内存泄漏
3. 代码中涉及到overload, override和overwrite的问题。

		#include <iostream>
		#include <vector>
		#include <string>
		#include <cmath>
		#include <memory>
		using namespace std;
		 
		class A{
		    public:
		        A(){cout << "A" << endl;}
		        virtual ~A(){cout << "~A" << endl;}
		        virtual void virtualFunction() {cout << "A::virtualFunction" << endl;} // overload
		        virtual void virtualFunction(int a) {cout << "A::virtualFunction(int a)" << endl;} // overload
		        virtual void virtualFunction(int a, int b) {cout << "A::virtualFunction(int a, int b)" << endl;} // overload
		        void virtualFunction(int a, double b) {cout << "A::virtualFunction(int a, double b)" << endl;}
		        void virtualFunction(const string s) {cout << "A::virtualFunction(const string s)" << endl;}
		        void normalFunction() {cout << "A::normalFunction" << endl;}
		};
		
		class B : public A {
		    public:
		        B(){cout << "B" << endl;}
		        virtual ~B(){cout << "~B" << endl;}
		        virtual void virtualFunction() {cout << "B::virtualFunction" << endl;}
		        virtual void virtualFunction(int a) {cout << "B::virtualFunction(int a)" << endl;} // 和A相比，发生了override
		        virtual void virtualFunction(int a, int b, int c) {cout << "B::virtualFunction(int a, int b, int c)" << endl;} // 不管有无virtual关键字，都与A类发送了overwrite，因为父类中没有该函数（包括参数）的声明，因此，无法通过父类指针来调用该函数
		        void virtualFunction(int a, double b) {cout << "B::virtualFunction(int a, double b)" << endl;} // overload
		        void normalFunction() {cout << "B::normalFunction" << endl;}
		};
		
		int main(){
		    A a;
		    B b;
		    cout << "derived to base cast" << endl;
		    shared_ptr<A> pa = make_shared<B>();
		    // shared_ptr<A> pa2(new A());
		    // shared_ptr<B> pb = dynamic_pointer_cast<B>(pa2); // not recommended
		
		    pa->virtualFunction();
		    pa->normalFunction();
		    pa->virtualFunction(1);
		    pa->virtualFunction(1, 2);
		    // pa->virtualFunction(1, 2, 3); // compliation error
		    cout << "Reference" << endl;
		    A& ra = b;
		    ra.virtualFunction();
		    ra.normalFunction();
		    ra.virtualFunction(1);
		    ra.virtualFunction(1, 2);
		    
		    cout << "overload, override, overwrite" << endl;
		    b.virtualFunction(); // override
		    b.virtualFunction(1, 1.0); // overwrite
		    b.virtualFunction(1, 2, 3); // overwrite
		    // b.virtualFunction("abc"); // compilation error, 原因，父类中的virtualFunction(const string s)因为不是virtual函数，被子类覆盖掉了.即使子类中没有void virtualFunction(int a, double b)也无法访问
		    // ra.virtualFunction(1, 2, 3); // compilation error, 原因同指针
		    // pb->virtualFunction(); // runtime error
		    // pb->normalFunction(); // runtime error
		    return 0;
		}


输出结果

		A
		A
		B
		derived to base cast
		A
		B
		B::virtualFunction
		A::normalFunction
		B::virtualFunction(int a)
		A::virtualFunction(int a, int b)
		Reference
		B::virtualFunction
		A::normalFunction
		B::virtualFunction(int a)
		A::virtualFunction(int a, int b)
		~B
		~A
		~B
		~A
		~A

