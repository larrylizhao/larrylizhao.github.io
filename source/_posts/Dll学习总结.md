title: Dll学习总结
date: 2014-11-18 15:48:45
tags: ["C++", DLL]
---

最近在弄vb+vc的一个打印程序，两者的协同工作用到了dll方面的内容，所以学习的时候在这里做一下笔记。

##VB和VC共同编程的3种方式
----------

  1. VC生成DLL，在VB中调用DLL
  2. VC生成ActiveX控件(.ocx)，在VB中插入
  3. 在VC中生成ActiveX Automation服务器，在VB中调用
相对而言，第一种方法对VC编程者的要求最低。

##先说第一种方法的VC++的编程
----------

首先在VC++中生成Win32 DLL工程。在这个工程中添加几个函数供VB用户调用。一个DLL中的函数要想被VB调用，必须满足两个条件：

 - 一是调用方式为stdcall，
 - 另一个是必须是export的。

要做到第一条，只须在函数声明前加上`__stdcall`关键字。如: 
```c++
short __stdcall sample(short nLen, short *buffer) 
```
上面的例子，nLen是数组大小，buffer是数组地址

要做到第二条，需要在*.def文件中加上如下的几行：
```
LIBRARY      "mydll.DLL"

EXPORTS
	Sample   @1 
```
这里的`sample`是你要在VB中调用的函数名，`@1`表示该函数在DLL中的编号，每个函数都不一样。注意这里的函数名是区分大小写的。

##再谈谈VB的编程
----------

VB调用DLL的方法和调用Windows API的方法是一样的，一般在VB的书中有介绍。对于上面一个例子，先要声明VC函数： 
```
Declare Function sample1 Lib "mydll.dll" (ByVal nLen As Integer, buffer As Integer) As Integer 
```
这里`mydll.dll`是你的dll的名字。你可能已经注意到了两个参数的声明有所不同，第一个参数加上了`ByVal`。规则是这样的：如果在VC中某个参数声明为指针和数组，就不加`ByVal`，否则都要加上`ByVal`。

在VB中调用这个函数采用这样的语法： 
```
sample 10, a(0) 
```
这里的a()数组是用来存放数据的，10为数组长度，这里的第二个参数不能是a()，而必须是要传递的数据中的第一个。这是VB编程的关键。
##下面在说几个可能遇到的问题
----------

 1. 一个问题是VB可能报告找不到dll，你可以把dll放到system目录下，并确保VB的Declare语句正确。
 2. 另一个问题是VB报告找不到需要的函数，这通常是因为在VC中*.def文件没设置。
 3. 第三种情况是VB告诉不能进行转换，这可能是在VC中没有加上__stdcall关键字，也可能是VB和VC的参数类型不一致，注意在VC中int是4个字节(相当于VB的Long)，而VB的Integer只有2个字节。必须保证VB和VC的参数个数相同，所占字节数也一致。
 4. 最后一个要注意的问题是VC中绝对不能出现数组越界的情况，否则会导致VB程序崩溃。

 
