English | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>Some Resources</title>

# Lerbot's official [URDF file](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf)



## URDF Studio

https://urdf.d-robotics.cc/



## ROS2 simulation control (implement it yourself)

https://github.com/holmsslk/so-arm-moveit-hardware



## LeRobot's official graphical interface

https://github.com/huggingface/leLab

LeLab is a web app that brings all of LeRobot's workflow — calibration, teleoperation, recording, training, playback — into a single browser interface. Just connect the robotic arm, open the app, and you can start working. No cumbersome command-line work and no keyboard input required.

🤗 LeRobot's native web entry point, designed to let new users go from "out of the box" to "training their first policy" in a matter of minutes.

🤗 Install and run everything with a single command.



# Controlling the Follower Arm from a Phone

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## Cloud robotics development: ROS 2 devices and Isaac Sim LeRobot simulation and data streaming on AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Setting servo IDs and center calibration in the web UI

https://bambot.org/feetech.js?lang=zh

1. Enter 0 or 1 depending on the servo model, then click "Connect"

![The image shows the connection interface when setting servo IDs and center calibration in the web UI. The interface has a "Connect" section including a baud-rate dropdown (1,000,000 bps, Index 0), a protocol-end dropdown (0=STS/SMS, 1=SCS, currently set to 0), a Connect button (green, marked with the number 2) and a status display (red, showing "Disconnected"). This image echoes the earlier text "Enter 0 or 1 depending on the servo model, then click 'Connect'", visually presenting the interface settings for the connection operation.](../en/images/d58-01.png)

2. Scan servos with IDs 1\~6; use FOUND in the scan results to confirm the corresponding ID servo. For example, in the image servo ID 1 was found

![This is a screenshot of the servo-scanning interface in Lerbot-related software; its core content is the process and results of servo scanning. The interface has "Start ID" and "End ID" input fields, filled with 1 and 6 respectively, plus a "Start scan" button, with the corresponding scan-result display area below, matching the content about scanned servo IDs mentioned in the document. The scan-result log shows that servo information was successfully obtained only when scanning ID1, displaying "FOUND: Pos-1230, Mode=0, BaudIdx=0", while scanning ID2 through ID6 each produced an error about abnormal position reading and no status packet.](../en/images/d58-02.png)

3. ID setting and center calibration

① Set the current servo ID input to the scanned servo's ID

② Enter a number in "ID management" and click "Change ID" to set the ID

③ Center calibration (the STS3215 servo center is 2047, the SCS0009 servo center is 511)

STS servo: enter 2047 in "Position control" and click "Set"

SCS servo: enter 511 in "Position control" and click "Set"

![The image shows the single-servo control interface in Lerbot's official URDF Studio. The "Current servo ID" is shown as 1; below, in "ID management", you can enter a number and click "Change ID"; in the "Position Control" area there is a "Read position" button showing position 2047, next to a "Set" button. This image is closely tied to the context, visually presenting the steps mentioned there: setting the current servo ID input to the scanned servo's ID, entering a number in "ID management" and clicking "Change ID" to set the ID, and entering 2047 in "Position control" and clicking "Set" for an STS servo.](../en/images/d58-03.png)
