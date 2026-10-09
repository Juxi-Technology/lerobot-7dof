[English](../en/09-train-model/upload-model-to-huggingface.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# 上传模型到HuggingFace（可选）

## 创建模型Repo

<grid>
<column width-ratio="0.354197">
![图片展示了Hugging Face平台的用户界面。画面中有一个头像图标，点击后弹出下拉菜单，其中“New Model”选项被红色框线突出显示。该图片与文档中“上传模型到HuggingFace（可选）”部分内容相关，对应“创建模型Repo”步骤，直观呈现了在Hugging Face平台创建新模型的操作入口，帮助用户了解如何在平台创建模型相关资源。](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![图片展示的是Hugging Face网站上创建新模型仓库的界面。界面中“Owner”下拉菜单已选择“TommyZihao”，“Model name”输入框为“lerobot_zihao_model_a”，““License”输入框内为“mit”。下方有“Base template”选项，以及“Public”和“Private”仓库类型选择。该图片与文档中“创建模型Repo”部分相关，是创建模型仓库时填写相关信息的示例界面。](../../en/images/d46-02.png)
</column>
</grid>

## 查看模型Repo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

现在是空的

![图片展示的是Hugging Face平台上的“TommyZihao/lerobot_zihao_model_a”模型页面。页面左侧有“Model card”选项卡，可编辑模型卡片。右侧“Getting started with your model”部分介绍如何开始使用模型，包括添加完整模型信息、推送模型文件等。下方“Edit Model Card”区域可添加模型的License、语言、基础模型等信息。页面底部有“Push your model files”区域，提供CLI、Python、Git、HTTPS、SSH等多种上传模型文件的方式。该图片与文档中上传模型到HuggingFace的内容相关，展示了模型页面的操作界面。](../../en/images/d46-03.png)

![图片展示的是Hugging Face平台上的TommyZihao/lerobot_zihao_model_a模型Repo页面。页面显示该模型文件大小为1.54 KB，有1个贡献者，历史记录为1次提交，提交时间为9分钟前。页面还列出了.gitattributes和README.md文件，它们的大小分别为1.52 KB和24 Bytes，同样为初始提交，提交时间也是9分钟前。该图片与文档中上传模型到HuggingFace的内容相关，展示了模型上传后的页面情况。](../../en/images/d46-04.png)

## 上传模型

创建`upload_model.py`文件，内容如下

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

运行

```Shell
python upload_model.py
```

![图片展示了在命令行中运行`python upload_model.py`命令的输出结果。显示文件处理进度为34%，新数据上传进度同样为34%，并列出了`d_model/model.safetensors`和`tokenizer_processor.safetensors`两个文件的上传进度，分别为92%。该图片与文档中上传模型到HuggingFace的内容相关，直观呈现了上传模型文件时的进度情况。](../../en/images/d46-05.png)

## 查看模型Repo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![图片展示的是TommyZihao的lerobot_zihao_model_a模型在HuggingFace的Repo页面。页面显示该模型的License为mit，有1个贡献者，历史记录2次提交。页面中部列出了多个文件，如README.md、config.json等、model.safetensors等，每个文件右侧均有“Upload folder using huggingface_hub”字样，表明这些文件是通过huggingface_hub上传的。该图片与文档中上传模型到HuggingFace的内容相关，直观呈现了模型文件在HuggingFace中的存储情况。](../../en/images/d46-06.png)

现在有了模型文件