English | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Windows Computer

## Connect the Camera to the Computer

```Shell
lerobot-find-cameras opencv
```

![This image is the Windows command-line error screen, corresponding to the "camera cannot connect" error scenario mentioned in the document. The error information shown has two parts: a global error "Camera index out of range", and a lerobot-related script error "Failed to connect or configure OpenCV camera 0". The interface also lists the detected camera information: 2 devices named OpenCV Camera, both using the DSHOW backend, format YUY2, width 640, height 480, frame rate 0.0FPS. This error content corresponds to the situation described in the document where the camera cannot connect but still opens normally when switching cameras in Tencent Meeting.](../../en/images/d22-01.png)

## Teleoperation with the Camera Feed Displayed

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

The rerun.io window opens, showing the trajectory of each servo joint in real time, along with the live camera feed

and saves the images to the `C:\Users\username\outputs\captured_images` directory

![The image shows the rerun.io window, displaying the trajectories of each servo joint and the live camera feed in real time. On the left is the blueprint interface, with options such as "teleoperation". In the middle is the trajectory chart, showing joint position data such as "observation_wrist_rot.pos". On the right is the camera feed, showing the scene from the robot's perspective. In the top-right corner it shows "Waiting for data on rerun: http://127.0.0.1:9876/remote...", with data source information below. This image relates to the content describing the rerun.io window showing the camera feed in real time, and visually presents the effect.](../../en/images/d22-02.png)

## If You Run into the Following Error

The camera cannot connect, but switching cameras in Tencent Meeting still opens normally

![The image shows the Windows command-line interface with camera detection results. At the top it shows "Administrator: Command Prompt - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot". Below it outputs "Detected Cameras" information, listing "Camera #0" as OpenCV Camera @ 0, with Type OpenCV, Backend api DSHOW, Default stream profile and other parameters, and also shows error prompts such as "ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)". This image relates to the context of the camera connection problem in the document, visually presenting the related error information.](../../en/images/d22-03.png)

Modify the `lerobot\src\lerobot\cameras\utils.py` file to change the OpenCV backend to `cv2.CAP_SHOW`

![The image shows a section of Python code used to modify the `utils.py` file in the lerobot library, corresponding to the troubleshooting step for the Windows camera-connection error in the document. The code defines a `get_cv2_backend()` function that sets the OpenCV backend for different systems: when the system is Windows, the originally commented-out `return int(cv2.CAP_MSMF)` is commented out and the currently highlighted content is `return int(cv2.CAP_DSHOW)`, with a note beside it explaining that this approach replaces AVFOUNDATION; the function also sets the corresponding OpenCV backend for macOS and Linux. It is the core code modification for fixing the specified camera-connection problem.](../../en/images/d22-04.png)

> This is a bug that even Doubao cannot solve; it is all because the lerobot library is wrapped too deeply, and it is very hard for beginners to debug

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Connecting Multiple Cameras, Teleoperation with the Camera Feeds Displayed

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
