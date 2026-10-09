English | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Mac Computer

## Connect the Camera to the Computer

```Shell
lerobot-find-cameras opencv
```

![The image shows the detection result after connecting a camera to a Mac. It lists the information of two cameras: the external camera and the Mac's built-in front camera. The external camera's Fps is 60.00024 and the built-in camera's Fps is 30.0. This image relates to the content on connecting a camera to a Mac, visually presenting the detection result after connection and helping users understand each camera's type, ID, backend API and other information, as well as each camera's frame rate.](../../en/images/d21-01.png)

## One Camera, Teleoperation with the Camera Feed Displayed

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

After running, teleoperation starts

The rerun.io window opens, showing the trajectory of each servo joint in real time, along with the live camera feed

and saves the images to the `~/username/outputs/captured_images` directory

![This image of the rerun.io interface corresponds to the teleoperation-with-camera content in the document and is the live display for the single-camera teleoperation scenario. On the left is a line-chart area showing the trajectory curves of each servo joint, with legends marked in different colors to distinguish the various joint data; on the right are two stacked windows showing the live indoor feed captured by the cameras; at the bottom is the timeline and stream information area. This scenario also supports saving the images captured by the camera to the specified `~/username/outputs/captured_images` directory.](../../en/images/d21-02.png)

## Multiple Cameras, Teleoperation with the Camera Feeds Displayed

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
