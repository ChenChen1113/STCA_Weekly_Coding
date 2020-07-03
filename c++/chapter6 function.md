# 6 函数

### 局部静态变量
```C++
size_t count_calls(){
    static size_t ctr = 0;
    return ++ctr;
}
```
如上述程序，程序第一次执行ctr=0时对其进行初始化。
之后调用该函数，都会对ctr+1，ctr会生存到程序结束。


### 函数声明
.h文件里的那些，不包括函数体，所以形参无实际意义
为了理解函数功能，往往加上形参


### 指针形参
```C++
void reset(int *p){
    *p = 0; //指针指向的东西可以改
    p = 0;  //只在函数内部有效
}
```


### 传引用的作用
1、可修改
2、避免拷贝（与const配合使用更佳）
3、使函数返回多个值


### 使用常量引用
如果函数定义为 find_str(string &str)，则不能用作find_str("hello world")，因为字面值不可以传给普通引用。

### 数组传参 传长度的三种方法
1）字符串的结尾有明显标记（空串）
```c++
    int array[] = {1, 2, 3};    //输出 1 2 3 -858993460 469046760 18085964 1191326 1 21713888 21734480 18086056 1190919 469046648 1184319 1184319 16551936
    char array1[] = {'1', '2', '3'}; //输出 1 2 3 ╠ ╠ ╠ ╠ ╠ ╠ ╠ ╠ ╠ ╔
    char array2[] = {'1', '2', '3', '\0'};  //输出 1 2 3
    char* str = "123";  //输出 1 2 3
    int* ptr = &array[0];
    while(*ptr){
        cout << *ptr++ << " ";
    }
```

2）c++11标准库函数，begin()和end()分别获取首指针和结尾元素下一位置指针
```c++
void print(const int* begin, const int* end){
    while(begin != end){
        cout << *begin++ << " ";
    }
}

print(begin(array), end(array));
```

3）传一个size
```c++
void print(const int* begin, size_t size){}
```

### 多维数组传参
```c++
int array[] //int数组
int* array[]    //int指针数组
int &array[]    //int引用数组

int (*array)[10]    //指向含有10个整数的数组的指针

//以下两种等价：
void print(int (*matrix)[10], int rows)
void print(int matrix[][10], int rows)
//第二维的“10”是数组类型的一部分，不能省略
```

### initializer_list传多个相同类型的参数
与vector不同的是，initializer_list存的是常量值


### 函数返回引用
```c++
const string &shorter(const string& s1, const string& s2){
    return s1.size() <= s2.size() ? s1 : s2;
}
```
注：不能返回局部对象的指针或者引用，因为函数返回之后 局部变量就没了……


### 左值和右值
左值可以位于赋值语句的左侧，右值不能
当一个对象被用作右值时，用的是对象的值
当一个对象被用作左值时，用的是对象的身份（在内存中的位置）

调用一个返回引用的函数得到左值


### 函数返回指针
函数不可以返回数组，因为数组不可以拷贝，但是可以返回指针
（由于目前没有遇到过，所以先跳过）


### 底层const和顶层const
const修饰类型：底层const，所指变量不能修改
const修饰变量名，顶层const，指针本身不能修改


### 内联函数的使用场景
用于优化规模较小、流程直接、频繁调用的函数


### 函数指针
（没有用过，跳过）