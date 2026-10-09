[English](../en/resources.md) | 简体中文 | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>一些资源</title>

# Lerbot官方的[URDF文件](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf)



## URDF Studio

https://urdf.d-robotics.cc/



## ROS2 仿真控制（可自行实现）

https://github.com/holmsslk/so-arm-moveit-hardware



## LeRobot的官方图形界面

https://github.com/huggingface/leLab

LeLab是一款网页应用，它将 LeRobot 的全部工作流程——校准、远程操控、记录、训练、回放——整合到一个浏览器界面中。只需连接机械臂，打开应用，即可开始操作。无需繁琐的命令行操作，也无需键盘输入。

🤗 LeRobot 的原生网页入口，旨在让新用户在几分钟内完成从“开箱”到“训练他们的第一个保单”的整个过程。

🤗 只需一条命令即可安装并运行所有程序。



# 手机控制从动臂

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## 云端机器人研发：基于 AWS 实现 ROS 2 设备与 Isaac Sim 的 Lerobot 仿真及数据流

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## 网页端设置舵机ID和中位校准

https://bambot.org/feetech.js?lang=zh

1、根据舵机型号输入0或1，点击“连接”

![图片展示的是网页端设置舵机ID和中位校准时的连接界面。界面中有“连接”部分，包括波特率选择框（1,000,000 bps，Index 0）、协议端选择框（0=STS/SMS，1=SCS，当前选中0）、连接按钮（绿色，带数字2标识）以及状态显示（红色，显示“已断开”）。该图片与上文提到的“根据舵机型号输入0或1，点击‘连接’”相呼应，直观呈现了连接操作的界面设置情况。](../en/images/d58-01.png)

2、扫描ID 1\~6 的舵机，可以根据扫描结果里的FOUND确认对应ID舵机。例如图片里舵机 ID 1 被扫描到了

![这张是Lerbot相关软件的扫描舵机界面截图，核心内容是舵机扫描的操作过程与结果。界面设有“起始ID”“结束ID”输入框，分别输入了1和6，还有“开始扫描”按钮，下方是对应的扫描结果显示区域，该位置对应文档中提到的舵机ID被扫描到的相关内容。扫描结果的日志显示，仅在扫描ID1时成功获取到舵机信息，显示“FOUND: Pos-1230, Mode=0, BaudIdx=0”，扫描ID2到ID6时均出现了读取位置异常、无状态包的错误提示。](../en/images/d58-02.png)

3、ID设置和中位校准

①当前舵机ID输入为被扫描到的舵机ID

②在“ID管理”中输入数字，点击“更改ID”即可设置ID

③中位校准（STS3215舵机中位是2047，SCS0009舵机中位是511）

STS舵机：在“位置控制”输入2047，并点击“Set”

SCS舵机：在“位置控制”输入511，并点击“Set”

![图片展示了Lerbot官方URDF Studio中单个舵机控制界面。界面中“当前舵机ID”显示为1，下方“ID管理”处可输入数字并点击“更改ID”；“位置控制”区域有“读取位置”按钮，显示位置为2047，旁边有“Set”按钮。该图片与上下文紧密相关，直观呈现了上下文中提到的当前舵机ID输入为被扫描到的舵机ID、在“ID管理”中输入数字点击“更改ID”设置ID、STS舵机在“位置控制”输入2047并点击“Set”等操作步骤。](../en/images/d58-03.png)
