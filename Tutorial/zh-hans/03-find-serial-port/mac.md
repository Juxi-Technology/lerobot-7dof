[English](../en/03-find-serial-port/mac.md) | 简体中文 | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# MAC电脑

## 查看端口

```Shell
ls /dev/tty.*
```

效果类似下图，用两个端口中的任意一个都可

![图片展示了在MAC电脑终端中执行“ls /dev/tty.*”命令后的输出结果，列出了多个端口信息。其中，有两个端口“/dev/tty.usbmodem5AAF2194741”和“/dev/tty.wchusbserial5AAF2194741”被红色框高亮显示。上下文提到查看端口，效果类似此图，使用两个端口中的任意一个即可，图片直观呈现了所需查看的端口信息，与上文查看端口的内容紧密相关，是对查看端口操作结果的展示。](../../en/images/d09-01.png)

## 给端口赋予权限

让所有用户都有权限读写这些串口设备

```Shell
chmod 666 /dev/tty.*
```

## 记录我的端口

从动臂：

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

主动臂：

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## 为什么Mac中会有两个端口？

我们用的舵机控制板，在Mac系统中被**同时识别为了两种不同类型的串口驱动**，所以会显示两个端口：

- 一个是系统默认的通用串口驱动（`/dev/tty.usbmodemxxxx`）
- 另一个是芯片厂商（比如这里的 “wch” 对应南京沁恒的 CH340/CH341 芯片）提供的专用串口驱动（`/dev/tty.wchusbserialxxxx`）

这属于正常现象，**两个端口其实对应同一个硬件设备**，选择其中任意一个都可以连接通信（比如在控制机械臂的软件中选择其中一个端口即可）。

如果后续操作某一个端口报错，可以换成另一个端口试试。