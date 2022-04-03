---
title: PIC16F877A(3)ADC的使用
date: 2021-04-16 21:07:33
mathjax: false
tags: 
		- 单片机
		- PIC
categories: 单片机
---

ADC(Analog Digital Converter)，即模拟数字转换器，用于将模拟电压转换为数字信号用于计算系统处理。我们可以单独使用一片 ADC 芯片完成这个功能，但是 PIC16F877 中已经为我们集成了一个 10 位精度的 ADC，我们只需要配置寄存器就可以便捷使用。

<!-- more -->

{% note primary %}

在这篇博客中，我只介绍具体的操作。原理上的寄存器以及外设的介绍之后我会出一篇博客系统地讲解的👋。

{% endnote %}

### 摘要

在本文中，我会介绍以下几个内容：

- 简单的 A/D 转化实验，测量一个电位器的电压；
- 实验的 Proteus 电路连接；
- C 语言代码介绍。

---

## 一、简单的 A/D 转化实验

### 实验描述

通过 AD 采集电位器的电压，用中断方式查询转换结果，将结果的高 8 位以 LED 灯的形式显示出来。

### 实验效果

![chpb6K.gif](https://z3.ax1x.com/2021/04/16/chpb6K.gif)

可以看到，LED 灯的情况基本与电位器的电压值情况一致。

---


## 二、电路连接

![chpHl6.png](https://z3.ax1x.com/2021/04/16/chpHl6.png)

---

## 三、代码介绍

```c
/*********************************************************
*** 功能：通过 AD 采集模拟电压，中断方式查询转换结果，将结果的高 8 位以 LED 灯的形式显示出来。
**********************************************************/
#include<pic.h>           	          		

void Init_AD();
void Delay();
void Delay_ms(int xms);

/*****************************************
* 名    称：main() 
* 功    能：主函数
*****************************************/
void main()                 
{
    Init_AD();
    GO_DONE=1;
	while(1);
}

/*****************************************
* 名    称：Init_AD()
* 功    能：初始化 AD Module
*****************************************/
void Init_AD()
{
    TRISC = 0;  	         // 设置PORTC为输出
    PORTC = 0;               // 设置PORTC初值

	TRISA=0x01;				 //RA0输入
	ADCON1=0x0e;		 	 //左对齐,RA0做模拟输入,其他做普通IO
	ADCON0=0x41;		  	 //fosc/8,RA0,使能AD
    PEIE = 1;
    ADIE = 1;
    GIE  = 1;
    Delay();
}

void interrupt ISR()
{
    ADIF = 0;
    PORTC = ADRESH;
    Delay_ms(500);
    // 手动置 1 GO_DONE
    GO_DONE=1;
}

void Delay()
{
    char i = 0;
    for(;i<200;i++);
}

void Delay_ms(int xms)
{
 	int i,j;
 	for(i=0;i<xms;i++)
 		{ for(j=0;j<71;j++) ; }
}
```

---

## 总结

- 一定要记住手动置位 GO_DONE，这是开始转化的操作。
- 一定要配置好相关的 I/O 口。

