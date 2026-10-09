[English](../en/10-inference/common-bugs.md) | 简体中文 | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# 常见Bug及解决

## 摄像头获取失败

![图片展示的是LeroBot机器人相关代码运行时的终端输出信息。其中，`INFO`日志显示了OpenCV摄像头打开、Follower断连等信息；`ERROR`日志则指出在`camera_opencv.py`文件中，`read`函数因`OpenCVCamera(0) read failed`而引发`RuntimeError`。该图片与文档中“摄像头获取失败”问题相关，直观呈现了代码运行时出现的错误情况，帮助理解该问题在实际运行中的表现。](../../en/images/d55-01.png)

检查一下腕部摄像头的接线是否松动，特别是靠近摄像头那端的接线，非常容易接触不良

## 摄像头断连

![图片展示的是LeroBot HF在运行过程中出现的摄像头获取失败问题的代码及错误信息界面。界面上方显示了OpenCV摄像头连接失败的提示，下方是`lerobot_record.py`文件中`record_loop`函数的错误堆栈，指出在`async_read`时发生`TimeoutError`，提示摄像头在200ms内未获取到帧。该图片与文档中“摄像头获取失败”问题的上下文对应，直观呈现了该问题在代码层面的具体表现。](../../en/images/d55-02.png)

重新启动一下命令行

## 舵机通信问题1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![这张图片是一段终端报错日志，对应文档中提及的舵机通信问题1的相关内容。日志中显示了具体的代码文件路径，在`lerobot/src/lerobot/motors/motors_bus.py`文件的第1069行、第1181行，记录了`ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`的错误信息，还标注了`There is no status packet!`的错误原因，完整呈现了文档中提到的舵机通信问题1的报错详情。](../../en/images/fix-04.png)

解决方案，把`lerobot/src/lerobot/motors/motors_bus.py`代码中所有`num_retry`都改成99，特别是报错行对应的

![图片展示的是LeroBot项目中`motors_bus.py`代码文件内容。文件中`MotorsBusABC`类的`write`方法被高亮显示，其中`num_retry`变量被修改为`99`。该图片与文档中“舵机通信问题1”部分相关，对应解决方案中提到的把`lerobot/src/lerobot/motors/motors_bus.py`代码中所有`num_retry`都改成99的操作，特别是报错行对应的代码部分。](../../en/images/d55-03.png)

## 舵机通信问题2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![这张图片展示了一份错误日志，属于文档中“舵机通信问题2”对应的报错内容，具体是在执行代码时出现了名为ConnectionError的错误，具体表现为Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries，附带的错误信息还说明There is no status packet，同时还标注了触发该错误的代码所在路径，这份错误日志与文档中“舵机通信问题2”的描述完全对应，是该问题的实际报错记录。](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![图片展示的是在macOS系统下，使用zsh终端执行命令行操作的界面。终端显示了多个文件路径及代码文件内容，其中`/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py`文件中存在`ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`的报错信息。该图片与文档中“舵机通信问题2”内容相关，直观呈现了报错时的终端界面情况。](../../en/images/fix-05.png)
</column>
</grid>

解决方法：重新校准机械臂
