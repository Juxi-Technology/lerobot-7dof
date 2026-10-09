English | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# Method 1: Check Directly from the Linux Command Line

## Check the Serial Device Ports

```Shell
ls /dev/ttyACM*
```

## Connect the USB Ports of the Computer and the Robot Arm

Plug in the Follower arm first, then plug in the Leader arm

![The image shows the process of checking serial device ports with the Linux command line under Ubuntu. It first shows "nothing plugged in", then after plugging in the Follower arm "/dev/ttyACM0" appears; then after plugging in the Leader arm "/dev/ttyACM1" appears. The image is closely related to the context, visually presenting how the serial device ports are seen from the command line after connecting the computer and the robot arm's USB ports, helping explain how to check serial device ports from the Linux command line.](../../en/images/d08-01.png)

# Method 2: The Official LeRobot Tool

```Shell
lerobot-find-port
```

![The image shows the command line interface for checking serial device ports with the official LeRobot tool under Ubuntu. The command is "lerobot-find-port", displaying all available ports, including several "/dev/ttyACM" ports. The prompt asks you to unplug the Follower's USB cable and find its port number "/dev/ttyACM0", then plug the USB cable back in. This image relates to method two for checking serial device ports, visually presenting the steps and results.](../../en/images/d08-02.png)

![The image shows the process of using the `lerobot-find-port` command to check the robot arm's serial device ports under Ubuntu. After the command runs, it lists all available ports, then prompts you to unplug the Leader's USB cable, and finally shows `/dev/ttyACM1` as the serial device port number of the Leader arm. This image relates to method two for checking serial device ports, visually presenting the result of obtaining port numbers with the official LeRobot tool.](../../en/images/d08-03.png)

# Record My Ports

`/dev/ttyACM0` is the serial device port number of the Follower arm

`/dev/ttyACM1` is the serial device port number of the Leader arm

# Grant Permissions to the Port

Give all users permission to read and write these serial devices

```Shell
sudo chmod 666 /dev/ttyACM*
```