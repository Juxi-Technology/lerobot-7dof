English | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Windows Computer



<callout emoji="🚫">
The Leader and Follower arms must both be connected
</callout>

## Calibrate the Follower Arm

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![The image shows the command line interface on a Windows computer running a lerobot calibration. The command is "lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm". The interface displays calibration information, including prompts such as "zihao_follower_arm SO10IFollower connected", and also lists the NAME, MIN, POS and MAX values of each joint of the robot arm. During calibration, it prompts the user to move the robot arm to the middle of its range of motion and press ENTER, while recording positions, and press ENTER to stop. This image relates to calibrating the Follower arm, showing the specific steps and interface feedback.](../../en/images/d14-01.png)

## Calibrate the Leader Arm

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![The image shows the command line interface calibrating the robot arm with the lerobot-calibrate command on a Windows computer. It displays information about calibrating the Follower and Leader arms, including the calibration position save path, robot type, port number and ID. It also prompts you to move the Follower to the middle of its range of motion and press ENTER, pass each joint through its full range of motion, record positions, and press ENTER to stop. At the bottom it displays the name, minimum value, current position and maximum value of each joint.](../../en/images/d14-02.png)

## Where the Files Are Exported

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Calibrating a Different Robot Arm

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Notes

### ① One arm stops moving after reaching a limit

It needs to be recalibrated

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos not found

![The image shows an error message when running the lerobot program under macOS. While the program runs, a RuntimeError appears: "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", indicating missing servo IDs, including servos 1-6, all with an expected model number of 777, but the list of servos actually found is empty. This relates to the "servos not found" note; it may be because the servos are not plugged in, so re-plug them and rotate the connector.](../../en/images/d14-03.png)

The servo power is not plugged in; re-plug it and rotate the connector