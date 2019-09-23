title: "我把看vc遇到的各种鬼全都丢在这里面...!"
date: 2014-11-19 10:20:25
tags: "C++"
---

最近在看一个ClientPrint程序涉及到vc+vb编程，各种以前没有见到过得奇怪的东东我全部摆在这里留念了！

##CWPSTRUCT structure
---
Defines the message parameters passed to a WH_CALLWNDPROC hook procedure,
[CallWndProc][1].
###Syntax:
```c++
typedef struct tagCWPSTRUCT {
  LPARAM lParam;
  WPARAM wParam;
  UINT   message;
  HWND   hwnd;
} CWPSTRUCT, *PCWPSTRUCT, *LPCWPSTRUCT;
```
###Members:

*lParam*
　　　Type: **LPARAM** 
　　　Additional information about the message. The exact meaning depends on the **message** value. 
*wParam* 
　　　Type: **WPARAM** 
　　　Additional information about the message. The exact meaning depends 　　　on the **message** value. 
*message* 
　　　Type: **UINT** 
　　　The message. 
*hwnd* 
　　　Type: **HWND** 
　　　A handle to the window to receive the message.

##EnumChildWindows
---
              
			  jiguo 
              fjislfajsd
              fsafijsdf

##CallWndProc callback function
---
An application-defined or library-defined callback function used with the [SetWindowsHookEx][2] function. The system calls this function before calling the window procedure to process a message sent to the thread.

The **HOOKPROC** type defines a pointer to this callback function. `CallWndProc` is a placeholder for the application-defined or library-defined function name.
###Syntax
```c++
LRESULT CALLBACK CallWndProc(
  _In_  int nCode,
  _In_  WPARAM wParam,
  _In_  LPARAM lParam
);
```
###Parameters
nCode [in]
　　　Type: **int**
　　　Specifies whether the hook procedure must process the message. If *nCode* is 
　　　**HC_ACTION**, the hook procedure must process themessage. If *nCode* is less 
　　　than zero, the hook procedure must passthe message to the [CallNextHookEx][3] 
　　　function without further processing and must return the value returned by 
　　　`CallNextHookEx`.
wParam [in]
　　　Type: **WPARAM**
　　　Specifies whether the message was sent by the current thread. If the message 
　　　was sent by the current thread, it is nonzero; otherwise, it is zero.
lParam [in]
　　　Type: **LPARAM**
　　　A pointer to a [CWPSTRUCT][4] structure that contains details about the message.
###Return Value
Type: **LRESULT**
If nCode is less than zero, the hook procedure must return the value returned by [CallNextHookEx][5].

If nCode is greater than or equal to zero, it is highly recommended that you call [CallNextHookEx][6] and return the value it returns; otherwise, other applications that have installed [WH_CALLWNDPROC][7] hooks will not receive hook notifications and may behave incorrectly as a result. If the hook procedure does not call **CallNextHookEx**, the return value should be zero.


  [1]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644975%28v=vs.85%29.aspx
  [2]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644990%28v=vs.85%29.aspx
  [3]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644974%28v=vs.85%29.aspx
  [4]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644964%28v=vs.85%29.aspx
  [5]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644974%28v=vs.85%29.aspx
  [6]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644974%28v=vs.85%29.aspx
  [7]: http://msdn.microsoft.com/en-us/library/windows/desktop/ms644959%28v=vs.85%29.aspx#wh_callwndproc_wh_callwndprocret