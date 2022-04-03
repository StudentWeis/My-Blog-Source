---
title: STM32H750+CubeMX+DCMI+OV2640+USBFS拍摄传输JPEG格式图片
date: 2021-12-24 20:18:41
tags: 
        - 单片机
        - STM32
        - CubeMX
categories: 
        - 单片机
---

淘宝买了一个带摄像头接口的 STM32H750 的核心板，想着学一下摄像头模块的使用。花了好久才把厂家给的例程跑通，但是例程不是 CubeMX 的，感觉不方便后续使用，就想移植过来。网上几乎没有使用 CubeMX 开发的资料，自己废了好大的功夫才找到一个相关的 [Github 仓库](https://github.com/McflyWZX/DCMItest)，下载下来发现非常简洁，稍微修改了一下，顺利拍摄图片。

源工程是使用串口来传输图片的，我个人不太喜欢这种方式，改成了 USB 虚拟串口传输。把工程整理了一遍，在这里稍微总结一下，以便后人。

<!-- more -->

完整工程在 Github、Gitee 上开源：

- Github：https://github.com/StudentWeis/STM32H750VBT-CubeMX
- Gitee：https://gitee.com/studentwei/stm32h750vbt-cubmx

开发环境如下所示：👇

- 开发方式：STM32CubeMX + Keil 5.32
- 开发板：DevEBox STM32H750 V2.0
- 摄像头：OV2640 + 淘宝优信电子摄像头底板

## 一、CubeMX 配置

需要设置的外设：USBFS、I2C、DCMI、GPIO。

### USBFS

选择 USB_OTG_FS >> Device_Only，然后 Middleware >> Virtual Port Com 即可，其他保持默认。

![USB OTG FS 设置](https://s2.loli.net/2021/12/24/uU5M6LIFZe2xbyi.jpg)

![USB Device 设置](https://s2.loli.net/2021/12/24/XKbsLegSwnxFp9k.jpg)

### I2C

I2C 设置为 Fast Mode，GPIO 设置为 Pull-up，其他保持默认。

![I2C 设置](https://s2.loli.net/2021/12/24/YpL7V6ImPRgQhuE.jpg)

![I2C GPIO 设置](https://s2.loli.net/2021/12/24/Kf4OgbtEc8xwNLF.jpg)

### DCMI

- DCMI >> Mode >> DCMI >> Slave 8 bits External Synchro；
- DCMI >> Pixel clock polarity >> Active on Rising edge；
- JPEG mode >> Enabled；
- NVIC 开启；
- DMA 开启；

![DCMI 设置](https://s2.loli.net/2021/12/24/vzwd71ElcCXJeMu.jpg)

![DCMI DMA 设置](https://s2.loli.net/2021/12/24/kERB2LyMirn3Cl9.jpg)

![DCMI NVIC 设置](https://s2.loli.net/2021/12/24/DfcgH5x1sEwl2N6.jpg)

### GPIO

配置 OV2640 复位引脚。

![GPIO 设置](https://s2.loli.net/2021/12/24/9VKfP4swqzZxiWl.jpg)

### 其他配置

![时钟设置](https://s2.loli.net/2021/12/24/oEzRf6XtJxB2I41.jpg)

![STM32H7-USB时钟设置](https://s2.loli.net/2021/12/17/atWkwGL8JcUmgxh.jpg)

**注意**：这里 USB 的时钟源选择为 RC48，否则可能无法正常运行。

![代码生成设置](https://s2.loli.net/2021/12/24/ERzYd5ak3jGvpXw.jpg)

![堆栈设置](https://s2.loli.net/2021/12/24/fxWrlh6B1Pqn2kK.jpg)

**注意**：这里堆栈要设置大一点，不然 USB 无法完成初始化。

## 二、代码

完整代码均已开源，自行下载查看，这里只对一些关键的程序说明。

### USB printf 重定向

```c
int fputc(int ch, FILE *f)
{
    while (!(CDC_Transmit_FS((uint8_t *)&ch, 1) == USBD_OK))
        ;
    return ch;
}
```

### OV2640.c 关键程序

```c
void StartOV2640()
{
    __HAL_DCMI_ENABLE_IT(DCMI_hdcmi, DCMI_IT_FRAME);                                               // 使用帧中断
    memset(JpegBuffer, 0, sizeof(JpegBuffer));                                                     // 把接收BUF清空
    HAL_DCMI_Start_DMA(DCMI_hdcmi, DCMI_MODE_SNAPSHOT, (uint32_t)JpegBuffer, pictureBufferLength); // 启动拍照
}

extern uint8_t jpgok;
void HAL_DCMI_FrameEventCallback(DCMI_HandleTypeDef *hdcmi)
{
    HAL_DCMI_Suspend(DCMI_hdcmi); // 拍照完成，挂起 DCMI
    HAL_DCMI_Stop(DCMI_hdcmi);    // 拍照完成，停止 DMA传输
    uint16_t i, jpgstart, jpglen = 0;
    uint8_t *jpgp = (uint8_t *)JpegBuffer;
    uint8_t headok = 0;
    for (i = 0; i < 65535; i++) // 查找 0xFF, 0xD8, 0xFF 和 0xFF, 0xD9, 获取 jpg 文件大小
    {
        if ((jpgp[i] == 0XFF) && (jpgp[i + 1] == 0XD8) && (jpgp[i + 2] == 0xFF) && (!headok)) // 找到 FF D8 FF
        {
            jpgstart = i;
            headok = 1; // 标记找到 jpg 头
        }
        if ((jpgp[i] == 0XFF) && (jpgp[i + 1] == 0XD9) && headok) // 找到头以后,再找 FF D9
        {
            jpglen = i - jpgstart + 2;
            break;
        }
    }
    jpgp += jpgstart; 
    CDC_Transmit_FS(jpgp, jpglen);
    jpgok = 1;
}
```

## 三、结果处理

实际收到的是 JPEG 图片的十六进制格式，在串口助手中点击“保存窗口”，再在[十六进制编辑器](https://hexed.it/)中编辑修改。

JPEG 的文件头是 `FF D8`，文件尾是 `FF D9`，将这部分之外的全删除即可
