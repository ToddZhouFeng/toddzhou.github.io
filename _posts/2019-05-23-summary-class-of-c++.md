---
layout: post
title: 对C++类的整理(2)——运算符重载
date:   2019-05-23 22:04:00 +0800
#大类配置
categories: document
#小类配置
tag: C++
#网易云音乐，只能播放无版权保护的
music-id: 465675773
---

* content
{:toc}
# 基本概念

&emsp;&emsp;重载运算符是一种特殊的函数，使得类可以依靠运算符进行特点的操作。它的定义形式如下：

```c++
//声明为成员函数
[类型] [类名]::operator[运算符]( [参数表] ){
    //操作
}

//声明为非成员函数
[类型] operator[运算符]( [类名],[参数表] ){
    //操作
}
```

* 参数的数量与该运算符作用的运算对象数量一样多
* 若运算符函数是成员函数，则它左侧（第一个）运算对象绑定到this指针上，故它的（显式）参数数量要少一个
* 运算符函数要么是类成员，要么含有一个类类型的参数
* 重载不改变运算符的优先级和结合律

&emsp;&emsp;有四个运算符不能被重载：`::`，`.*`，`。`，`?:`。而有两个不应该被重载：`,`，`&`（取地址），`&&`（逻辑与），`||`（逻辑或），因为它们有特殊含义。



## 成员or非成员

&emsp;&emsp;对于声明为成员函数还是友元函数，应遵循以下原则：

* 赋值`=`、下标`[]`、调用`()`、成员访问箭头`->`运算符必须是成员函数
* 具有对称性的运算符应该是非成员（比如加减乘除、相等性、关系、位运算符）
* 改变对象状态的运算符应该是成员（比如递增、递减、解引用）



# 具体的运算符

## 输入输出运算符(<<、>>)

&emsp;&emsp;考虑`<<`的使用方法：`cout<<类类型`，所以第一个对象是`ostream`，第二个对象是我们的类，故我们声明为非成员函数，并且声明为类的友元：

```c++
class Sales_data{
private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
public:
    friend ostream& operator<<(ostream &, const Sales_data);
};
```

&emsp;&emsp;由于`ostream`无法被复制，所以它的形参和返回值都是引用；而由于我们一般不改变类的数据，所以类用`const`修饰。



&emsp;&emsp;考虑`>>`的用法：`cin>>类类型`，所以第一个对象为`istream`，第二个对象是我们的类，故和`<<`的重载方法差不多：

```c++
class Sales_data{
private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
public:
    friend istream& operator>>(istream &, Sales_data &);
};
```

&emsp;&emsp;同样，`istream`不能被复制，故形参和返回值也都是引用；而我们需要改变原类类型的值，故类的形参是引用。特别的，我们在定义输入重载函数时，需要考虑输入失败的情况，并要从失败中恢复，并将流状态设置为`failbit`（见《C++ Pimer》496页）



## 算术和关系运算符（+、-、==、!=、<、>等）

&emsp;&emsp;算术运算符有三个特点：

1. 左右对象可交换（故一般定义为**友元函数**）
2. 不改变操作对象的值（故形参为**常量引用**）
3. 返回一个临时的类类型（故返回局部变量的副本）

&emsp;&emsp;以加法为例：

```c++
class Sales_data{
private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
public:
    friend Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs);
};

Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs){
    Sales_data sum=lhs;
    sum += rhs;//采用后面的+=的重载函数
    return sum;
}
```

&emsp;&emsp;《C++ Primer》 497页：类如果同时定义了算术运算符和复合赋值运算符，通常情况下应用复合赋值来实现算术运算符。



&emsp;&emsp;关系运算符也大同小异，唯一不同是返回值是bool值。并且要遵循如下设计准则：

* 相等运算符`==`应具有传递性
* 定义了`==`，则相应的要定义`!=`；同理，定义了`<`，相应的要定义`>`



## 递增和递减运算符（++、--）

&emsp;&emsp;虽然没必要，但还是建议将递增和递减运算符定义为成员函数。因为它们只改变所操作的对象。

### 前置版本（++a）

&emsp;&emsp;应该返回**递增或递减后的对象的引用**，这样才能作为左值使用。

```c++
class A{
private:
    int data;
public:
    A& operator++();
}

A& A::operator++(){
    ++data;
    return *this;//返回递增后的对象的引用
}
```



### 后置版本（a++）

&emsp;&emsp;应该返回递增或递减**前**的对象的**原值**，而非引用。

```c++
class A{
private:
    int data;
public:
    A& operator++(int);
}

A& A::operator++(int){
    A temp=*this;
    ++*this;//用之前定义的前置来实现
    return temp;//返回递增后的对象的引用
}
```

&emsp;&emsp;需要注意的是，我们的参数中有一个`int`，但我们并不会用到它，所以无需为其命名，编译器会为它提供一个值为0的实参。也就等价于：

```c++
A& A::operator++(int i=0){};
```



## 赋值运算符（=）

&emsp;&emsp;之前本来是要讲拷贝赋值的，不过基础部分还是不要太深入了，其实和复制构造函数是一样的，也就是编译器会为我们自动重载赋值运算符，但那只是浅复制。要实现深复制，则需要手动对赋值运算符进行重载。要求如下：

* 必须是成员函数
* 返回左侧运算对象的引用（`return *this`）

&emsp;&emsp;例子和深复制那部分差不多，就不给了。



## 下标运算符（ [] ）

&emsp;&emsp;貌似最近写程序都没用过这个，就大概讲一下要求：

* 必须是成员函数
* 返回的是对应元素的引用（这样可以作为左值或右值）
* 最好定义多一个常量版本（函数后面加const），并且返回常量引用



## 成员访问运算符（*）

（待定）

## 函数调用运算符（ ( ) ）

（待定）



# 类类型转换

&emsp;&emsp;假如我们需要将类和一个整数相加，则编译器会利用构造函数，将整数转化为类，再相加。那么有没有办法能让类转化为整数呢？这就是类类型转换。

&emsp;&emsp;类型转换运算符类似于一般的运算符重载，它的声明格式如下：

```c++
operator type() const;//type为要转换成的类型，可以是内置类型，也可以是其它类
```

&emsp;&emsp;它有几个特点：

* 类类型转换函数必须是成员函数；
* 不能声明返回类型（返回类型只能为type）；
* 不能有形参；
* 通常应该是const

&emsp;&emsp;类类型转换一般是隐式执行的，因此无法给这些函数传递实参（强制执行的话可以用`int([类类型])`，或者`[类型].operator type()`）。



## 注意

&emsp;&emsp;首先，不是所有类都需要类类型转换，比如某个类表示Date，某一天Aug 13, 2000既可以解释为20000813，也可以解释为从Jan 1, 2000经过的天数。在这种情况下，应该定义两个成员函数负责返回不同的格式。

&emsp;&emsp;其次，要避免二义性。比如：类A定义了接受类B的转换构造函数，而类B定义了转换到类A的函数。这种情况下，我们只能显式的使用对应的函数，否则编译器无法确定要用哪个。并且我们不能使用强制类型转换，因为这种情况下，强制类型转换也面临二义性。

&emsp;&emsp;关于如何避免这些情况，见



## 显式的类类型转换

&emsp;&emsp;为了防止某些情况下，隐式的类类型转换造成错误，可以在类类型转换函数声明前加`explicit`：

```c++
class A{
private:
    int val;
public:
    explicit operator int() const {return val;}
}
```

&emsp;&emsp;此时，如果想要将A转化为int，需要显式的强制转换：

```c++
A a=3;
a+3;//错误
int(a)+3;//正确
```

&emsp;&emsp;但是，在下列位置，显式的类类型转换会被隐式执行：

- if、while及do语句的条件部分
- for语句头的条件表达式
- 逻辑运算符的运算对象
- 条件运算符（?:）的条件表达式

&emsp;&emsp;实际上，cin有一个向bool转换的显式的类型转换函数，所以我们可以在条件表达式中用cin，并且在其他位置用cin不会出错。



# 题外话

顺便说一句：NASA有一个*把名字刻在火星漫游者芯片上* 的活动，也就是说，我的名字要上火星咯~

<iframe width="980" height="410" src="https://mars.nasa.gov/layout/embed/send-your-name/mars2020/certificate/?cn=798945412605" frameborder="0"></iframe>

