English | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# Uploading a Model to HuggingFace (Optional)

## Create a Model Repo

<grid>
<column width-ratio="0.354197">
![This image shows the HuggingFace user interface. It shows an avatar icon; clicking it opens a drop-down menu in which the "New Model" option is highlighted with a red box. The image relates to the "Uploading a Model to HuggingFace (Optional)" section and corresponds to the "Create a Model Repo" step, visually presenting the entry point for creating a new model on HuggingFace and helping the user understand how to create model-related resources on the platform.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![This image shows the interface for creating a new model repository on the HuggingFace website. The "Owner" drop-down is set to "TommyZihao", the "Model name" field contains "lerobot_zihao_model_a" and the "License" field contains "mit". Below are a "Base template" option and the "Public" and "Private" repository type choices. The image relates to the "Create a Model Repo" section and is an example of filling in the details when creating a model repository.](../../en/images/d46-02.png)
</column>
</grid>

## View the Model Repo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

It is empty for now

![This image shows the "TommyZihao/lerobot_zihao_model_a" model page on the HuggingFace platform. On the left is a "Model card" tab for editing the model card. On the right, the "Getting started with your model" section explains how to start using the model, including adding complete model information and pushing model files. Below it, the "Edit Model Card" area lets you add the model's License, language, base model and other information. At the bottom, the "Push your model files" area offers several ways to upload model files, including CLI, Python, Git, HTTPS and SSH. The image relates to uploading a model to HuggingFace, showing the page's controls.](../../en/images/d46-03.png)

![This image shows the TommyZihao/lerobot_zihao_model_a model repo page on the HuggingFace platform. The page shows the model's file size as 1.54 KB, one contributor and a history of 1 commit, made 9 minutes ago. It also lists the .gitattributes and README.md files, sized 1.52 KB and 24 Bytes respectively, likewise from the initial commit, also 9 minutes ago. The image relates to uploading a model to HuggingFace, showing what the page looks like after the model is uploaded.](../../en/images/d46-04.png)

## Upload the Model

Create an `upload_model.py` file with the following content

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

Run it

```Shell
python upload_model.py
```

![This image shows the output of running the `python upload_model.py` command from the command line. It shows the file processing progress at 34% and the new-data upload progress also at 34%, and lists the upload progress for the two files `d_model/model.safetensors` and `tokenizer_processor.safetensors` at 92% each. The image relates to uploading a model to HuggingFace, visually showing the progress when uploading model files.](../../en/images/d46-05.png)

## View the Model Repo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![This image shows the HuggingFace repo page for TommyZihao's lerobot_zihao_model_a model. The page shows the model's License as mit, one contributor and a history of 2 commits. In the middle it lists several files, such as README.md, config.json and model.safetensors, each with the text "Upload folder using huggingface_hub" on its right, indicating that these files were uploaded via huggingface_hub. The image relates to uploading a model to HuggingFace, visually showing how the model files are stored on HuggingFace.](../../en/images/d46-06.png)

Now the model files are in place
