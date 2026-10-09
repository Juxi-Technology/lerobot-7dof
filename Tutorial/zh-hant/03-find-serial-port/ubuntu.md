[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | 繁體中文 | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# 方法一：Linux命令列直接檢視

## 檢視序列埠裝置連接埠

```Shell
ls /dev/ttyACM*
```

## 連接電腦和機械手臂的USB埠

先插上Follower從動臂，再繼續插上Leader主動臂

![圖片展示了在Ubuntu系統下使用Linux命令列檢視序列埠裝置連接埠的操作過程。先是顯示「什麼都沒插」，接著先插上Follower從動臂，出現「/dev/ttyACM0」；再插上Leader主動臂，又出現「/dev/ttyACM1」。圖片與上下文緊密相關，直觀呈現了連接電腦和機械手臂USB埠後，透過命令列檢視序列埠裝置連接埠的變化情況，輔助說明了如何透過Linux命令列檢視序列埠裝置連接埠。](../../en/images/d08-01.png)

# 方法二：Lerobot官方工具

```Shell
lerobot-find-port
```

![圖片展示的是在Ubuntu系統下使用Lerobot官方工具檢視序列埠裝置連接埠的命令列操作介面。命令為「lerobot-find-port」，顯示了所有可用連接埠，包括多個「/dev/ttyACM」系列連接埠。操作提示拔掉Follower的USB線，找到其連接埠號「/dev/ttyACM0」，最後重新插上USB線。該圖片與文件中介紹檢視序列埠裝置連接埠的方法二相關，直觀呈現了操作步驟及結果。](../../en/images/d08-02.png)

![圖片展示的是在Ubuntu系統下使用`lerobot-find-port`命令檢視機械手臂序列埠裝置連接埠的過程。命令執行後列出所有可用連接埠，接著提示拔掉Leader的USB線，最後顯示`/dev/ttyACM1`為Leader主動臂的序列埠裝置連接埠號。該圖片與文件中介紹檢視序列埠裝置連接埠的方法二相關，直觀呈現了透過Lerobot官方工具取得連接埠號的操作結果。](../../en/images/d08-03.png)

# 記錄我的連接埠

`/dev/ttyACM0`為Follower從動臂的序列埠裝置連接埠號

`/dev/ttyACM1`為Leader主動臂的序列埠裝置連接埠號

# 為連接埠賦予權限

讓所有使用者都有權限讀寫這些序列埠裝置

```Shell
sudo chmod 666 /dev/ttyACM*
```
