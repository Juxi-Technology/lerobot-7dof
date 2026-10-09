[English](../en/10-inference/inference-rdk-s100.md) | 简体中文 | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# 地瓜机器人 RDK S100推理

具体实现流程可以参考这个链接<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy 全流程文档" type="doc"></cite>



## 在 RDK S100/S100P 上进行 ACT 模型端到端部署

本节将带你完成 ACT 模型在地瓜机器人 RDK S100 系列硬件的完整部署闭环。整个过程分为三个核心阶段：**模型导出**、**量化编译** 和 **板端运行**。

<callout emoji="💡">
**前置说明：**
- **开发机 (Host)：**用于执行步骤 1 和步骤 2，通常为你的模型训练机（需具备较好性能并安装 Docker）。
- **板端 (Edge)：**地瓜机器人 RDK S100/S100P，用于执行步骤 3。
- **工具链：**本文依赖 `rdk_LeRobot_tools` 仓库，详情可参考 [GitHub 仓库地址](https://github.com/D-Robotics/rdk_LeRobot_tools)。
</callout>

<callout emoji="🚨">
**版本兼容性重要提示 (必读)：** 当前版本的 `rdk_LeRobot_tools` ONNX 导出流程完美兼容 **LeRobot datasets v2.1** 版本。由于最新的 v3.0 版本存在数据结构修改，**强烈建议**在进行本章节操作前，将原始的 `lerobot` 主仓库切换至兼容 v2.1 的特定 commit，以确保导出流程顺畅。 
*推荐使用的 Commit ID：* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### 阶段一：模型 ONNX 格式导出 💻 (在开发机进行)

首先，我们需要将 **PyTorch 训练好的** 模型导出为中间格式（ONNX）。



#### **1. 拉取工具链仓库** 

进入你的 `lerobot` 工作目录，克隆 RDK 专属工具链：

```Bash
cd lerobot

# 1. 切换到兼容 v2.1 datasets 的稳定版本
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. 拉取地瓜机器人 RDK 专属工具链
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. 配置导出参数** 

编辑 `rdk_LeRobot_tools/bpu_export_config.yaml` 文件，根据你的实际路径修改配置：

```YAML
dataset:
  root: "data/so101_pick_place" # 你的数据集绝对或相对地址
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # 原始 PyTorch 模型权重地址
type: "nash-e" # 目标硬件架构，RDK S100 对应 nash-e / S100P 对应 nash-m
```



#### 3. 执行导出脚本

```Bash
# 导出 ONNX (开发机)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **成功标志**：当前目录下生成 `bpu_export_output` 文件夹，内部包含后续所需的 `build_all.sh` 脚本和量化校准数据。



### 阶段二：编译 BPU 模型 🐳 (在开发机 Docker 环境进行)

地瓜机器人的 BPU 模型量化与编译需要依赖 OpenExplorer (OE) 环境。我们推荐使用 Docker 来隔离环境。



#### **1.** **准备 Docker 环境与镜像** 

确保开发机已安装 Docker（[官方安装指南](https://docs.docker.com/engine/install/)）。下载推荐的 CPU 镜像并加载：

```Bash
# 加载下载好的离线镜像压缩包
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. 启动编译容器**

<callout emoji="⚠️">
**避坑指南**：编译模型需要较大的共享内存。请务必添加 `--shm-size=15g` 参数，否则极易引发 IPC 内存报错。
</callout>

将开发机的工作目录（包含刚才导出的文件夹）挂载到容器内：

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(注：请将 `<docker-image-name>` 替换为你通过 `sudo docker images` 查看到的实际镜像名。)



#### **3.** **容器内执行编译** 

进入容器内部后，执行一键编译脚本：

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **检查编译产物** 

编译完成后，会在 `bpu_export_output` 下生成 `bpu_output/` 文件夹。这里面包含了 RDK 板端运行所需的全部核心文件： 

- 点击查看 `bpu_output/` 目录结构

  - `BPU_ACTPolicy_TransformerLayers.hbm` (量化后的模型文件)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (量化后的模型文件)
  - `action_mean.npy` 等若干数据集归一化参数
  - `camera1_mean.npy` 等相机统计参数

---

### 阶段三：板端部署与推理 🤖 (在 RDK S100 上进行)

<callout emoji="📌">
**前置条件检查：**
1. RDK 板端已配置好 `D-Robotics/lerobot` 运行环境，并安装 `hbm_runtime`。
2. 已通过 `scp`、U盘等方式，将上一步生成的整个 `bpu_output/` 文件夹完整拷贝至 RDK 板端。
3. 已完成基础的遥操作配置，确保机械臂串口、相机 USB 端口及标定文件配置无误。
</callout>



#### **1.** **运行 BPU 加速推理**

在 RDK 板端终端，进入工具链目录并启动控制脚本：

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ 常见故障排查 (Troubleshooting)

在实际部署中如果遇到问题，请对照以下清单排查：

- **机械臂没有动作？**

  - 检查设备挂载情况：终端输入 `ls /dev/ttyACM*`，确认机械臂对应的串口号是否正确。
  - 检查权限：尝试使用 `sudo` 运行推理脚本，或将当前用户加入 `dialout` 用户组。
- **相机拉流报错 / 画面异常 / 机械臂原地抖动？**

  - 确认相机索引号（Camera Index）是否因热插拔发生了漂移，检查代码中的相机参数配置是否与实际 `/dev/video*` 对应。
- **开发机复制容器生成的文件时提示“权限不够”？**

  - Docker 挂载目录产生的文件归属默认为 root，在开发机执行 `sudo chown -R $USER:$USER bpu_export_output` 即可修复。