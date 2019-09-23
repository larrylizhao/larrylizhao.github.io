title: 清空StringBuffer的几种方法
date: 2014-11-24 16:09:26
tags: [java,String]
---

在字符串拼接的时候经常用到**StringBuffer**,拼接过程中有时需要清空并重用，记录一下清空**StringBuffer**的几种方法

##`delete`方法
---
```
delete(int start, int end)
```
>移除此序列的子字符串中的字符
```
StringBuffer my_StringBuffer = new StringBuffer();
my_StringBuffer.append('helloworld');    //添加字符串到StringBuffer中
int  sb_length = my_StringBuffer.length();// 取得字符串的长度
my_StringBuffer.delete(0,sb_length);    //删除字符串从0~sb_length-1处的内容 (这个方法就是用来清除StringBuffer中的内容的)
```

##`setLength`方法
---
```
setLength(int newLength)
```
>设置字符序列的长度。
```
my_StringBuffer.setLength(0);   //设置StringBuffer变量的长度为0
```

##`new`方法
直接用`new`清空**StringBuffer**中的内容
```
my_StringBuffer = new StringBuffer()
```
##几种方法效率比较
测试程序：
```
private static void testStringBufferclear() {
     StringBuffer sbf = new  StringBuffer("wwwwww");
     StringBuffer sbi = new  StringBuffer("wwwwww");
     long s1 = System.currentTimeMillis();
     for (int i = 0; i < 500000; i++) {
      sbi.setLength(0);
     }
     long s11 = System.currentTimeMillis();
     System.out.println("StringBuffer-setLength:" + (s11 - s1));
     s1 = System.currentTimeMillis();
     for (int i = 0; i < 500000; i++) {
      sbf.delete(0, sbf.length());
     }
     s11 = System.currentTimeMillis();
     System.out.println("StringBuffer--delete:" + (s11 - s1));
     s1 = System.currentTimeMillis();
     for (int i = 0; i < 500000; i++) {
      sbf = new StringBuffer("");
     }
     s11 = System.currentTimeMillis();
     System.out.println("StringBuffer--new StringBuffer:" + (s11 - s1));
    }
```
测试结果：
```
StringBuffer-setLength:63
StringBuffer--delete:109
StringBuffer--new StringBuffer:78
```
测试结论：
要通过使用`setLength(0)`来清空**StringBuffer**对象中的内容效率最高。

