[English](../en/09-train-model/train-diffusion.md) | [简体中文](../zh-hans/09-train-model/train-diffusion.md) | 繁體中文 | [Deutsch](../de/09-train-model/train-diffusion.md) | [Español](../es/09-train-model/train-diffusion.md) | [Français](../fr/09-train-model/train-diffusion.md) | [Italiano](../it/09-train-model/train-diffusion.md) | [日本語](../ja/09-train-model/train-diffusion.md) | [한국어](../ko/09-train-model/train-diffusion.md) | [Português (BR)](../pt-br/09-train-model/train-diffusion.md) | [Português (PT)](../pt-pt/09-train-model/train-diffusion.md)

# 訓練命令列-Diffusion

## 參考文件

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/policy_diffusion_README.md

## 命令列

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.streaming=false \
  --policy.type=diffusion \
  --output_dir=output_lerobot_train/shake/diffusion_a \
  --job_name=shake_diffusion_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=30000 \
  --batch_size=8
```

Diffusion模型壓縮檔大概1個G左右

## 效果一般：機械臂一下一下卡頓，不推薦

<grid><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: wx_camera_1768547494388.mp4](../../en/images/wx_camera_1768547494388.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: wx_camera_1768551529764.mp4](../../en/images/wx_camera_1768551529764.mp4)</figure></column></grid>
