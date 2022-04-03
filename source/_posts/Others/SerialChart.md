---
title: 串口示波器 SerialChart 简明介绍
date: 2021-11-19 10:27:23
categories: 
        - 其他
tags: 
        - 串口
        - 示波器
---

做单片机开发时，往往有一些参数需要通过串口发送到 PC 上进行监测，比如说温度值、PID 输出、角度值、速度值等等。直接看发送过来的数据不够直观，如果直接能看到数据的波形图🔀会更加方便。这种设计一般被称作**串口示波器**。

<!-- more -->

### 1. 简介

之前知道微软商店有一款不错的[串口调试助手](https://www.microsoft.com/store/productId/9NBLGGH43HDM)载有串口示波功能，需要付费，效果还没多好。前两天逛稚辉君的 Github 时正好看到大佬移植了一款串口示波器——[SerialChart](https://github.com/peng-zhihui/SerialChart)。试了一下，发现效果很不错，是从 [Starlion](https://github.com/starlino/serialchart) 那里移植过来的。稚辉君的修改了部分 UI 且做了部分翻译，有 120 多 MB；而原版是 85 MB，且更加简洁。所以我打算使用 Starlion 的原版，从[这里](http://www.starlino.com/serialchart)下载。

![SerialChart概览图](https://i.loli.net/2021/11/17/9UTMgnDGyhbtK1C.png)

### 2. 使用

这是一款绿色软件，下载好之后无需安装，直接打开 `SerialChart.exe` 就可以使用。进行简单的配置之后就可以使用 MCU 串口发送数据了，根据选择的数据格式发送数据，根据配置，即可在 PC 端软件的 Chart 窗口里显示波形了。

#### 1）配置

需要添加配置（Configuration），软件有其自定义的一种格式：

```
[section1]
param1 = value
param2 = value
...

[section2]
param1 = value
param2 = value
```

- **Section** 部分代表配置的种类名称；
- **param** 部分代表对应的参数；

显然这个格式是非常简单的。对于串口示波器，我们需要的配置有：

- **基本设置**（**Setup**）：包括串口配置、图纸配置；
- **数据显示**（**Field**）：接收数据的协议，具体波形显示的变量；

#### 2）基本设置

如下例👇所示：

```
[_setup_] 
port = COM11
baudrate = 115200
 
width = 1000 
height = 200
background_color = white 

grid_h_step = 20 
grid_h_color = #EEE
grid_h_origin = 100 
grid_h_origin_color = green
grid_v_step = 10 
grid_v_color = #EEE
grid_v_origin = 100 
grid_v_origin_color = green 
```

- `port`：串口的端口；
- `baudrate`：波特率；
- `width`：波形显示区域宽度；
- `height`：波形显示区域高度；
- `background_color`：波形显示区域背景颜色；
- `grid_h_step`：波形显示区域水平网格线间隔；
- `grid_h_color`：波形显示区域水平网格线颜色；
- `grid_h_origin`：波形显示区域水平网格线起始点；
- `grid_h_origin_color`：波形显示区域水平网格线起始线颜色；
- `grid_v_step`：波形显示区域垂直网格线间隔；
- `grid_v_color`：波形显示区域垂直网格线颜色；
- `grid_v_origin`：波形显示区域垂直网格线起始点；
- `grid_v_origin_color`：波形显示区域垂直网格线起始线颜色；

按上述参数设置出来的波形 区域如下所示：

![SerialChart基本配置](https://i.loli.net/2021/11/19/YT3CfwBbMcGsjp8.jpg)

#### 3）数据显示

SerialChart 默认使用 [CSV](https://baike.baidu.com/item/CSV/10739) 格式进行数据的传输，即逗号分隔值文件格式。以半角逗号 `,` 作分隔符；用换行符 `\n` 作换行区分。通过这个协议，我们便可使用 MCU 的串口发送对应的数据，在 SerialChart 中显示波形。

如下例👇所示：

若想在 PC 端显示两个变量 Field1、Field2 的波形，需要在 MCU 程序中加入如下函数：

```c
while(1)
{
    printf("%d,%d\n", Field1, Field2);
    delay_ms(50);
}
```

再在 SerialChart 中加如下设置：

```
[_default_] 
min = 0
max = 255
 
[_Field1_] 
color = green 

[_Field2_] 
color = red 
min = 0
max = 200 
format = %d
```

- `_default_`：波形显示的默认设置，会自动添加到之后的每一个 Field 中；
- `min`：波形显示的最小值；
- `max`：波形显示的最大值；
- `_Field1_`：接收到的第一个数据的波形变量，若无专门定义则会使用 `_default_` 中的配置；
- `color`：对应波形变量的颜色；
- `format`：数据显示的格式，有如下几种：
  - `%f`：浮点型；
  - `%d`：整型；
  - `%s`：原始数据；
  - `%x`：原始数据的十六进制；

#### 4）更多设置

还有一些更详细的配置本文不做介绍，可以前往 [SerialChart](https://github.com/peng-zhihui/SerialChart) Github 仓库阅读 Readme。

### 3. 示例

完整配置如下所示：

```
[_setup_] 
port = COM11
baudrate = 115200
 
width = 1000 
height = 200
background_color = white 

grid_h_step = 20 
grid_h_color = #EEE
grid_h_origin = 100 
grid_h_origin_color = green
grid_v_step = 10 
grid_v_color = #EEE
grid_v_origin = 100 
grid_v_origin_color = green 

[_default_] 
min = 0
max = 255
 
[_Field1_] 
color = green 

[_Field2_] 
color = red 
min = 0
max = 200 
format = %d
```

![SerialChart示例](https://i.loli.net/2021/11/19/urGtKyZNxhg73B8.gif)
