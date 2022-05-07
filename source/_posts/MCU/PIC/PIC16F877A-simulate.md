---
title: PIC16F877A(1)仿真&简单的输入输出
date: 2021-03-25 19:52:17
tags: 
		- 单片机
		- PIC
categories: 单片机
---

PIC16F877A 是一款古老的芯片，之所以我会用到它，是因为这学期的一门课程，毕竟学校的知识总是有滞后性的嘛！但我的原则是：**既然不得不学，那就把它学好**，并把自己收获的知识总结下来分享给大家。

本文是该系列的第一篇。主要介绍通过 Proteus 来对 PIC16F877A 这款单片机做一个简单仿真使用的基础知识。同时，为学会如何**编译**并**仿真测试自己写的程序**，我通过一个简单的输入输出小程序来练手，借此熟悉完整的工作流程。

<!-- more -->

## 一、环境准备

### 1、软件准备

软件：**Proteus 8.6** 

编译器：**HI-TECH C for PIC10/12/16**

HI-TECH C for PIC10/12/16 是一款 C 语言编译器，遵循 PICC 语法，用来编译 PIC 单片机的 C 语言程序。不过由于年代的久远，MicroChip 公司已经放弃了它的维护，但是对我们的这款单片机来说还是绰绰有余的。

以下是下载链接：👇

{% note success %}

- HI-TECH 编译器

  - 下载链接: https://pan.baidu.com/s/1eT4CwKB35tFsuxr4O9c9cA 

  - 提取码: 9qma 

- Proteus 下载：由于版权保护，本站不提供，请自行百度。

{% endnote %}



### 2、安装

1. 双击下载好的 HI-TECH 编译器 `exe` 文件，根据提示安装即可；
2. 打开 Proteus，创建一个新的工程；
3. 点击菜单栏中的“源代码”；
4. 进入源代码界面后，点击选项栏的“系统”；
5. 选择“编译器配置”，下滑可以看到 HI-TECH 编译器的选项；
6. 点击下侧的检查当前，Proteus 便可自动搜索到安装好的 HI-TECH 编译器。

{% note success %}

安装完毕！

{% endnote %}

-----

<center><a href="https://imgtu.com/i/6qDk9O"><img src="https://z3.ax1x.com/2021/03/24/6qDk9O.gif" alt="6qDk9O.gif" border="0" width="80%" /></a></center>

-----

## 二、简单的输入输出实验

### 实验描述

设置一个按键，每按一次按键，LED灯向右👉移动一位。

### 实验效果

<center><a href="https://imgtu.com/i/6xVg9x"><img src="https://z3.ax1x.com/2021/03/27/6xVg9x.gif" alt="6xVg9x.gif" border="0" /></a></center>



## 三、电路

如图所示：

<a href="https://imgtu.com/i/6xVlng"><img src="https://z3.ax1x.com/2021/03/26/6xVlng.png" alt="6xVlng.png" border="0" /></a>



## 四、代码

```c
/******************************************
*** 功能：按下按键，LED灯左移一位。
******************************************/
#include<pic.h>              	
void delay();              		
void delay_mm() ;  
void Keyscan();

char event = 0;
char count = 0;
 
//主函数
void main()                 
{
   TRISC = 0;  		//设置portc为输出
   TRISB = 0x01;  	//设置portb0为输入
   PORTC = 0x01; 

   while (1)         	
   {
       Keyscan();
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

//小延时函数
void delay_mm()              
{
   int t;              
   for (t = 0xfff;t--;);
}

//按键检测函数
void Keyscan()
{   
   // 如果按键第一次被按下
   if(RB0==1)
   {
      //延时消抖
      delay_mm(); 
      if(RB0==1)
	 {
	    // 确认按键被按下后，事件标志位置1
	    event = 1;
	    // 等待按键被松开
	    while(RB0);
	    return;
	 }
   }
}
```



## 五、总结

经过这个实验，我们就大概了解了 PIC16F877A 的开发流程啦！可以开始尝试一些更难的实验了。

完整工程可从这个以下链接中下载：

{% note success %}

百度链接：https://pan.baidu.com/s/15M6OHtSZPb1rmZRaFwFnjw 
提取码：em3m 

{% endnote %}

## 其他

### 参考

https://blog.csdn.net/yunjie167/article/details/83999489

