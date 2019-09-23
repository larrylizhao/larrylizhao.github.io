title: VB使用Alias子句简介
date: 2014-11-19 18:21:37
tags: vb
---
Declare语句中的Alias子句是一个可选的部分，用户可以通过它所标识的别名对动态 库中的函数进行引用。例如，在下面的语句中，声明了一个在VB中名为MyFunction的函数，而它在动态库Mydll.dll中最初的名字是MyFunctionX。
```vb
Private Declare Function MyFunction Lib "Mydll.dll" _Alias "MyFunctionX" ( ) As Long 
```
通常在以下几种情况时需要VB使用Alias子句：

##处理使用字符串的系统Windows API过程
---
如果调用的系统Windows API过程要使用字符串，那么声明语句中必须增加一个Alias 子句，以指定正确的字符集。包含字符串的系统WindowsAPI函数实际有两种格式：ANSI和Unicode。因此，在Windows头文件中，每个包含字符串的函数都同时有ANSI版本和Unicode版本。例如，下面是SetWindowText函数的两种C语言描述。可以看到，第一个描述将函数定义为SetWindowTextA，尾部的"A"表明它是一个ANSI函数：
```
WINUSERAPI BOOL WINAPI SetWindowTextA(HWND hWnd, LPCSTR lpString);  
```
第二个描述将它定义为 SetWindowTextW， 尾部的"W" 表明它是一个 Unicode 函数：
```
WINUSERAPI BOOL WINAPI SetWindowTextW(HWND hWnd, LPCWSTR lpString);  
```
因为两个函数实际的名称都不是"SetWindowText"，要引用正确的函数就必 须增加一个Alias子句：
```
Private Declare Function SetWindowText Lib "user32" Alias "SetWindowTextA" (ByVal hwnd As Long, ByVal lpString As String) As Long 
```
>应当注意，对于VB中使用的系统WindowsAPI函数，应该指定函数的ANSI版本，因为只 有WindowsNT才支持Unicode版本，而Windows95不支持这个版本。仅当应用程序只运行 在WindowsNT平台上的时候才可以使用Unicode版本。

##函数名是不标准的名称
---
有时，个别的DLL过程的名称不是有效的标识符。例如，它可能包含了非法的字符（如连字符），或者名称是VB的关键字（如GetObject）。在这种情况下，可以使用Alias关键字。例如，操作环境DLLs中的某些过程名以下划线开始。尽管在VB标识符中允许使用标识符，但是下划线不能作为标识符的第一个字符。为了使用这种过程，必须先声明一个名称合法的过程，然后VB使用Alias子句引用过程的真实名称：
```
Declare Function lopen Lib "kernel32" Alias "_lopen" (ByVal lpPathName As String, ByVal iReadWrite As Long) As Long 
```
在上例中，lopen是VB中使用的过程名称。而_lopen则是动态连接库中可以识别的名称。
##使用序号标识DLL过程
---
要使用序号来声明DLL过程，Alias子句中的字符串需要包含过程的序号，并在序号的 前面加一个数字标记字符(#)。例如，Windowskernel中的GetWindowsDirectory函数的序 号为432；可以用下面的语句来声明该DLL过程：
```
Declare Function GetWindowsDirectory Lib "kernel32" Alias "#432" (ByVal lpBuffer As String, ByVal nSize As Long) As Long 
```
在这里，可以使用任意的合法名称作为过程的名称，VB将用序号在DLL中寻找过程。