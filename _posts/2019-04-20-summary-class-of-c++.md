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

&emsp;&emsp;类的声明以关键字`struct`开始（也可以用`class`，区别后面会讲。），紧跟着类名和类体（类体可为空）：

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

&emsp;&emsp;成员函数的声明在类内，定义则可以在类内或内外（如果在类内，则自动是内联的），在内外定义时要指明函数的作用域（因为类本身就是一个作用域）。比如我们给销售数据类加点东西：

~~~c++
struct Sales_data{
    std::string isbn() const {return bookNo;}//返回isbn码 //类内声明+定义
    double avg_price() const;//返回平均售价 //类内声明，注意不要忘了";"
    
    //下面的是数据成员
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}

double Sales_data::avg_price() const{//类外定义，注意不要忘了"::"
    if (units_sold)
    	return revenue/units_sold;
    else
    	return 0;
}
~~~



## this 指针

&emsp;&emsp;调用成员函数时，用`类名.函数名()`的形式。当我们调用成员函数时，实际上是替某个具体对象调用它，为了使成员函数知道使哪个具体对象调用它，C++规定了一个名为`this`的隐式参数，当编译时，具体对象的地址会传入`this`。比如：

~~~C++
Sales_data total;
total.isbn();#伪代码，相当于：Sales_data::isbn(&total)
~~~

&emsp;&emsp;如果你的类类型是一个常量类（即具体化类时用了const），由于this指针是一个指向非常量的常量指针，所以不能绑定到常量对象上。此时可以通过在函数后面加const，使this能指向常量。比如上上面的`isbn()`。推荐凡是不改变类数据的函数都加上const

&emsp;&emsp;最后说一句，this是隐式参数意味着我们不能定义this，但我们依然可以在函数内使用或返回this指针，比如上面`isbn()`可以写成：

~~~c++
std::string isbn() const {return this->bookNo;}
~~~

---



# 构造函数

&emsp;&emsp;构造函数是特殊的成员函数，其任务是初始化类对象的数据成员，如果在构造函数中没有对数据成员

初始化，则编译器会对数据成员赋默认值。构造函数有几个特点：

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

&emsp;&emsp;如果我们定义的默认构造函数和合成的默认构造函数干的事差不多，则可以直接在构造函数的声明（或定义）的参数括号后写上`= default;`

&emsp;&emsp;值得注意的是，如果你的编译器不支持类内初始值，你就不能这样写。	



## 构造函数初始值列表/初始化

~~~c++
Sales_data(const std::string &s): bookNo(s) {}
~~~

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

&emsp;&emsp;所以尽量使用初始化。



### 类的包含

&emsp;&emsp;（待定）



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
Sales_data data4={"9-999-99999-9", 0, 0。0};//注意：这种写法只适用于聚合类（仅有数据成员而无成员函数的类）
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



### delete（禁止复制）

&emsp;&emsp;如果我们不希望编译器为我们隐式生成一个复制构造函数（某些对象复制是没意义的，比如iostream，见《C++ Primer》449页），我们可以在第一次声明时，在参数的括号后面加`=delete`：

~~~c++
struct NoCopy{
    NoCopy() = default;
    NoCopy(const NoCopy&) = delete;
    NoCopy& operator=(const NoCopy&) = delete;
};
~~~

&emsp;&emsp;`delete`不仅适用于隐式的复制构造函数，也适用于其他隐式生成的函数（比如上面的拷贝赋值运算符的重载），析构函数除外。

&emsp;&emsp;另外，还有一种方法禁止复制，那就是将复制构造函数声明在`private`中，并且不定义它。这样在编译的过程中就会出错。尽量不要用这种方法，而是用`delete`



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

&emsp;&emsp;要是我们想禁止这种转换（比如我们想对“=”进行重载），我们可以在构造函数的**声明**前加`explicit`来阻止（在定义处加explicit会报错）：

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

&emsp;&emsp;析构函数是特殊的成员函数，与构造函数相反，其任务是销毁类对象的数据成员。析构函数的基本特点是：

* 名字由波浪号+类名构成，比如`~Sales_data();`
* 不接受参数，不能重载，也不返回值。因此每个类只有一个析构函数；
* 首先执行函数体，之后按初始化顺序的逆序销毁成员；

&emsp;&emsp;析构函数被调用的时机：

* 变量离开作用域时；
* 对象被销毁时；
* 动态分配的对象（`new`），对指向它的指针用`delete`运算符时；
* 临时对象，当创建它的表达式结束时；

&emsp;&emsp;特别的，当指向对象的引用或指针离开作用域时，并不会调用析构函数。



## 合成的析构函数

&emsp;&emsp;当一个类未定义析构函数时，编译器会为它定义一个合成的析构函数，合成的析构函数等价于：

~~~c++
class Sales_data{
public:
    ~sales_data(){}
};
~~~

&emsp;&emsp;注意到它的函数体为空。在此再次强调：无论函数体是否为空，析构函数**首先执行函数体，之后按初始化顺序的逆序销毁成员**。也就是说析构函数内的内容对后面的析构无影响。



## 析构指针对象

&emsp;&emsp;销毁一个指针时，指针所指向的对象并不会被删除。因此如果要析构指针，要手动`delete`它所指的对象。我们这里的指针是内置的指针类型，C++还有一种“智能指针”，它可以自动销毁，无需手动`delete`。

---



# 访问控制和封装

&emsp;&emsp;为了限制用户对类的访问权限，可以利用**访问说明符**：`public`、`protected` 和`private`（`protected`后面再讲）

```c++
class Sales_data{
public://以下整个程序内可被访问//公有成员
    Sales_data() = default;
    Sales_data(const std::string &s, unsigned n, double p): bookNo(s), units_sold(n), revenue(p*n) {}
private://以下可被类的成员函数访问
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```

&emsp;&emsp;特别的，如果在访问说明符前声明数据成员或成员函数，对于`struct`则默认为`public`，对于`class`则默认为`private`。这是`struct`和`class`唯二的区别（另一个在继承那里）。



## 友元

&emsp;&emsp;如果希望类外的某个函数，或其他类能访问非公有成员，可以将其声明为友元，声明的位置不限（可以在public或private等）：

```c++
class Sales_data{
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::istream& read(std::istream&, Sales_data&);
};

Sales_data add(const Sales_data&, const Sales_data&);
friend std::istream& read(std::istream&, Sales_data&);
```

&emsp;&emsp;最好在类定义开始或结束前的位置集中声明友元。注意，友元的声明仅指定了访问权限，并非函数声明，我们还是需要在类外声明函数。另外，友元必须和类在同一个作用域内。

&emsp;&emsp;如果想要将某个类的成员函数声明为友元，必须仔细组织代码结构：

```c++
class A{
    void function();//声明，但不定义
};

class B{
    friend void A::function();
}

void A::function(){}//在A声明了友元后再定义，这样才能访问到A
```



# 其他成员

## 可变数据成员

&emsp;&emsp;如果我们想要修改类的某个数据成员，即使是在一个const成员函数内。可以在变量的声明前加入`mutable`:

```c++
class A{
public:
    void function() const
private:
    mutable int a;
};、

void A::function(){
    ++a;
}
```



## 静态成员

### 声明和定义

&emsp;&emsp;对于一般的数据成员，每个类类型都有自己的版本；而有时候我们希望所有的类类型都共有一个数据成员（也就只有一个版本）。可以在变量的声明前加入`static`：

```c++
class A{
public:
    static int a=10;//在类内声明并提供初始值
    static int b;
}
int A::a;//在类外定义，不再指定初始值
int A::b=20;//在类外定义//不要忘了int
```

&emsp;&emsp;静态成员不能通过特定的类类型去初始化，必须在类内声明，在类外定义（在类外定义时不能加`static`）。在类内声明时，可以提供一个初始值，这样在类外定义时就不能再指定初始值。静态成员实际上类似于全局变量（因为在程序中有且只有一个），所以我们**必须在类外定义**，这样编译器才能为它分配空间。

### 使用

&emsp;&emsp;有两种方式使用静态成员：通过类，或通过某一类类型

```c++
A sample;
A::a=20;//通过“类::静态成员”访问
sample.a=20;//通过某一的类类型访问
```

### 特殊

&emsp;&emsp;静态成员可以是不完全类型（仅声明而未定义的类），比如：

```c++
class A{
public:
    static A c;//如果c不是静态成员，这种写法是错误的
}
A A::c=//略
```

&emsp;&emsp;而且其他成员函数可以将静态成员作为默认实参：

```c++
class A{
public:
    static int a=10;
    void function(int = a);
}
int A::a;
void A::function(int num=a){}//略
```

