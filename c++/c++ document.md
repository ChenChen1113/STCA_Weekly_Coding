ms c++ document：https://docs.microsoft.com/zh-cn/cpp/cpp/destructors-cpp?view=vs-2019

### static的作用
1、加了static关键字的变量只能在本文件中使用，其他文件不能通过extern调用
2、类的成员变量和成员函数加了static之后就没有this指针了，必须通过类名访问


### c与c++的区别
1、c语言是面向过程的结构化变成语言，c++是面向对象的语言
2、C语言程序设计首要考虑的是如何通过一个过程，对输入进行一系列的运算处理得到输出
c++首要考虑的是如何构造一个对象模型，使之契合与之对应的问题域，这样就可以通过获取对象的状态信息得到输出或实现过程控制

### c++与java的区别
https://blog.csdn.net/qq_14982047/article/details/50769133
https://www.cnblogs.com/nucdy/p/6702726.html
1、java运行在虚拟机上，号称平台无关。c和c++都是编译成可执行文件，能否跨平台还要看编译器
2、c和c++是编译型的语言，将源代码编译成机器代码，所以效率高，但可移植性差
java是解释型语言，源代码被编译成二进制伪代码，由虚拟机解释执行，所以效率低，但具有良好的可移植性
编译一个普通的本地应用程序，c++要快于java，对于web应用，c的cgi程序是基本进程型，java servlet采用线程方式，所以在高并发的情况下，java有优势。
3、java运行在虚拟机上，编码时不需要考虑内存管理和垃圾回收，c++用到指针就一定要考虑内存申请和释放
4、java有个根类Object，所有的子类都继承于它，提高代码的重用性。
c++没有根对象，但c++提供了功能强大的模板，同样提高了代码的重用性。

### c++的四种cast
#### 1、static_cast
#### 2、dynamic_cast
#### 3、reinterpret_cast 什么都可以转
#### 4、const_cast 将常量转化为非常量，就可以赋值给非常量了

### c++指针和引用的区别
1、指针有一块自己的内存空间，而引用只是一个别名
2、使用sizeof查看指针的大小为4，引用的大小等于被引用对象的大小
3、指针可以初始化为NULL，引用必须引用一个对象
4、指针在使用过程中可以指向其他对象，而引用只能是一个固定对象的引用，不可改变
5、指针可以有多级指针**ptr，引用只能有移机

### c++的四种智能指针 （auto_ptr已经被c++11弃用）
智能指针主要用于管理在堆上分配的内存，它将普通的指针封装为一个栈的对象。
当栈对象的生命周期结束后，会在析构函数中释放掉申请的内存，从而防止内存泄露。
#### 1、unique_ptr 独占/严格拥有，保证同一时间只有一个智能指针可以指向该对象，不可拷贝，只能转移（move）所有权
目的：避免内存泄露，避免忘记delete而导致的内存泄漏
unique_ptr不可直接赋值 如p1=p2，这样会留下悬挂的p2，可以用move来实现unique_ptr的赋值，p1=move(p2)
可以将临时右值赋值给unique_ptr 如p1=unique_ptr<string>(new string("hello"))，因为临时右值转让所有权之后会被销毁

使用场景：
1）在new和delete之间可能发成异常，导致无法正常delete
就不要用int* p = new int(1)、delete p，用unique_ptr<int> p(new int(1))
2）在子函数内返回unique_ptr返回指针的所有权，外层函数结束后会自动释放资源
```c++
unique_ptr<int> Func(int p)
{
    unique_ptr<int> pInt(new int(p));
    return pInt;    // 返回unique_ptr
}

int main() {
    int p = 5;
    unique_ptr<int> ret = Func(p);
    cout << *ret << endl;
    // 函数结束后，自动释放资源
}
```
3）在容器中保存指针（将指针move到容器中）
```c++
int main(){
    vector<unique_ptr<int>> vec;
    unique_ptr<int> p(new int(1));
    vec.push_back(std::move(p));
}
```
4）动态管理数组
5）代替auto_ptr

#### 2、shared_ptr 多个智能指针指向相同对象，最后一个引用被销毁时释放
可复制

#### 3、weak_ptr 对对象的弱引用，不会更改引用计数，和shared_ptr之间可以相互转化
为了解决shared_ptr的循环引用
weak_ptr<int> fwPtr = sPtr; 用shared_ptr创建weak_ptr
shared_ptr<A> pa = wp.lock(); 返回weak_ptr指向的共享对象

### 野指针
1、指针没被初始化，不会指向null，会xjb指
2、free或delete之后，没有指向null，还是会xjb指，用ptr!=NULL完全不起作用

free释放的是内存，不是指针

### 内存泄露
是指堆内存的泄露
用动态存储分配函数动态开辟的空间，在使用完毕后未释放，导致一直占据该内存，直至程序结束，就是内存泄漏。
智能指针的内存泄漏情况：循环引用 （解决方法：weak_ptr）

### 析构函数
可以声明为虚函数，这样无需知道对象的类型即可销毁对象，使用虚函数机制调用该对象的正确的析构函数。
也可以声明为抽象类的纯虚函数。

构造函数的创建顺序：基类，类的非静态成员、子类
析构函数的析构顺序：子类，类的非静态成员、基类（不管虚不虚）
```c++
// order_of_destruction.cpp
#include <cstdio>

struct A1      { virtual ~A1() { printf("A1 dtor\n"); } };
struct A2 : A1 { virtual ~A2() { printf("A2 dtor\n"); } };
struct A3 : A2 { virtual ~A3() { printf("A3 dtor\n"); } };

struct B1      { ~B1() { printf("B1 dtor\n"); } };
struct B2 : B1 { ~B2() { printf("B2 dtor\n"); } };
struct B3 : B2 { ~B3() { printf("B3 dtor\n"); } };

int main() {
   A1 * a = new A3;
   delete a;
   printf("\n");

   B1 * b = new B3;
   delete b;
   printf("\n");

   B3 * b2 = new B3;
   delete b2;
}

Output: A3 dtor
A2 dtor
A1 dtor

B1 dtor

B3 dtor
B2 dtor
B1 dtor
```

默认析构函数不是虚函数的原因：
1、虚函数需要额外的虚函数表和虚表指针，占用额外内存
2、有的类不需要被继承，没必要虚

### 函数指针
```c++
int add(int a, int b);
int (*ptr)(int, int);
ptr = add;
```
作用：
1、将指针函数当做参数传给具有一定功能的模块，提高代码的灵活性和后期维护的便捷性。

### 常量的存储位置
局部常量：栈
全局常量：全局/静态存储区
字面值常量：常量存储区

### 拷贝构造函数不能是值传递
不能。如果是这种情况下，调用拷贝构造函数的时候，首先要将实参传递给形参，这个传递的时候又要调用拷贝构造函数。。如此循环，无法完成拷贝，栈也会满。

### vector和list
1）vector底层实现是数组；list是双向 链表。
2）vector支持随机访问，list不支持。
3）vector是顺序内存，list不是。
4）vector在中间节点进行插入删除会导致内存拷贝，list不会。
5）vector一次性分配好内存，不够时才进行2倍扩容；list每次插入新节点都会进行内存申请。
6）vector随机访问性能好，插入删除性能差；list随机访问性能差，插入删除性能好。 

使用场景：
vector拥有一段连续的内存空间，因此支持随机访问，如果需要高效的随即访问，而不在乎插入和删除的效率，使用vector。
list拥有一段不连续的内存空间，如果需要高效的插入和删除，而不关心随机访问，则应使用list。 

### 栈和队列的区别和应用场景
栈：回退功能（如浏览器）、括号匹配
队列：排队系统

### 哈希表的原理
通过key直接访问内存位置的数据结构。加快了查找速度。
冲突：key不同而value相同
解决冲突的方法：
1）开放地址法：hash = (hash(key) + d) mod len(table)，按照某种方法继续探测哈希表中的其他存储单元，直到找到空位置为止。
2）单独链表法：将value相同的元素保存在一个链表里
3）再散列
……

散列函数的原则：value不同 则key一定不同

### 多态是如何实现的

### 函数调用的时候都什么需要入栈

### 动态链接库和静态链接库
