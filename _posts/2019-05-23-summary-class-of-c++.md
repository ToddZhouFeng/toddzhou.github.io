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

&emsp;&emsp;重载运算符是一种特殊的函数，用于对类类型进行特定的操作。它的定义形式如下：

~~~c++
//声明为成员函数
[类型] [类名]::operator[运算符]( [参数表] ){
    //操作
}

//声明为非成员函数
[类型] operator[运算符]( [类名],[参数表] ){
    //操作
}
~~~

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

~~~c++
Sales_data{
private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
public:
    friend ostream& operator<<(ostream &, const Sales_data);
};
~~~

&emsp;&emsp;由于`ostream`无法被复制，所以它的形参和返回值都是引用；而由于我们一般不改变类的数据，所以类用`const`修饰。



&emsp;&emsp;考虑`>>`的用法：`cin>>类类型`，所以第一个对象为`istream`，第二个对象是我们的类，故和`<<`的重载方法差不多：

~~~c++
Sales_data{
private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
public:
    friend istream& operator>>(istream &, Sales_data &);
};
~~~

&emsp;&emsp;同样，`istream`不能被复制，故形参和返回值也都是引用；而我们需要改变原类类型的值，故类的形参是引用。特别的，我们在定义输入重载函数时，需要考虑输入失败的情况，并要从失败中恢复，并将流状态设置为`failbit`（见《C++ Pimer》496页）



## 算术和关系运算符
