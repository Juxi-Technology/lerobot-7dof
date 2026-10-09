[English](../en/03-find-serial-port/ubuntu.md) | 简体中文 | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# 方法一：Linux命令行直接查看

## 查看串口设备端口

```Shell
ls /dev/ttyACM*
```

## 连接电脑和机械臂的USB口

先插上Follower从动臂，再继续插上Leader主动臂

![图片展示了在Ubuntu系统下使用Linux命令行查看串口设备端口的操作过程。先是显示“什么都没插”，接着先插上Follower从动臂，出现“/dev/ttyACM0”；再插上Leader主动臂，又出现“/dev/ttyACM1”。图片与上下文紧密相关，直观呈现了连接电脑和机械臂USB口后，通过命令行查看串口设备端口的变化情况，辅助说明了如何通过Linux命令行查看串口设备端口。](../../en/images/d08-01.png)

# 方法二：Lerobot官方工具

```Shell
lerobot-find-port
```

![图片展示的是在Ubuntu系统下使用Lerobot官方工具查看串口设备端口的命令行操作界面。命令为“lerobot-find-port”，显示了所有可用端口，包括多个“/dev/ttyACM”系列端口。操作提示拔掉Follower的USB线，找到其端口号“/dev/ttyACM0”，最后重新插上USB线。该图片与文档中介绍查看串口设备端口的方法二相关，直观呈现了操作步骤及结果。](../../en/images/d08-02.png)

![图片展示的是在Ubuntu系统下使用`lerobot-find-port`命令查看机械臂串口设备端口的过程。命令执行后列出所有可用端口，接着提示拔掉Leader的USB线，最后显示`/dev/ttyACM1`为Leader主动臂的串口设备端口号。该图片与文档中介绍查看串口设备端口的方法二相关，直观呈现了通过Lerobot官方工具获取端口号的操作结果。](../../en/images/d08-03.png)

# 记录我的端口

`/dev/ttyACM0`为Follower从动臂的串口设备端口号

`/dev/ttyACM1`为Leader主动臂的串口设备端口号

# 给端口赋予权限

让所有用户都有权限读写这些串口设备

```Shell
sudo chmod 666 /dev/ttyACM*
```