---
title: PIC16F877A(2)定时/计数器Timer使用
date: 2021-04-03 21:20:41
tags: 
		- 单片机
		- PIC
categories: 单片机
---

上一篇 Blog 我介绍了如何在 Proteus 中仿真 PIC16F877 单片机，并进行了简单的 I/O 实验，按键控制 LED 灯✨。其中的定时 1s 采用了软件延时的方式，但实际中单片机给我们留了一个更适合的外设——Timer，即定时/计数器。它可以相对更精确地进行定时操作，并且不占用主程序的运行线程，可以在计时的时候进行其他的操作，非常好用。

<!-- more -->

{% note primary %}

在这篇博客中，我只介绍具体的操作。原理上的寄存器以及外设的介绍之后我会出一篇博客系统地讲解的👋。

{% endnote %}

### 摘要

在本文中，我会介绍以下几个内容：

- 简单的 Timer 延时实验介绍；
- Proteus 电路连接；
- C 语言代码介绍。

## 一、简单的 Timer 延时实验

### 实验描述

设计一款流水灯，变化的间隔定为 500ms，延时程序用 PIC16F877A 的 Timer0 来完成。

### 实验效果

实验效果如下 GIF 所示：

<center>
<a href="https://imgtu.com/i/cue1Wd"><img src="https://z3.ax1x.com/2021/04/03/cue1Wd.gif" alt="cue1Wd.gif" border="0"></a>
</center>

## 二、电路连接

电路连接如下图所示：

<center>
<a href="https://imgtu.com/i/cue8SA"><img src="https://z3.ax1x.com/2021/04/03/cue8SA.png" alt="cue8SA.png" border="0"></a>
</center>

## 三、代码介绍

```c
/****************************************
*** 功能：Timer0 定时 500ms，流水灯循环
*****************************************/
#include<pic.h>           	          		

// 计数变量
char count = 0;
char event = 0;
int T0COUNT = 0;

void Scan_T0();
 
/*****************************************
* 名    称：main() 
* 功    能：主函数
*****************************************/
void main()                 
{
    TRISC = 0;  	         // 设置portc为输出
    PORTC = 0x01;            // 设置PORTC初值
    T0CS = 0;                // 选择 Timer0 工作方式为定时器
    TMR0 = 6;                // 设置 Timer0 计数初值
    PSA = 0;                 // 选择为 Timer0 分频
    PS0 = 1;PS1 = 0;PS2 = 0; // 1:4 分频
    while (1)         	
    {
        // 检测 T0IF
        Scan_T0();
        if(event)
        {
            event = 0;
            PORTC = PORTC << 1;  
            count++;
            if(count == 8)
            {
               count = 0;
               PORTC = 0x01;
            }  
        }
    }
}

/*****************************************
* 名    称：Scan_T0() 
* 功    能：查询 T0IF 的值，判读 T0 是否溢出
* 结    果：若溢出，则设定全局变量 event 为 1
*****************************************/
void Scan_T0()
{
    if(T0IF)
    {
        TMR0 = 6;       // 恢复初值
        T0IF = 0;       // 恢复中断标志
        T0COUNT ++;
        // 一次溢出 1ms，累计 500 次即可
        if(T0COUNT == 500)
        {
            T0COUNT = 0;
            event = 1;
        }
    }
}
```

### 定时时间计算

Timer0 的溢出时间计算如下式：[^1]
$$
T=\frac{4}{Fosc}*PS*(256-TMR0)
$$
我设定的 Fosc 为 4MHz，分频系数 PS 为 4，TMR0 为 6，计算结果为 1ms。

## 总结

操作还是比较简单的，总的来说就是写好寄存器的初值，然后通过查询或者中断的方式获取溢出状态。在溢出函数中进行累计计数，即可完成任意时间的定时。

且使用 Timer 定时不需要占用 CPU 的运行，是一种高效且节能的方式。





## 参考资料及脚注

[^1]:PIC16F877A Datasheet

