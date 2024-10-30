### **1 原理**
1. C++ 程序中的内存分为两个部分：
- 栈：在函数内部声明的所有变量都将占用栈内存。
- **堆：这是程序中未使用的内存，在程序运行时可用于动态分配内存。**

很多时候，您无法提前预知需要多少内存来存储某个定义变量中的特定信息，所需内存的大小需要在运行时才能确定。

在 C++ 中，您可以使用new运算符为给定类型的变量在运行时分配 **堆** 内的内存，这会返回所分配的空间地址。

### 2# malloc
new 与 malloc() 函数相比，其主要的优点是，new 不只是分配了内存，它还**创建了对象**。

### 3# 语法

```cpp
Point p1 = Point(0,0); //普通方法
```

```cpp
Point* p1 = new Point(0, 0); //比普通方法多了一个"*"和"new"

delete p1; //只需delete指针"p1"，就能删除其所指向的动态内存，不需要delete"*p1"
```

### 4# 何时适用？

- 当您希望一个对象 **在您使用delete手动删除之前一直存在时** ，应该使用new。如果不使用new，那么对象将在超出作用域时被销毁。例如：

```cpp
void foo()
{
  Point p = Point(0,0);
} // p is now destroyed.

for (...)
{
  Point p = Point(0,0);
} // p is destroyed after each loop
```

- 此外，如果需要创建一个 **长度可变的数组** ，就用new语句

- 值得注意的是，new语句的代价比普通创建对象的语句要大，所以谨慎使用


### 5# 动态数组

1. 一维数组
```cpp
int *array=new int [m];
 
delete [] array;
```

2. 二维数组
```cpp
int **array;

array = new int *[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int [n];
}

for( int i=0; i<m; i++ )
{
    delete [] array[i];
}
delete [] array;
```
