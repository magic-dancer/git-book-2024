# 平台介绍

本次实验使用的 FPGA 芯片型号为安路 EG4S20开发板, 使用的开发板如下图.

<div align ="center"><img src="/img/lab0/03.jpg" alt="开发板" style="zoom:100%;" /><div align ="center">



<center style="color:#000000;font-size:10pt;">本次实验使用的FPGA开发板</center>

一般来说, 开发板包含基本输入操作类模块、输出显示类模块、蜂鸣器发声模块、时钟模块、I/O 扩展类模块、存储类模块及电源模块. 其中, 基本输入操作类模块主要有拨码开关和矩阵键盘, 用于向系统输入中断信号或操作信号. 输出显示类模块主要用于将实验结果通过 LED 灯或七段数码管表现出来. I/O 扩展类模块主要用于扩展外设, 接收外设输入的信号或向外设输出信号. 本节将对 FPGA 板上的各部分做一个简单的介绍.

## LED 灯

LED 灯对应[上图](#平台介绍)中红框 1 的部分. 本次实验使用的开发板提供8个直接由 FPGA 引脚驱动的 LED 灯 LED0 - LED7, 当 FPGA 输出高电平时, LED 灯点亮.

## 数码管

数码管对应[上图](#平台介绍)中红圈 2 的部分. 开发板上配有4个七段数码管 DIG1-DIG4(当正放 FPGA 开发板时, 从左至右为 1-4), 每个数码管都由一个专用片选信号(DIG1- DIG4)控制. 七段数码管的每个引脚(共阴模式)均连接到 FPGA 芯片上, 当 FPGA 输出高电压时, 对应的字码段点亮, 反之则熄灭. 七段数码管的片选信号也直接与 FPGA 引脚相连, 当 FPGA 输出低电压时, 对应的数码管选中, 反之则不选中.

## 拨码开关

拨码开关对应[上图](#平台介绍)中红圈 3 的部分. 开发板上有 8 个拨动开关 SW7-SW0, 当拨动开关处在 DOWN 位置(靠近开发板边缘)时向 FPGA 相应引脚输入低电平, 当拨动开关在 UP 位置时向 FPGA 相应引脚输入高电平. 

## 调试模块

调试模块对应[上图](#平台介绍)中红圈 4 的部分. 在搭建基于 Cortex-M0 的 SoC 时, 调试系统是必不可少的. 为了方便使用, FPGA 开发板在设计之初便将调试电路封装在板上, 预留出来一个 JTAG 口用于和 PC 机通信, 这样做不仅简化了对调试器内部原理的理解需求, 而且方便了连线. 调试器部分的端口需要约束的有一个时钟信号和一个双向的数据信号. 调试时需要注意.

> #### hint::为什么会有两个接口?
>
> 上面的 DAP-LINK 的接口是用来调试 FPGA 中的 SoC 的, 下面的 JTAG 接口是用来下载比特流文件的. 
>
> 通俗的来说, 当你想要把 Verilog 代码下载到 FPGA 开发板上时, 你应该使用下面的接口; 当你想要使用汇编语言或者 C 语言来控制 FPGA 中实现的 SoC 时, 你应该使用上面的接口. 这部分内容我们将会在[LAB2: "点石成金" - 实现你的首个 SoC](../lab2/introduction.md)中涉及.

## 矩阵键盘

矩阵键盘对应[上图](#平台介绍)中红框 5 的部分. FPGA 板带有一个 4 x 4 的矩阵键盘, 可以用于一些外界的控制交互. 左侧的 KEY1 - KEY8 信号分别代表横竖的4个端口, 分别与 FPGA 上的 4 个输出端口连接. 矩阵键盘的工作原理可以举个例子说明, 如果将 KEY1 设置为低电平, KEY2 - KEY4 设置为高电平, 则按下 KEY15 按键后, KEY5 端口输出低电平, 松开 KEY15 按键后, KEY5 端口输出高电平. 唯一需要注意的是, 按下按键时信号存在抖动, 所以需要设计按键消抖模块以免按键抖动带来未预期的系统错误.



## FPGA芯片

1. 搭配安路 EG4S20开发板，扩展更多实验项目，底板配有
   - 安路 EG4S20口袋实验核心板
   - 320*240 并口液晶屏 或 320*240 SPI液晶屏
   - 一路麦克风输入信号调理电路和ADC采样电路
   - 一路音频输出信号调理和功放电路
   - 一个SD卡插槽
   - 一路RGB565液晶屏输出
   - · 一路RGB565 VGA输出
   - 一路HDMI输出
   - 一路CMSIS DAP调试器，用于微机原理课程中调试FPGA内部M0软核

<div align ="center"><img src="/img/lab0/12.png" alt="开发板" style="zoom:100%;" /><div align ="center">
<center style="color:#000000;font-size:10pt;">本次实验使用的FPGA开发板的扩展板</center>

对于扩展板的更多信息可以访问[微处理器原理与片上系统设计简介](https://www.yuque.com/yingmuketang/01/ybuzk6rfe3lk923i)

> #### hint::使用板子时请注意
>
> 在使用 FPGA 时, 一定要注意资源的合理利用以及避免出现时序问题. 在复杂系统中, 时序违例是一个很严重的问题, 通常是由于时钟周期小于关键路径(critical path)导致. 但现在的 EDA 工具的时序分析功能已经非常强大, 所以我们在学习时一定要多看工具输出的报告, 养成良好的习惯. 