English | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Mac Computer

## Check the Port

```Shell
ls /dev/tty.*
```

The result looks similar to the image below; either of the two ports will work

![The image shows the output in the Mac terminal after running the "ls /dev/tty.*" command, listing several ports. Two of them, "/dev/tty.usbmodem5AAF2194741" and "/dev/tty.wchusbserial5AAF2194741", are highlighted with red boxes. The context mentions checking ports, and the result is similar to this image; either of the two ports can be used. The image visually presents the port information to check, closely related to the "Check the Port" content above, and is a display of the result of the port-checking operation.](../../en/images/d09-01.png)

## Grant Permissions to the Port

Give all users permission to read and write these serial devices

```Shell
chmod 666 /dev/tty.*
```

## Record My Ports

Follower arm:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader arm:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Why Are There Two Ports on a Mac?

The servo controller board we use is **recognized by macOS as two different types of serial drivers at the same time**, so two ports are shown:

- One is the system's default generic serial driver (`/dev/tty.usbmodemxxxx`)
- The other is the dedicated serial driver provided by the chip vendor (for example, "wch" here refers to the CH340/CH341 chip from Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

This is normal — **the two ports actually correspond to the same hardware device**, and you can connect and communicate through either one (for example, just pick either port in the software that controls the robot arm).

If you run into an error when using one port later, try switching to the other one.