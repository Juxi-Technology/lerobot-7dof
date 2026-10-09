[English](../en/09-train-model/wandb-training-curves.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/wandb-training-curves.md) | [Deutsch](../de/09-train-model/wandb-training-curves.md) | [Español](../es/09-train-model/wandb-training-curves.md) | [Français](../fr/09-train-model/wandb-training-curves.md) | [Italiano](../it/09-train-model/wandb-training-curves.md) | [日本語](../ja/09-train-model/wandb-training-curves.md) | [한국어](../ko/09-train-model/wandb-training-curves.md) | [Português (BR)](../pt-br/09-train-model/wandb-training-curves.md) | [Português (PT)](../pt-pt/09-train-model/wandb-training-curves.md)

# wandb查看实时训练曲线

- 获取wandb链接

![图片展示的是在浏览器中打开的Lerobot训练界面。界面中显示了Lerobot - dataset repo_id = x的运行日志，其中“one will be synced with wandb”及“Creating dataset”等关键信息被红色框突出显示。该图片与文档中“查看训练实时曲线”部分相关，用于说明在Lerobot训练过程中，可通过浏览器查看训练实时曲线，此界面即为查看训练曲线时所见的运行日志界面。](../../en/images/d44-01.png)

- 查看训练实时曲线

![图片展示的是在wandb上查看实时训练曲线的界面。界面上方有“Charts”“Overview”“Logs”“Files”等选项卡，当前选中“Charts”。下方有多个图表，包括“train/update_x”“train/loss”“train/samples”“train/ir”“train/loss”“train/TL_loss”等，各图表以线条图形式呈现训练过程中的相关数据变化，横轴为Step，纵轴为不同指标。该图与上文“获取wandb链接”及“查看训练实时曲线”内容相关，直观呈现了训练曲线情况。](../../en/images/d44-02.png)