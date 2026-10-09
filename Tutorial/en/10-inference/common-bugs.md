English | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# Common Bugs and Fixes

## Camera capture fails

![The image shows the terminal output when LeRobot robot code runs. Among it, the `INFO` logs show the OpenCV camera opening and the Follower disconnecting; the `ERROR` log points out that in the `camera_opencv.py` file, the `read` function raised a `RuntimeError` because of `OpenCVCamera(0) read failed`. This image relates to the "Camera capture fails" issue in the document, visually presenting the error that appears when the code runs and helping to explain how the problem shows up in practice.](../../en/images/d55-01.png)

Check whether the wrist camera cable is loose, especially the end near the camera — that connector is very prone to poor contact

## Camera disconnects

![The image shows the code and error information for the camera-capture failure that occurs while LeRobot HF is running. At the top it shows an OpenCV camera connection failure; below is the error stack of the `record_loop` function in the `lerobot_record.py` file, pointing out that a `TimeoutError` occurred during `async_read` and indicating that no frame was captured from the camera within 200 ms. This image corresponds to the "Camera capture fails" issue in the document, visually presenting the specific manifestation of the problem at the code level.](../../en/images/d55-02.png)

Restart the command line

## Servo communication problem 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![This image is a terminal error log, corresponding to the "Servo communication problem 1" content mentioned in the document. The log shows specific code file paths: lines 1069 and 1181 of the `lerobot/src/lerobot/motors/motors_bus.py` file record the error `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`, and also notes the error cause `There is no status packet!`, completely presenting the details of the servo communication problem 1 error mentioned in the document.](../../en/images/fix-04.png)

The fix: change every `num_retry` in the code at `lerobot/src/lerobot/motors/motors_bus.py` to 99, especially the one on the line that errors

![The image shows the contents of the `motors_bus.py` code file in the LeRobot project. The `write` method of the `MotorsBusABC` class is highlighted, with the `num_retry` variable changed to `99`. This image relates to the "Servo communication problem 1" section of the document, corresponding to the fix of changing every `num_retry` in the code at `lerobot/src/lerobot/motors/motors_bus.py` to 99, especially the code on the line that errors.](../../en/images/d55-03.png)

## Servo communication problem 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![This image shows an error log belonging to the "Servo communication problem 2" error content in the document. Specifically, a ConnectionError occurred while running the code: Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries; the accompanying error message also states There is no status packet, and it also notes the path of the code that triggered the error. This error log corresponds exactly to the description of "Servo communication problem 2" in the document and is the actual error record for that problem.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![The image shows a command-line session in the zsh terminal on macOS. The terminal shows several file paths and code file contents, where the `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` file contains the error `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. This image relates to the "Servo communication problem 2" content in the document, visually presenting the terminal interface at the moment of the error.](../../en/images/fix-05.png)
</column>
</grid>

Solution: recalibrate the robot arm
