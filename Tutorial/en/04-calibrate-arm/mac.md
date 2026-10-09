English | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Mac Computer

## Review the Port Numbers

Follower arm:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader arm:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Calibrate the Follower Arm

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![The image shows the command line interface for calibrating the SO101 servos on a Mac. The command is "lerobot-calibrate", with parameters including robot.type, robot.port and robot.id. The interface displays robot configuration information such as "zihao_follower_arm". Below, it prompts you to press "c" and Enter to start calibration, and also shows messages such as "zihao_follower_arm SO101Follower connected". This image corresponds to the "Calibrate the Follower Arm" section, visually presenting the calibration command and interface feedback.](../../en/images/d13-01.png)

![The image shows the command line interface for a LeRobot calibration operation under Ubuntu. The command is "lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm", displaying the Follower's calibration information, including the minimum, maximum and current position of each joint. Key operation prompts are highlighted with red boxes, such as "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" and "press Enter to finish calibration", echoing the calibration steps described in the context.](../../en/images/d13-02.png)

## Calibrate the Leader Arm

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![The image shows the command line interface for a LeRobot calibration under Ubuntu. The command line ran operations such as "sudo chmod 666 /dev/ttyACM*" and "lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1", displaying the port number information of the Follower and Leader arms. The interface also prompts you to press Enter to start calibration, turn each joint through its upper and lower limits in turn, and press Enter to finish, and finally shows the path where the calibration configuration file is saved. This image relates to the LeRobot calibration content, visually presenting the calibration steps.](../../en/images/d13-03.png)

## View the Calibration Configuration File

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Common Bugs

- One or several of the servos cannot be found

![The image shows the servo parameter information displayed during calibration of the SO Follower. The top shows connection information and a calibration prompt asking you to move the Follower to the middle of its range of motion and press ENTER, then pass through all joints' range of motion in order, record positions, and press ENTER to stop. The table below lists the NAME, MIN, POS and MAX values for servos such as shoulder_pan, shoulder_lift, elbow_flex, wrist_flex and gripper. This image relates to calibrating the Follower arm, visually presenting the parameters during calibration.](../../en/images/d13-04.png)



## Notes

### ① One arm stops moving after reaching a limit

It needs to be recalibrated

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos not found

![The image shows the Mac terminal interface with an error message from running the LeRobot robot code. The error states that the FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061', with motor IDs -1 to -6 missing and an expected model of 777. It also lists the full list of expected motors and the full list of motors found. This image relates to the "Common Bugs" section, visually presenting how the "servos not found" problem appears as a runtime error.](../../en/images/d13-05.png)

The servo power is not plugged in