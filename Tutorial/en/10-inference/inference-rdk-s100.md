English | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# D-Robotics RDK S100 Inference

For the detailed implementation flow, refer to this link<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## End-to-End ACT Model Deployment on RDK S100/S100P

This section walks you through the complete deployment loop for the ACT model on D-Robotics RDK S100 series hardware. The whole process has three core stages: **model export**, **quantisation compilation** and **on-board execution**.

<callout emoji="💡">
**Prerequisites:**
- **Development machine (Host):** used to run steps 1 and 2, usually your model training machine (it needs decent performance and Docker installed).
- **Board (Edge):** the D-Robotics RDK S100/S100P, used to run step 3.
- **Toolchain:** this article relies on the `rdk_LeRobot_tools` repository; see the [GitHub repository](https://github.com/D-Robotics/rdk_LeRobot_tools) for details.
</callout>

<callout emoji="🚨">
**Important version-compatibility note (must read):** the current `rdk_LeRobot_tools` ONNX export flow is fully compatible with **LeRobot datasets v2.1**. Because the latest v3.0 changes the data structure, it is **strongly recommended** that, before doing the work in this section, you switch the main `lerobot` repository to the specific commit that is compatible with v2.1, so that the export flow runs smoothly. 
*Recommended Commit ID:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Stage 1: Export the Model to ONNX Format 💻 (on the development machine)

First, we need to export the **PyTorch-trained** model to an intermediate format (ONNX).



#### **1. Clone the Toolchain Repository** 

Go to your `lerobot` working directory and clone the RDK-specific toolchain:

```Bash
cd lerobot

# 1. Switch to the stable version compatible with v2.1 datasets
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Clone the D-Robotics RDK-specific toolchain
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Configure the Export Parameters** 

Edit the `rdk_LeRobot_tools/bpu_export_config.yaml` file and adjust the configuration to match your actual paths:

```YAML
dataset:
  root: "data/so101_pick_place" # absolute or relative path to your dataset
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # path to the original PyTorch model weights
type: "nash-e" # target hardware architecture; RDK S100 corresponds to nash-e / S100P corresponds to nash-m
```



#### 3. Run the Export Script

```Bash
# Export ONNX (development machine)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Success indicator**: a `bpu_export_output` folder is created in the current directory, containing the `build_all.sh` script and the quantisation calibration data needed later.



### Stage 2: Compile the BPU Model 🐳 (in a Docker environment on the development machine)

Quantising and compiling D-Robotics BPU models requires an OpenExplorer (OE) environment. We recommend using Docker to isolate the environment.



#### **1.** **Prepare the Docker Environment and Image** 

Make sure Docker is installed on the development machine ([official installation guide](https://docs.docker.com/engine/install/)). Download the recommended CPU image and load it:

```Bash
# Load the downloaded offline image archive
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Start the Compilation Container**

<callout emoji="⚠️">
**Pitfall warning**: compiling the model needs a large amount of shared memory. Be sure to add the `--shm-size=15g` argument, otherwise IPC memory errors are very likely.
</callout>

Mount the development machine's working directory (containing the folder you just exported) into the container:

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Note: replace `<docker-image-name>` with the actual image name you see via `sudo docker images`.)



#### **3.** **Run the Compilation Inside the Container** 

Once inside the container, run the one-click compilation script:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Check the Build Artefacts** 

After compilation finishes, a `bpu_output/` folder is created under `bpu_export_output`. It contains all the core files needed to run on the RDK board: 

- Click to view the `bpu_output/` directory structure

  - `BPU_ACTPolicy_TransformerLayers.hbm` (quantised model file)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (quantised model file)
  - `action_mean.npy` and several other dataset normalisation parameters
  - `camera1_mean.npy` and other camera statistics parameters

---

### Stage 3: On-Board Deployment and Inference 🤖 (on the RDK S100)

<callout emoji="📌">
**Prerequisites check:**
1. The RDK board already has the `D-Robotics/lerobot` runtime environment configured, with `hbm_runtime` installed.
2. The whole `bpu_output/` folder generated in the previous step has been fully copied to the RDK board, via `scp`, a USB drive or similar.
3. Basic teleoperation configuration is already done, ensuring the arm's serial port, the camera's USB port and the calibration file are configured correctly.
</callout>



#### **1.** **Run BPU-Accelerated Inference**

In the terminal on the RDK board, go to the toolchain directory and start the control script:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Troubleshooting

If you hit problems during an actual deployment, check against the following list:

- **The arm does not move?**

  - Check that the device is mounted: type `ls /dev/ttyACM*` in the terminal and confirm that the serial port for the arm is correct.
  - Check permissions: try running the inference script with `sudo`, or add the current user to the `dialout` group.
- **Camera streaming error / abnormal image / the arm shakes in place?**

  - Confirm whether the camera index drifted because of a hot-plug, and check that the camera parameters in the code match the actual `/dev/video*`.
- **Copying files generated by the container on the development machine reports "insufficient permissions"?**

  - Files created in a Docker-mounted directory are owned by root by default; run `sudo chown -R $USER:$USER bpu_export_output` on the development machine to fix it.
