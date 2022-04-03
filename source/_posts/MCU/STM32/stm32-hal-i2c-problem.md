---
title: STM32-HAL库I2C函数地址问题
date: 2021-12-15 13:27:41
tags: 
		- 单片机
		- STM32
categories: 
        - 单片机
---

前几天使用 CubeMX 开发 STM32 和 OV2640 的通信，其中用到了 I2C 作为摄像头的控制通信。但是死活无法建立连接，只好通过示波器观察 I2C 的实际波形。才发现 STM32 HAL 的库函数好像有些问题。在这里记录一下，留待探讨。

<!-- more -->

使用的芯片是 STM32F407ZGT6，使用的 Firmware Package 版本是 1.26.2。

使用库函数 `HAL_I2C_Master_Transmit` 函数发送地址 `0x60`，示波器观察到的波形如下图：👇

![STM32HAL库问题](https://s2.loli.net/2021/12/14/Zmech4PBHawIFbt.png)

显然，实际发送的地址不是 `0x60`，而是 `0x30`，也就是右移了一位，查看库函数手册《STM32F479xx_User_Manual》之后才了解到👇

```c
* @brief  Transmits in master mode an amount of data in blocking mode.
* @param  hi2c Pointer to a I2C_HandleTypeDef structure that contains
*                the configuration information for the specified I2C.
* @param  DevAddress Target device address: The device 7 bits address value
*         in datasheet must be shifted to the left before calling the interface
* @param  pData Pointer to data buffer
* @param  Size Amount of data to be sent
* @param  Timeout Timeout duration
* @retval HAL status
*/
HAL_StatusTypeDef HAL_I2C_Master_Transmit(I2C_HandleTypeDef *hi2c, uint16_t DevAddress, uint8_t *pData, uint16_t Size, uint32_t Timeout)
```

DevAddress Target device address: The device 7 bits address value in datasheet must be shifted to the left before calling the interface 这句表示，在使用这个函数之前，需要先将对应的地址左移一位。也就是说，如果设备地址为 `0x60`，实际调用函数时要写的地址应该是 `0xc0`。



---



**2021.12.17 更新**

这个问题其实不是 STM32 本身的硬件问题，而是库函数的一点特殊的要求而已。在这里记录只是为了把这次的经验分享给大家。

