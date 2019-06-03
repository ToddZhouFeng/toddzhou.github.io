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

&emsp;&emsp;注意，在最后一个例子中，cout.precision(int)是从后往前执行，并且最终的输出结果取决于前面的。并且float类型的最大精度为6，double最大精度为15。



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

`setw(int)`：**包含在iomanip中**。指定**下一个**数字或字符串的最小空间（宽度）。如果没填满，则在前面加空格；如果填满或大于，则按正常输出。**只对下一个数字或字符串有效**。

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

`internal`：控制负数符号的位置，设置之后左对齐符号或基数指示符，右对齐数字，中间补白（前提是setw的宽度要大于负数长度）。一旦设置，对后面所有的负数或非十进制数都有效。并没有找到取消的方法......

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



# 其他格式化输入输出

&emsp;&emsp;上面大部分使用`<<`和`>>`+控制符来实现，下面介绍两种其他设置格式的方法：

## ios类中的方法

&emsp;&emsp;ios类中可以通过格式控制函数来设置格式。格式控制函数如下：

```c++
//返回标志字
ios.flags();

//设置标志字
ios.flags(Flags);//清空并设置Flags标志位，返回之前的标志位
ios.setf(Flags);//在原基础上加上Flags标志位，返回之前的标志位
ios.setf(Flags, Masks);//在基础上加上Flags标志位，清除Masks标志位，返回之前的标志位

//清除标志位
ios.unsetf(Flags)//清除Flags标志位，返回之前的标志位

//设置宽度
cout.width(int n);
//返回宽度
cout.width();

//设置填充字符
cout.fill(chat ch);
//返回填充字符
cout.fill();

//设置精度
cout.precision(int n);
//返回精度
cout.precision();
```

&emsp;&emsp;其中，标志字和上面的控制符差不多，不过用的时候要加上`ios::`，下面列出常用的标志字：

<table style="height: 686px; width: 825px;" border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="75">
<p>位组</p>
</td>
<td valign="top" width="76">
<p>格式标志</p>
</td>
<td valign="top" width="217">
<p>作用</p>
</td>
<td valign="top" width="113">
<p>默认值</p>
</td>
<td valign="top" width="71">
<p>所占bit</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>skipws</p>
</td>
<td valign="top" width="217">
<p>使用输入操作符时跳过空白字符</p>
</td>
<td valign="top" width="113">
<p>设置</p>
</td>
<td valign="top" width="71">
<p>1</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>unitbuf</p>
</td>
<td valign="top" width="217">
<p>每次操作后刷新缓冲区</p>
</td>
<td valign="top" width="113">
<p>Cerr设置,其他对象不设置</p>
</td>
<td valign="top" width="71">
<p>2</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>uppercase</p>
</td>
<td valign="top" width="217">
<p>字母采用大写</p>
</td>
<td valign="top" width="113">
<p>不设置</p>
</td>
<td valign="top" width="71">
<p>3</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>showbase</p>
</td>
<td valign="top" width="217">
<p>输出整数时加上进制前缀</p>
</td>
<td valign="top" width="113">
<p>未设置</p>
</td>
<td valign="top" width="71">
<p>4</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>showpoint</p>
</td>
<td valign="top" width="217">
<p>按精度输出浮点数（不够补0）</p>
</td>
<td valign="top" width="113">
<p>未设置</p>
</td>
<td valign="top" width="71">
<p>5</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>showpos</p>
</td>
<td valign="top" width="217">
<p>输出非负数时加‘+’</p>
</td>
<td valign="top" width="113">
<p>未设置</p>
</td>
<td valign="top" width="71">
<p>6</p>
</td>
</tr>
<tr>
<td rowspan="3" valign="top" width="75">
<p>&nbsp;</p>
<p>adjustfield</p>
</td>
<td valign="top" width="76">
<p>left</p>
</td>
<td valign="top" width="217">
<p>加入指定字符使输出左对齐</p>
</td>
<td rowspan="3" valign="top" width="113">
<p>&nbsp;</p>
<p>right</p>
</td>
<td valign="top" width="71">
<p>7</p>
</td>
</tr>
<tr>
<td valign="top" width="76">
<p>right</p>
</td>
<td valign="top" width="217">
<p>加入指定字符使输出右对齐</p>
</td>
<td valign="top" width="71">
<p>8</p>
</td>
</tr>
<tr>
<td valign="top" width="76">
<p>Internal</p>
</td>
<td valign="top" width="217">
<p>在符号和数值中间插入指定字符</p>
</td>
<td valign="top" width="71">
<p>9</p>
</td>
</tr>
<tr>
<td rowspan="3" valign="top" width="75">
<p>&nbsp;</p>
<p>basefield</p>
</td>
<td valign="top" width="76">
<p>dec</p>
</td>
<td valign="top" width="217">
<p>10进制输入/输出</p>
</td>
<td rowspan="3" valign="top" width="113">
<p>&nbsp;</p>
<p>dec</p>
</td>
<td valign="top" width="71">
<p>10</p>
</td>
</tr>
<tr>
<td valign="top" width="76">
<p>oct</p>
</td>
<td valign="top" width="217">
<p>8进制输入/输出</p>
</td>
<td valign="top" width="71">
<p>11</p>
</td>
</tr>
<tr>
<td valign="top" width="76">
<p>hex</p>
</td>
<td valign="top" width="217">
<p>16进制输入/输出</p>
</td>
<td valign="top" width="71">
<p>12</p>
</td>
</tr>
<tr>
<td rowspan="2" valign="top" width="75">
<p>floatfield</p>
</td>
<td valign="top" width="76">
<p>scientific</p>
</td>
<td valign="top" width="217">
<p>浮点数按科学计数法输出</p>
</td>
<td rowspan="2" valign="top" width="113">
<p>无,由浮点数量级决定</p>
</td>
<td valign="top" width="71">
<p>13</p>
</td>
</tr>
<tr>
<td valign="top" width="76">
<p>fixed</p>
</td>
<td valign="top" width="217">
<p>浮点数按小数输出</p>
</td>
<td valign="top" width="71">
<p>14</p>
</td>
</tr>
<tr>
<td valign="top" width="75">
<p>&nbsp;</p>
</td>
<td valign="top" width="76">
<p>boolalpha</p>
</td>
<td valign="top" width="217">
<p>以字母格式输入和输出布尔值</p>
</td>
<td valign="top" width="113">
<p>未设置</p>
</td>
<td valign="top" width="71">
<p>15</p>
</td>
</tr>
</tbody>
</table>

&emsp;&emsp;下面几点额外需要注意：

* 用的时候前面要加上`ios::`
* 在使用`setf`设置属于某一位组的标记位时,需要提供位组作为第二个参数来将其他互斥的标记位复位,否则可能会出现设置无效的现象。并且建议用公有静态符号`basefield`、`adjustfield`和`floatfield`
* cout和cin初始时只有`ios::skipws`和`ios::dec`，即0010 0000 0001



## iomanip库中的方法

&emsp;&emsp;iomanip中有一些控制符可以用于设置标志字：

```c++
//设定标志字
setiosflags(ios::Flags);

//清除标志字
resetiosflags(ios::Flags);

//设置基数
setbase(int base)//base可以取8 10 16

//设置填充符
setfill(char ch);

//设置浮点数精度
setprecision(int n);

//设置输出宽度
setw(int n);
```

&emsp;&emsp;这些并不是函数，其用法和操纵符一样：

```c++
cout<<setiosflags(ios::left);
```





# 标准流(iostream)

&emsp;&emsp;标准流用于用户与硬件之间的输入输出。它有如下几个特殊的对象：

* `cin`：istream对象，连向键盘，从键盘读取数据
* `cout`：ostream对象，连向显示器，将标准流输出到屏幕上
* `cerr`：ostream对象，标准错误输出流，连向显式器，将错误信息“不经过缓冲区地”、“实时地”输出到屏幕上。不能重定向到文件
* `clog`：ostream对象，标准错误输出流，连向打印机，将错误信息输出到缓冲区，等到缓冲区刷新再输出。不能重定向到文件

&emsp;&emsp;除了上面几个对象外，我们不能定义自己的`istream`或`ostream`或`iostream`，因为它们并没有构造函数。



## 未格式化的操作

&emsp;&emsp;之前，我们使用的`<<`和`>>`会根据要 读取或写入的数据类型 来转换成对应格式，并且默认忽略空白符，这种叫格式化的IO操作。如果只是要单纯地提取字节，并且**不忽略空白符**，可以用底层操作，也就是未格式化的操作。

### 单字节操作

```c++
char ch;
cin.get();//将下一个字节作为int返回
cin.get(ch);//输入一个字节到ch，返回cin或0（读取到文件结束符时）
cout.put(ch);//输出一个ch字节
```

```c++
cin.peek();//偷窥~以int类型返回下一个字节，但不从流中删去
cin.unget();//将上一个读取的字节放回去
cin.putback(ch);//如果上一个读取的字节是ch，就放回去
```

&emsp;&emsp;注意，`cin.get()`和`cin.peek()`是返回一个`int`而不是`char`，这样我们就可以返回文件尾标记（`EOF`）。而`char`中每一个都表示一个真实的字符，不能表示文件尾。

### 多字节操作

* `cin.get(char sink[], int size, char delim)`

  从cin中读取最多size个字符，如果遇到delim或文件尾则结束（delim不会读取出来，保留在流中），读取的字符存放到sink内。

* `cin.getline(char sink[], int size, char delim)`

  和上面类似，不过会读取并丢弃delim.

* `cin.read(char sink[], int size)`

  读取size个字节放入sink中，返回cin

* `cin.gcount()`

  返回上一个未格式化的操作（不包括gcount）从is读取的字节数。如果是`peek`、`unget`、`putback`，则返回0

* `cout.write(char sink[], int size)`

  将sink中size个字节存入cout中，返回cout

* `cin.ignore(int size, char delim)`

  读取并忽略最多size个字符，包括delim。与其他未格式化的操作不同，ignore有默认参数size=1, delim=eof





# 文件流(fstream)

&emsp;&emsp;头文件`fstream`中定义了三个类：只读文件`ifstream`、只写文件`ofstream`、读写文件`fstream`。它们分别继承自`istream`、`ostream`和`iostream`，因此可以在函数参数中用文件流代替相应的标准流。

## 打开文件

&emsp;&emsp;我们先定义一个文件流对象，然后再将对象与文件关联起来：

```c++
//方法一
ifstream infile("input.txt");//文件名可以是c字符串或string
//方法二
ifstream infile;
infile.open("input.txt");
```

&emsp;&emsp;文件有不同的打开方式，如下：

| 标识常量 | 值     | 意义   |
| -------- | ------ | ------ |
| ios::in  | 0x0001 | 读方式 |
|ios::out|0x0002|写方式|
|ios::ate|0x0004|打开文件后文件指针定位到文件末尾|
|ios::app|0x0008|每次的写入内容都追加到文件末尾|
|ios::trunc|0x0010|删除文件已有内容|
|ios::nocreate|0x0020|如果文件不存在，则打开失败|
|ios::noreplace|0x0040|如果文件存在，则打开失败|
|ios::binary|0x0080|以二进制方式打开|

* `ifstream`不能以`ios::out`打开，`ofstream`不能以`ifstream`打开；
* `ofstream`默认以`ios::out||ios::trunc`打开，即默认会删除原文件。要想保留源文件，可以用`ios::out||iot::app`或`ios::out||ios::in`；
* `ios::trunc`只能在`ios::out`设定时才能设置；并且`ios::trunc`和`ios::app`不能同时设定；
* 在`ios::app`模式下，即使没有指定`ios::out`，文件也会以写方式打开；
* 默认情况下，文件以文本模式打开

&emsp;&emsp;根据上面所说的，我们可以指定打开方式：

```c++
fstream file("data.txt", ios::in||ios::out||ios::app);
```

&emsp;&emsp;如果打开失败，`failbit`会被置位，此时，`if(file)`为false。一旦一个文件流与一个文件关联，再调用open会导致文件流failbit被置位，因此，要关闭后才能打开新的文件。



## 关闭文件

&emsp;&emsp;当一个文件用完后，最好即使关闭，即调用`file.close()`。这样缓冲区的数据会写入文件，并添加文件结束标志，切断文件流与文件的联系。

&emsp;&emsp;尽管文件流在析构时会自动调用`file.close()`（至于什么时候调用析构函数，可看之前的类基础博文），但我们最好手动写上，防止程序在中途崩溃。



## 读写文件

&emsp;&emsp;读写文件的操作与`cin`、`cout`类似，可以用`>>`、`<<`。

&emsp;&emsp;当然，`file.get()`、`file.getline()`等函数也是可以用的。

&emsp;&emsp;如果移动读指针用`seekg()`，写指针用`seekp()`，这和前面也是一样的。

&emsp;&emsp;唯一有点麻烦的是二进制文件，我们只能通过下面这种方法写入：

```c++
ofstream outfile("data", ios::binary);
int data=10;
file.write( (char*)&data, sizeof(data) );//将&data以及后面总共sizeof(data)的数据写入
```

&emsp;&emsp;而且读二进制文件也要这样读：

```c++
ifstream infile("data", ios::binary);
int data;
file.write( (char*)&data, sizeof(data) );//将sizeof(data)的数据写入&data以及后面的空间
```





# 串流(strstream/sstream)

&emsp;&emsp;串流有两类，一类是以C类型字符串为流的`strstream`，一类是以string为流的`sstream`。串流用起来与`cin`和`cout`没什么不同（毕竟是由它俩派生的嘛~），不过原理上，串流是将数据以字符串的格式储存，再将字符串格式的数据输出到数据中，因此它很适合当“中间类”。比如要一次读文件的一行：

```c++
ifstream infile("input.txt");
string line;
stringstream ss;
while(infile){
    infile.getline(line);
    ss<<line;
    ...
}
```






# 更多拓展知识

* [C++基本的输入输出](https://www.runoob.com/cplusplus/cpp-basic-input-output.html)
* [C++: ios标志位](https://www.cnblogs.com/reasno/p/4875656.html)

<div id="cnblogs_post_body">
<p>&nbsp;</p>
<p>&nbsp;</p>
<div class="table-box"><table border="1" cellpadding="0" cellspacing="0" width="747"><tbody><tr><td rowspan="2" width="125">
<p align="center"><strong>文件流</strong></p>
</td>
<td colspan="2" width="257">
<p align="center"><strong>ios::app</strong></p>
</td>
<td colspan="2" width="257">
<p align="center"><strong>ios::ate</strong></p>
</td>
</tr><tr><td width="142">
<p align="center"><strong>打开方式</strong></p>
</td>
<td width="115">
<p align="center"><strong>结果</strong></p>
</td>
<td width="149">
<p align="center"><strong>打开方式</strong></p>
</td>
<td width="108">
<p align="center"><strong>结果</strong></p>
</td>
</tr><tr><td rowspan="2" width="125" style="text-align:center;">
<p><strong>ofstream</strong></p>
<p>(默认是ios::in | ios::trunc)</p>
</td>
<td width="142">
<p>ios::app或ios::app|ios::out</p>
</td>
<td width="115">
<p>如果没有文件，生成空文件；</p>
<p>如果有文件，在文件尾追加</p>
</td>
<td width="149">
<p>ios::ate或ios::ate|ios::out</p>
</td>
<td width="108">
<p>如果没有文件，生成空文件；</p>
<p>如果有文件，清空该文件</p>
</td>
</tr><tr><td width="142">
<p>ios::app|ios::in</p>
</td>
<td width="115">
<p>不管有没有文件，都是失败</p>
</td>
<td width="149">
<p>ios::ate|ios::in</p>
</td>
<td width="108">
<p>如果没有文件，打开失败；</p>
<p>如果有文件，定位到文件尾，可以写文件，但是不能读文件</p>
</td>
</tr><tr><td rowspan="2" width="125" style="text-align:center;">
<p><strong>Ifstream</strong></p>
<p>(默认是ios::in)</p>
</td>
<td width="142">
<p>ios::app或ios::app|ios::out</p>
</td>
<td width="115">
<p>不管有没有文件，都是失败</p>
</td>
<td width="149">
<p>ios::ate或ios::ate|ios::out</p>
</td>
<td width="108">
<p>如果没有文件，打开失败；<br>
如果有文件，定位到文件尾，但是不能写文件</p>
</td>
</tr><tr><td width="142">
<p>ios::app|ios::in</p>
</td>
<td width="115">
<p>?</p>
</td>
<td width="149">
<p>ios::ate|ios::in</p>
</td>
<td width="108">
<p>?</p>
</td>
</tr><tr><td rowspan="3" width="125" style="text-align:center;">
<p><strong>fstream</strong></p>
<p>(默认是ios::in | ios::out)</p>
</td>
<td width="142">
<p>ios::app|ios::out</p>
</td>
<td width="115">
<p>如果没有文件，创建文件；</p>
<p>如果有文件，在文件尾追加</p>
</td>
<td width="149">
<p>ios::ate|ios::out</p>
</td>
<td width="108">
<p>如果没有文件，创建文件；</p>
<p>如果有，清空文件</p>
</td>
</tr><tr><td width="142">
<p>ios::app|ios::in</p>
</td>
<td width="115">
<p>如果没有文件，失败</p>
</td>
<td width="149">
<p>ios::ate|ios::in</p>
</td>
<td width="108">
<p>如果没有文件，失败</p>
</td>
</tr><tr><td width="142">
<p>N/A</p>
</td>
<td width="115">
<p>N/A</p>
</td>
<td width="149">
<p>ios::ate|ios::out|ios::in</p>
</td>
<td width="108">
<p>如果没有文件，打开失败，</p>
<p>如果有文件，定位到文件尾</p>
</td>
</tr><tr><td width="125" style="text-align:center;">
<p><strong>总结</strong></p>
</td>
<td colspan="2" width="257">
<p>ios::app不能和ios::in相配合,</p>
<p>但可以和ios::out配合，打开输入流</p>
</td>
<td colspan="2" width="257">
<p>ios::ate可以和ios::in配合，此时定位到文件尾；</p>
<p>如果没有ios::in相配合而只是同ios::out配合，那么将清空原文件；</p>
</td>
</tr><tr><td width="125" style="text-align:center;">
<p><strong>区别</strong></p>
</td>
<td colspan="2" width="257">
<p>app会在每次写操作之前都把写指针置于文件末尾，</p>
</td>
<td colspan="2" width="257">
<p>而ate模式则只在打开时才将写指针置于文件末尾。在文件操作过程中，可以通过seekp等操作移动指针位置。</p>
</td>
</tr><tr><td width="125" style="text-align:center;">
<p><strong>例子：</strong></p>
<p>多个线程或者进程对一个文件写的时候,假如文件原来的内容是abc</p>
<p>&nbsp;</p>
</td>
<td colspan="2" width="257">
<p>以ios::app：</p>
<p>第一个线程(进程)往里面写了个d,第二个线程(进程)写了个e的话，结果是abcde</p>
</td>
<td colspan="2" width="257">
<p>以ios:ate：</p>
<p>后面写的会覆盖前面一个写的，第一个线程(进程)往里面写了个d,第二个线程(进程)写了个e的话，结果为abce</p>
</td>
</tr></tbody></table></div><p>&nbsp;</p>
<p>参考：</p>
<p><a href="http://bbs.csdn.net/topics/70007597" rel="nofollow" target="_blank">CSDN：ios::app与ios::ate打开方式有什么不同</a></p>
<p><a href="http://utensil.iteye.com/blog/372138" rel="nofollow" target="_blank">ofstream与ate的故事</a></p>
</div>