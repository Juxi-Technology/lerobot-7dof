English | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ubuntu Computer

## Grant Permissions to the Port

Give all users permission to read and write these serial devices

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Calibrate the Follower Arm

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![The image shows the terminal interface on an Ubuntu computer running the "lerobot-calibrate" command to calibrate the Follower arm. It displays the Follower's connection information, joint names and upper/lower limit values. Key information includes: press Enter to start calibration, turn each joint through its upper and lower limits in turn, press Enter to finish calibration; and "Calibration saved to" and other calibration file path information. This image is closely related to the steps for calibrating the Follower arm, visually presenting the terminal feedback during calibration.](../../en/images/d12-01.png)

## Calibrate the Leader Arm

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![The image shows the interface on an Ubuntu computer after granting permissions to the port. The command line entered "sudo chmod 666 /dev/ttyACM*", and after execution displayed information such as "zihao_leader_arm". Below are prompts such as "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" and "press Enter to finish calibration", along with "Calibration saved to" and other calibration-related path information. This image corresponds to the "Calibrate the Leader Arm" section, visually presenting the preparation interface before calibration.](../../en/images/d12-02.png)

## View the Calibration Configuration File

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![The image shows the contents of the "zihao_follower_arm.json" file displayed in the Ubuntu terminal. The file contains configuration information for several arms, such as shoulder_pan, shoulder_lift, elbow_flex and wrist_flex, each arm having parameters like id, drive_mode, homing_offset, range_min and range_max. This image relates to the "View the Calibration Configuration File" section, visually presenting the specific parameter information in the calibration file and helping users understand the configuration of each arm.](../../en/images/d12-03.png)



## Notes

### ① One arm stops moving after reaching a limit

It needs to be recalibrated

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos not found

![This is a screenshot showing an error interface in the Ubuntu terminal, corresponding to the "servos not found" note. The interface reports a RuntimeError, specifically "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", i.e. the servo check failed. It also lists the expected servo information, with expected motor IDs 1-6 and expected model 777, but the list of motors actually found is empty; combined with the context, this error is caused by the servos not being powered.](../../en/images/d12-04.png)

The servo power is not plugged in