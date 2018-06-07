#  i.MX RT1050

## 1. 简介

i.MX RT 1050系列芯片，是由 NXP 半导体公司推出的跨界处理器芯片。它基于应用处理器的芯片架构，采用了微控制器的内核Cortex-M7，从而具有应用处理器的高性能及丰富的功能，又具备传统微控制器的易用、实时及低功耗的特性。

BSP默认支持的i.MX RT1052处理器具备以下简要的特性：

| 介绍 | 描述 |
| ---- | ---- |
| 主CPU平台 | ARM Cortex-M7 |
| 最高频率 | 600MHz |
| 内部存储器 | 512KB  SRAM |
| 外部存储器接口 | NAND、eMMC、QuadSPI NOR Flash 和 Parallel NOR Flash |

## 2. 编译说明

i.MX RT1050板级包支持MDK5﹑IAR开发环境和GCC编译器，以下是具体版本信息：

| IDE/编译器 | 已测试版本 |
| ---------- | --------- |
| MDK5 | MDK525 |
| IAR | IAR 8.11.3.13984 |
| GCC | GCC 5.4.1 20160919 (release) |

## 3.BSP使用

i.MX RT1052 BSP支持多块开发板，包括官方开发板MIMXRT1050-EVK，野火的i.MX RT1052开发板等。如果不是基于官方开发板，那么需要重新配置并生成工程：

- 在bsp下打开env工具
- 输入`menuconfig`命令，`RT1052 Board select (***)-->`选择正确的开发板。
- 输入`scons --target=mdk5 -s`或`scons --target=iar`来生成需要的工程


## 4. 驱动支持情况及计划

| 驱动 | 支持情况  | 备注 |
| ------ | ----  | ------ |
| UART | 支持 | UART 1~8 |
| GPIO | 支持 |  |
| IIC | 支持 | IIC 1~4 |
| SPI | 支持 | SPI 1~4 |
| ETH | 支持 | 暂时仅支持官方的ETH |
| LCD | 支持 |  |
| RTC | 支持 |  |
| SDIO | 支持 | 暂时仅仅支持一个SDIO，还不支持中断方式 |
| SDRAM | 支持 | 32M SDRAM，后面2M作为Non Cache区域 |

