---
layout: post
title: 对C++类的整理(1)——类基础
date:   2019-04-20 21:03:00 +0800
#大类配置
categories: document
#小类配置
tag: C++
#网易云音乐，只能播放无版权保护的
music-id: 465675773
---

* content
{:toc}
> 例子引入：想象我们经营一家书店，需要对每本书的销售数据进行统计，我们将编写一个`Sales_data`，来完成这件事。



# 类的简介

&emsp;&emsp;类的本质上是一种**自定义的数据类型**，它基本组成为(D, S, P)，D为数据对象，S为D上的关系集，P为对D的基本操作。简单来讲，就是：**类=数据+操作**。

&emsp;&emsp;类的声明以关键字`struct`开始，紧跟着类名和类体（类体可为空）：

~~~c++
struct Sales_data{
    //类体
};  //不要漏了分号！
~~~

&emsp;&emsp;每个类内部是一个新的作用域，所以其内部定义的名字可以和外部重复。

---



# 数据成员

&emsp;&emsp;类内数据的定义方法和类外相同，比如我们的销售数据要有每本书的编号bookNo，卖出的数量units_sold，收到的钱revenue。如下

~~~c++
struct Sales_data{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}
~~~

&emsp;&emsp;我们可以为数据成员提供**初始值**（就像上面的units_sold和revenue）,没有初始值的成员将被默认初始化（如bookNo将为空字符串）。

&emsp;&emsp;如果要在类外使用数据成员，只需在类类型后面加“.变量”：

~~~c++
Sales_data data1;
cout<<data1,revenue<<std::endl; //输出0.0
~~~

---



# 成员函数

&emsp;&emsp;成员函数的声明在类内，定义则可以在类内或内外，在内外定义时要指明函数的作用域（因为类本身就是一个作用域）。比如我们给销售数据类加点东西：

~~~c++
struct Sales_data{
    std::string isbn() const {return bookNo;}//返回isbn码 //类内声明+定义
    double avg_price() const;//返回平均售价 //类内声明，注意不要忘了";"
    
    //下面的是数据成员
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}

double Sales_data::avg_price() const{#类外定义，注意不要忘了"::"
    if (units_sold)
    	return revenue/units_sold;
    else
    	return 0;
}
~~~



## this 指针

&emsp;&emsp;调用成员函数时，用`类名.函数名()`的形式。当我们调用成员函数时，实际上是替某个具体对象调用它，为了使成员函数知道使哪个具体对象调用它，C++规定了一个名为`this`的隐式参数，当编译时，具体对象的地址会传入`this`。比如：

```C++
Sales_data total;
total.isbn();#伪代码，相当于：Sales_data::isbn(&total)
```

&emsp;&emsp;如果你的类类型是一个常量类（即具体化类时用了const），由于this指针是一个指向非常量的常量指针，所以不能绑定到常量对象上。此时可以通过在函数后面加const，使this能指向常量。比如上上面的`isbn()`。推荐凡是不改变类数据的函数都加上const

&emsp;&emsp;最后说一句，this是隐式参数意味着我们不能定义this，但我们依然可以在函数内使用this，比如上面`isbn()`可以写成：

```c++
std::string isbn() const {return this->bookNo;}
```

---



# 构造函数

&emsp;&emsp;构造函数是特殊的成员函数，其任务是初始化类对象的数据成员，如果在构造函中没有对数据成员

初始化。它有几个特点：

* 必须声明在public部分（否则无法在类外使用）
* 构造函数的名字与类名相同；
* 构造函数不能声明为const（声明成const了还怎么初始化数据成员的值?）；
* 没有返回值。

&emsp;&emsp;当然，它也具有其他普通函数的特点，比如重载，比如默认实参等。



## 默认构造函数

&emsp;&emsp;如果不对数据成员提供初始值，则通过**默认构造函数**来初始化，它无须任何实参（也就没任何形参或所有形参都有默认实参）。如果我们没有定义构造函数，则编译器会隐式定义一个**合成的默认构造函数**，它会将按照一定规则默认初始化数据成员。

**但是某些类不能用合成的默认构造函数，具体有如下几种类**：

* 只要我们定义了构造函数，无论是否是默认构造函数，编译器都不会生成合成的默认构造函数；
* 数据成员含有数组和指针时，其默认初始化的值是未定义的，因此需要在类内初始化，或定义一个自己的默认构造函数；
* 如果类中包含其他类型的成员且这个成员的类型没有默认构造函数，则编译器无法默认初始化该成员。




### default

&emsp;&emsp;如果我们定义的默认构造函数和合成的默认构造函数干的事差不多，则可以直接在默认构造函数的**定义**（不是声明）的括号后写上`= default;`

&emsp;&emsp;值得注意的是，如果你的编译器不支持类内初始值，你就不能这样写。	



## 构造函数初始值列表/初始化

```c++
Sales_data(const std::string &s): bookNo(s) {}
```

&emsp;&emsp;像上面一样，我们可以在默认函数的括号后面加`数据成员(形参)`来初始化数据成员，在这里其相当于：

~~~c++
Sales_data(const std::string &s) {
    bookNo=s;
}
~~~

&emsp;&emsp;如果要对多个数据成员初始化，它们之间用逗号隔开：

~~~c++
Sales_data(const std::string &s, unsigned n, double p): bookNo(s), units_sold(n), revenue(p*n) {}
~~~

&emsp;&emsp;因为这些构造函数的唯一的作用是赋初值，所以函数体可以为空。在定义类时写成：

~~~c++
Sales_data data1("978-7-121-15535-2", 0, 0.0);
~~~



### 初始化与赋值的区别

* 如果数据成员是const或引用或“某种未提供默认构造函数的类类型”，则只能用初始化；
* 在底层中，实际是先初始化后赋值，所以初始化的效率比较高。

&emsp;&emsp;所以尽量使用初始化



### 初始化的顺序

&emsp;&emsp;成员的初始化顺序与它们在类定义中的出现顺序一致，尽管知道顺序，但还是尽量避免使用一个数据成员初始化另一个数据成员。



## 复制构造函数（拷贝构造函数）

&emsp;&emsp;如果要通过另一个类类型来初始化，则需要通过复制构造函数将数据复制过来。复制构造函数的声明为：

~~~C++
struct Sales_data{
    Sales_data(const Sales_data & ); //形参为必须是引用，且最好是常引用，避免误修改。
};
~~~

&emsp;&emsp;下面是复制构造函数的两种使用方式：

~~~C++
Sales_data data1;
Sales_data data2(data1);
Sales_data data3=data1;
~~~

&emsp;&emsp;除了主动调用复制构造函数，当函数**有类类型参数**或**返回类类型值**时，都需要隐式地调用复制构造参数（即用到临时的类类型时都要），即：

~~~C++
Sales_data function(Sales_data a){ //调用复制构造函数
    return a;//调用复制构造函数
}
~~~

&emsp;&emsp;上面这段也是复制构造函数的参数必须是引用的原因。如果不是引用，则会建立临时量，而临时量本身又需要用到复制构造函数，从而造成循环。



### 浅复制和深复制

&emsp;&emsp;其实如果我们不写复制构造函数，编译器会隐式生成一个复制构造函数，但这个只能复制字面值（即类的数据成员储存的数据），即int就复制int，int* 就复制地址。这就叫浅复制。

&emsp;&emsp;浅复制有个问题，就是如果要复制指针，则只是复制指针所指的地址，而不分配内存空间（因为这个内存空间并不储存在类内）。如果复制得到的对象被析构了，那么原对象的指针就会指向空地址，等到原对象析构时，就会产生“释放空指针”的错误。

&emsp;&emsp;所以我们需要深复制：手动写一个复制构造函数，在复制构造函数里面分配新的内存空间，再复制。即

~~~C++
//我们另外一个类来示范
struct A{
    //数据成员
    int *p;
    
    //成员函数
    A(const A &a){
        p = new int; //分配新的内存空间
        *p=*a.p; //复制具体值，而非地址
    }
};
~~~

&emsp;&emsp;《C++ Primer》239页：使用Vector类或string类可以避免分配和释放内存带来的复杂性。



## 委托构造函数

&emsp;&emsp;就是一个构造函数用其他构造函数来初始化。比如假如我们定义了：

~~~c++
Sales_data(const std::string &s, unsigned n, double p): bookNo(s), units_sold(n), revenue(p*n) {}
~~~

&emsp;&emsp;我们可以利用这个来定义：

~~~c++
Sales_data(const std::string &s): Sales_data(s, 0, 0) {}//委托构造函数
~~~

&emsp;&emsp;这样，当运行后面这个构造函数时，实际上是先执行第一个构造函数，再执行后面这个。



## 隐式类类型转换

&emsp;&emsp;假如我们有某个函数需要接收一个Sales_data对象，而Sales_data有一个这样的构造函数：

~~~c++
void function(const Sales_data &item);

Sales_data::Sales_data(const std::string &s): Sales_data(s, 0, 0) {}
~~~

&emsp;&emsp;我们传递一个string对象：

~~~c++
function(string("9-999-99999-9"));
~~~

&emsp;&emsp;编译器会先掉用构造函数，生成一个临时的Sales_data，由于item是一个常量引用，我们可以把临时变量传递给item。

&emsp;&emsp;另一种用到隐式类类型转换的情况是拷贝：

~~~c++
Sales_data item="9-999-99999-9";
~~~

&emsp;&emsp;这种方法只能适用于只有一个变量的构造函数；并且只能适用于“一步的类类型转换”，比如下面这种就不行：

~~~c++
function("9-999-99999-9");//两步转换，先是将"9-999-99999-9"转化为string，然后再将string转化为Sales_data
~~~



### 抑制隐式类类型转换

&emsp;&emsp;要是我们想禁止这种转换，我们可以在构造函数的**声明**前加`explicit`来阻止（在定义处加explicit会报错）：

~~~c++
explicit Sales_data::Sales_data(const std::string &s): Sales_data(s, 0, 0) {}
~~~

&emsp;&emsp;注意，`explicit`只能用于只有一个参数的构造函数（有多个参数怎么进行类类型转换啊~）。在这种情况下，我们依然可以显式的使用构造函数来转换：

~~~c++
function( string("9-999-99999-9") );//错误
function( Sales_data( string("9-999-99999-9") ) );//正确
~~~

---



# 析构函数





