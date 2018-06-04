---
layout: post
title: CSAPP的学习笔记——Lab2、汇编
date: 2018-05-26 20:51
catagories: CSAPP
---

## lea命令与mov命令的区别  
lea命令：  
lea——Load effect address，其作用是获取地址，其一般用法为  
lea a_byte,%ebx (把a_byte的地址放在ebx中)  
还有一些特殊的使用方法，比如  
lea (%rdi,%rdi,2),%edx  
在汇编中，加了括号的部分表示的是M[%rdi + 2 * %rdi]，即内存中，地址为%rdi + 2 * %rdi处的值，假设这个值是x，又由于lea是取地址操作，因此其再取x的地址，即为%rdi + 2 * %rdi，那么这一命令相当于是将%rdi + 2 * %rdi放入了%edx中，也即计算了3 * %rdi，然后把结果放入了%edx中

## Caller saved & Callee saved  
Caller saved registers：这些寄存器用于存储临时变量，在函数调用的过程当中可以被改变，因此如果调用者（Caller）想要保存这些寄存器里面的值，其需要将这些值push到stack上面（即Caller saved，调用者负责保存）  
Callee saved registers：这些寄存器用于存储生存周期较长的变量，其在函数调用后值不会改变，即被调用者需要“保护”这里面的值（Callee saved）

## switch statement
switch语句的汇编代码中存在着一个jump table，jump table中存储着各个case的地址  
switch语句的一般执行情况为：  
* 首先进行预先处理（如进行一些优化等等，比如将case为100、101等情况优化为case为0、1的情况）  
* 接着对default case进行一次显式的比较和跳转  
* 最后利用进行间接跳转，一般格式为	jmpq   *0x4a1330(,%rax,8)	注意，%rax中存储的是switch(x)中的x，乘8的原因在于jump table中的地址是8bytes的，而0x4a1330表示的是jump table第一个元素的位置