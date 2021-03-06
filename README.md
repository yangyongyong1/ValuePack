# ValuePack

## 数据结构

**ValuePack要解决的问题是 多个不同类型的变量如何通过串口传输。为了解决这个问题，我定义了一种数据包结构。**
- 包头
- 原数据
- 校验
- 包尾

其中包头和包尾分别固定为 0xA5 和 0x5A，校验为所有原数据字节之和的低8位。
在原数据中：
定义了五种不同的类型 bool、byte、short、int和float。五种类型的变量按照严格的顺序排列，其中
- bool占1/8字节
- byte占1字节
- short占2字节
- int占4字节
- float占4字节

## 执行流程

**发送时：**
发送方将要发送的数据按照顺序填入原数据，然后加上包头、校验和包尾，最终得到一串字节串，然后通过串口或其它方式发出。

**接收时：**
接收方建立一个较大的缓冲区，一直接收发来的字节，并且定时解析缓冲区中是否已有完整数据包，如果有完整数据包则将数据包中的数据按照顺序一一读出。



-Code：
--Code中是ValuePack的实现代码，三种代码均是STM32上运行的，一般正常使用选择USART+DMA即可。

-Examples：
--Examples中是一系列围绕 蓝牙调试器 这个安卓应用实现的STM32代码。蓝牙调试器是一款可自由设置收发变量和控件布局的无线调试工具，
它可以让你在极短时间创建属于自己的“手机+”应用。


详细介绍在此：
https://www.jianshu.com/p/1a8262492619
只需在工程中添加ValuePack目录下的valuepack.c 和 valuepack.h两个文件就可以通过调用函数的方式实现数据打包。
