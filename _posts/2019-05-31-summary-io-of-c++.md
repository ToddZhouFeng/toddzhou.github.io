---
layout: post
title: 对C++I/O库的整理
date:   2019-05-31 15:19:00 +0800
#大类配置
categories: document
#小类配置
tag: C++
#网易云音乐，只能播放无版权保护的
music-id: 36198480
---

* content
{:toc}
# 基本的I/O类库与对象

* `iostream`
  * `istream`、`wistream`从流读取数据
  * `ostream`、`wostream`向流中写入数据
  * `iostream`、`wiostream`读写流
* `fstream`
  * `ifstream`、`wifstream`从文件读取数据
  * `ofstream`、`wofstream`向文件写入数据
  * `fstream`、`wfstream`读写文件
* `sstream`
  * `istringstream`、`wistringstream`从string读取数据
  * `ostringstream`、`wostringstream`向string写入数据
  * `stringstream`、`wstringstream`读写string

* `iomanip`：用于指定输入输出流的格式

&emsp;&emsp;为了支持宽字符的语言（即wchar_t类型），io库定义了以w开头的一组类型和对象，比如`wcin`、`wcout`分别对应`cin`、`cout`。这些用起来和普通字符没什么不同，后面我们就以普通字符为例子。



## IO类型间的关系

&emsp;&emsp;IO类型之间存在继承的关系。如下

![io类型的关系](https://cn.bing.com/th?id=OIP.vuMzG8pGvefTKe2X1DlFmgHaDR&pid=Api&rs=1&p=0 "io类型的关系")

&emsp;&emsp;当然，实际上的继承关系远比这复杂。要了解更多，可以看回课本。

&emsp;&emsp;正是因为有了上面的继承关系，一些用于`istream`、`ostream`的操作，比如`<<`、`>>`也可以用于`ifstream`、`istringstream`、`ofstream`、`ostringstream`



## IO对象无拷贝或赋值

&emsp;&emsp;**IO对象无拷贝或赋值！**所以我们不能用`=`对流赋值或拷贝，也不能在函数中使用流参数或返回流，只能使用或返回流的引用或指针。这点格外要注意。



## 流状态

&emsp;&emsp;由于IO可能发生错误，我们需要一些标志和函数标记或检测流状态。

### 标志

&emsp;&emsp;ios类中，有一个数据成员，其每一位都对应一种错误状态，称为状态字。具体如下

| 标识常量 | 值   | 含义 |
| -------- | ---- | ---- |
| goodbit  | 0x00 | 状态正常 |
|eofbit| 0x01 |文件结束|
|failbit|0x02|IO操作失败，但数据未丢失，可恢复|
|badbit|0x04|流崩溃，数据丢失，不可恢复|

&emsp;&emsp;使用时，记得格式是`ios::goodbit`，当然，直接用值也行，就是不那么好记。

### 检查与设置

&emsp;&emsp;以下函数用于检查流状态：

```c++
istream &s=cin;
//检查是否为eofbit，是则返回1，否则返回0
s.eof();

//检查是否为failbit或badbit，是则返回1，否则返回0
s.fail();
!s;

//检查是否为badbit，是则返回1，否则返回0
s.bad();

//检查是否为goodbit，是则返回1，否则返回0
s.good();
*s;

//返回状态字
s.rdstate();
```

&emsp;&emsp;以下函数用于设置流状态（都是返回void）：

```c++
//复位所有状态位，并将流状态设为有效
s.clear();

//复位所有状态位，并将流状态设为标识常量flags
s.clear(flags)

//单纯地将flags的对应位设为1，并不会清除其他状态
s.setstate(flags)
```

&emsp;&emsp;关于最后两个函数的区别可以看：[clear与setstate的区别](https://blog.csdn.net/origin_lee/article/details/38707643)



## 缓冲

&emsp;&emsp;输出流都有一个缓冲区，用来保存程序读写的数据，并直到缓冲区刷新时才写到输出设备或文件。缓冲刷新的时机为：

* 程序正常结束，main函数的return操作会执行缓冲刷新。

* 缓冲区满时，只有刷新后数据才能继续写入缓冲区

* 用操纵符`endl`、`ends`、`flush`来显式刷新，用法为`cin<<endl`。它们三个的区别是：

  `endl`：添加一个“换行”，再刷新；

  `ends`：添加一个“空格”，再刷新；

  `flush`：不添加额外字符，直接刷新；

* 用`unitbuf`来设置不缓存，立即刷新，用法为`cin<<unitbuf`；若要取消，可用`nounitbuf`。

* 一个输出流被关联到另一个流，当读写后面那个流时，刷新原输出流。比如`cout`与`cin`关联，当写cin时，刷新cout



# 格式化输入输出

&emsp;&emsp;标准库定义了一组**操纵符**来控制流的格式状态，也就是修改数值的输出形式或控制补白的数量和位置。一般来讲，操纵符都是“设置”/“复原“成对的。下面的若无说明，无需包含iomanip头文件，凡是以set开头的都在iomanip中。

## bool格式

&emsp;&emsp;默认情况下，bool值输出0/1；输出true/false，可用`boolalpha`；复原可用`noboolalpha`。一旦设置了bool格式，会对后面所有的bool值起作用。

```c++
cout<<true<<' '<<false<<'\n' \\输出0 1
    <<boolalpha<<true<<' '<<false<<'\n' \\输出true false
    <<noboolalpha<<true<<' '<<false<<endl; \\输出1 0
```



## 整型格式

&emsp;&emsp;默认情况下，整型输出使用十进制。用`hex`改为十六进制；`oct`改为八进制；`dec`改回十进制。一旦设置了格式，会对后面所有的整型起作用。

```c++
cout << 24 << ' '//24
	<< hex << 24 << ' '//18
	<< oct << 24 << ' '//30
	<< dec << 24 << endl;//24
```

&emsp;&emsp;默认是只输出数字。如果想要十六进制输出0x18，八进制输出030，可以用`showbase`，若要取消，可以用`noshowbase`。一旦设置，对后面所有的整型起作用。

```c++
cout <<showbase
	<< 24 << ' '//24
	<< hex << 24 << ' '//0x18
	<< oct << 24 << ' '//030
	<< dec << 24 << endl;//24
```

&emsp;&emsp;默认情况下，十六进制的0x18是用小写的x，并且用小写的“abcdef”，可以用`uppercase`来设置为大写，`nouppercase`设置为小写。一旦设置，对后面所有的整型起效。

```c++
cout <<showbase
	<< hex << 15 << ' '//0xf
	<<uppercase<<15<<endl;//0XF
```

&emsp;&emsp;默认情况下，正数前面无正号，若要输出正号，可用`showpos`，取消可以用`noshowpos`。一旦设置，对后面所有的正整数和正浮点数都有效。

## 浮点数格式

&emsp;&emsp;默认精度为6位，超出的位四舍五入。若要设置精度，可以用下面三个函数：

```c++
cout.precision();//返回旧精度
cout.precision(5);//设置新精度，返回旧精度
setprecision(5);//设置新精度，不返回值。要包含头文件iomanip
```

```c++
float pi = 3.1415926;
cout << setprecision(5) << ' ' << pi << '\n'//3.1416
	<< setprecision(4) << ' ' << pi << '\n'//3.142
	<< setprecision(3) << ' ' << pi << endl;//3.14
```

```c++
float pi = 3.1415926;
cout << cout.precision(5) << ' ' << pi << '\n'//4 3.1416
	<< cout.precision(4) << ' ' << pi << '\n'//3 3.1416
	<< cout.precision(3) << ' ' << pi << endl;//6 3.1416
```

&emsp;&emsp;注意，在最后一个例子中，cout.precision(int)是从后往前执行，并且最终的输出结果取决于前面的。



&emsp;&emsp;浮点数有三种计数法：科学计数法、定点十进制或十六进制计数法。操纵符`scientific`设置科学计数法；`fixed`设置定点十进制；`hexfloat`设置十六进制法。标准库默认会根据数值自动选择计数法，我们也可以通过`defaultfloat`来设置成默认模式。一旦设置，对后面所有的浮点数都有效。

&emsp;&emsp;一旦设置为`scientific`、`fixed`或`hexfloat`后，精度的含义会发生变化：默认模式指的是总位数，设置后指的是小数点后的位数。

```c++
cout << pi << ' ' //3.14159
	<< scientific << pi << ' ' //3.141593e+00
	<< fixed << pi << ' ' //3.141593
	<< hexfloat << pi << ' ' //0x1.921fb4p+1
	<< defaultfloat << pi << endl; //3.14159
```

&emsp;&emsp;科学计数法的e和十六进制默认为小写，要用大写的话可以用`uppercase`，要用小写的话可以用`nouppercase`。



&emsp;&emsp;默认情况下，若浮点数的小数部分为零，则不显示小数点。可以用`showpoint`和`noshowpoint`在显示与不显示之间转换。若显示，则小数点后面的零取决于精度。

```c++
cout<<showpoint<<3.0 //3.00000
	<<noshowpoint<<3.0 //3
```



## 输出补白

`setw(int)`：**包含在iomanip中**。指定**下一个**数字或字符串的最小空间（宽度）。如果没填满，则在前面加空格；如果填满或大于，则按正常输出。只对下一个数字或字符串有效。

```c++
cout << setw(10) << "yoyoyo" << endl;
//1234yoyoyo//1234是为了看清有几个空格人为标上去的
```

`left`：左对齐输出。比如在上面的最小空间中，没填满时，数字或字符串默认是在右边；设置左对齐后，是在左边。一旦设置，对后面所有的数字或字符串都有效。右对齐为`right`

```c++
cout<<setw(10)<<left<<"yoyoyo"<<endl;
//yoyoyo7890//7890是为了看清有几个空格人为标上去的
```

`setfill('a')`：**包含在iomanip中**。设置用于补白的字符，默认为空格，只允许用一个字符去替换。一旦设置，对后面所有的都有效。

```c++
cout<<setfill('a')<<setw(10)<<"yoyoyo"<<endl;
//aaaayoyoyo
```

`internal`：控制负数符号的位置，设置之后左对齐符号，右对齐数字，中间补白（前提是setw的宽度要大于负数长度）。一旦设置，对后面所有的负数都有效。并没有找到取消的方法......

```c++
cout<<internal<<setw(10)<<setfill('a')<<-10<<endl;\
//-aaaaaaaa10
```



## 控制输入格式

&emsp;&emsp;默认输入为忽略空格、制表、换行、换纸、回车符。要让输入不忽略，可用`noskipws`；忽略可用`skipws`

```c++
char ch;
while(cin>>ch)cout<<ch;
//输入a b c d//输出abcd
```

```c++
char ch;
cin>>noskipws;
while(cin>>ch)cout<<ch;
//输入a b c d//输出a b c d
```





# 标准流(iostream)

&emsp;&emsp;标准流用于用户与硬件之间的输入输出。它有如下几个特殊的对象：

* `cin`：istream对象，连向键盘，从键盘读取数据
* `cout`：ostream对象，连向显示器，将标准流输出到屏幕上
* `cerr`：ostream对象，标准错误输出流，连向显式器，将错误信息“不经过缓冲区地”、“实时地”输出到屏幕上。不能重定向到文件
* `clog`：ostream对象，标准错误输出流，连向打印机，将错误信息输出到缓冲区，等到缓冲区刷新再输出。不能重定向到文件

&emsp;&emsp;除了上面几个对象外，我们不能定义自己的`istream`或`ostream`或`iostream`，因为它们并没有构造函数。



## 输入流操作

* 读取字符

  ```c++
  cin.get(ch);//读一个字节并放入ch中，返回cin
  cin.getline();
  ```

* 流指针

  ```c++
  cin.seekg();
  cin.tellg();
  ```




## 输出流操作

* 插入字

  ```c++
  cout.put(ch);//将ch放入cout，返回cout
  cout.write();
  ```

* 刷新流

  ```c++
  cout.flush();
  ```

* 流指针

  ```c++
  cout.seekp();
  cout.tellp();
  ```


# 更多拓展知识

* [C++基本的输入输出](https://www.runoob.com/cplusplus/cpp-basic-input-output.html)