---
layout: post
title: 对C++模板的整理
date:   2019-05-30 20:43:00 +0800
#大类配置
categories: document
#小类配置
tag: C++
#网易云音乐，只能播放无版权保护的
music-id: 433018842
---

* content
{:toc}
# 函数模板



## 定义函数模板

&emsp;&emsp;普通的函数只能接受指定类型的参数，比如我们定义一个`int compare(int, int);`来比较大小，则我们只能比较两个整型的大小，如果需要比较浮点数的大小，则需要重新写一个新的函数，但新的函数和原函数做的事情其实是一样的。为了提高代码的复用率，故需要用`模板`。

&emsp;&emsp;我们可以定义一个这样的模板：

```c++
template <typename T>
int compare(const T &a,const T &b){
    if(a<b)return -1;
    if(a>b)return 1;
    return 0;
}
```

&emsp;&emsp;第一行以关键字`template`开始，后面跟着模板参数列表`<typename T1, typename T2, ...>`（旧的C++标准用`<class T1, class T2, ...>`，class和typename可以混用），每个`T`表示一种类型。

&emsp;&emsp;函数模板和普通函数一样可以重载，并且我们既可以用另一个模板来重载它，也可以用普通函数来重载它。比如：

```
template <typename T>
int compare(const T *a, const T *b){
    if(*a<*b)return -1;
    if(*a>*b)return 1;
    return 0;
}
```



&emsp;&emsp;顺便说一句，`inline`和`constexpr`跟在模板参数列表后面。



## 实例化函数模板

&emsp;&emsp;函数模板用起来和普通的函数一样：

```c++
compare(1, 0);
```

&emsp;&emsp;编译器会根据实参来实例化一个特定版本的函数`int compare(const int&, const int&)`、



## 模板参数

### 类型模板参数

&emsp;&emsp;类型模板参数就是我们上面的那种模板参数：

```c++
template <typename T1, typename T2>
```

&emsp;&emsp;`T1`、`T2`称为`类型参数`，我们可以把它当作类型说明符，就像内置类型一样使用，特别是，它可以用为返回类型、参数类型、函数内的变量声明和类型转换。

### 非类型模板参数

&emsp;&emsp;非类型参数表示一个值而非类型：

```c++
template <N, M>
```

&emsp;&emsp;非类型参数可以是一个整型，或指向对象或函数类型的指针或引用绑定。绑定到非类型整型参数的实参必须是常量表达式；绑定到指针、引用的非类型参数的实参必须具有静态的生存期。在模板定义内，非类型参数是一个常量值，用在需要常量表达式的地方。比如：

```c++
template<unsigned N, unsigned M>
int compare(const char (&p1)[N], const char(&p2)[M]){
    return strcmp(p1, p2);
}

compare("Hello", "World")
```



## 模板编译

&emsp;&emsp;编译器只有在遇到模板实例时，才会生成代码。这与普通的函数不同。因此，在头文件中，要有下面几个部分：

* 普通函数的声明
* 类定义+类中的函数的声明
* 函数模板的声明+定义
* 类模板的成员函数的定义



# 类模板

&emsp;&emsp;类模板大致上和函数模板差不多，但编译器无法通过实参推断参数类型。我们需要在模板名后的尖括号中提供额外的信息。



## 定义类模板

&emsp;&emsp;和函数模板一样，都是在类名前加上`template <[模板参数列表]>`

```c++
template <typename T>
class number{
    T data;
    number(const T &i):data(i){}
};
```

&emsp;&emsp;我们需要手动为类模板提供`<模板实参>`，如果在类外定义成员函数，必须这样：

```c++
int number<T>::getDate(){
    retrurn data;
}
```

&emsp;&emsp;但在类的作用域里面，我们可以不加`<>`，默认与类实例化的类型一致：

```c++
template <typename T>
class number{
    number *next;
};
```

&emsp;&emsp;特别的，在类外的成员函数定义中，我们也无需加`<>`

```c++
number<T> number<T>::operator++(int){
    number temp=*this;//默认是number<T>
    ++*this;
    return temp;
}
```



## 使用类模板

&emsp;&emsp;使用类模板时，必须提供**显式模板实参**。

```c++
number<int> a(1);//用类模板实例化一个类
bool max(const number<int> &a,const number<int> &b)//将类模板用作参数
```

&emsp;&emsp;就酱。



## 类模板与友元

&emsp;&emsp;类模板可以将其他普通的类或函数声明为友元（这样对所有类型的类模板都是友元，也就是多对一），也可以将另一个类模板或函数模板声明为友元。反过来，一个普通的类可以将特定类型的类模板声明为友元，也可以将所有类型的类模板声明为友元。

### 一对一

&emsp;&emsp;可以将某一类型的类模板或某一类型的函数模板声明为友元

```c++
template <typename> class number;
template <typename T> bool operator==(const number<T>&, const number<T>&);
template <typename T> clss number{
    friend bool operator==<T>(const number<T>&, const number<T>&);
}
```

&emsp;&emsp;有几点要注意：

* 为了在函数中用模板特例，我们需要先声明模板自身。也就是上面一二行。
* 为了让函数只能在特定类型的模板中使用，函数也要是模板函数。



### 特定的模板的友元关系

```c++
template <typename T> class A;
class B{//普通的类
    friend class A<int>;//只有int的类模板是友元
    template<typename T> friend class A;//所有类模板都是友元，无需前置声明
};

template <typename T> class C{//类模板
    friend class A<T>;//只有T的类模板是友元
    template<typename X> friend class A;//所有类模板都是友元，无需前置声明。注意用了不同的模板参数X
}; 
```



## 类模板与静态成员

&emsp;&emsp;只是说一句，静态成员`static`只在相同类型的模板类之间共享，不同类型的类模板的静态成员相互独立。