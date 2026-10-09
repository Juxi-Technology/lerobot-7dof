[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | 简体中文 | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>上传数据集到HuggingFace（可选）</title>

# 方法一：本地上传（不推荐，上传网速慢）

- 自动上传

在采集数据集时设置`push_to_hub=true`，采集完毕后自动上传

![这张图片展示了命令行运行界面，属于数据集上传相关流程的运行日志内容，界面上方显示了SVN、treet W2等工具及环境相关信息，中间标注了“Starting the second pass: moving the mov atom to the beginning of the file”这样的处理提示，下方记录了运行过程中的错误提示，如“error messaging the mach port for IMCRunLoopWakeUpReliable”，同时右侧列出了处理进度和数据传输的相关数值，比如不同条目对应的数据量、速度等信息，整体呈现出数据集上传处理过程中的运行状态记录。](../../en/images/d28-01.png)

- 手动上传

在采集数据集时设置`push_to_hub=false`，采集完毕后手动上传

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



无论自动上传还是手动上传，上传速度都很慢（每秒钟一百KB）

因为HuggingFace服务器在国外

# 方法二：云GPU平台上传（推荐）

## 登录云GPU平台Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## 开启一个云GPU实例

## 上传数据集压缩包到`数据集`

## 复制实例下载命令

![图片展示的是Featurize平台中数据集页面。页面上方显示“数据集”标题，下方是名为“soarm_amazing_hand_pick.zip”的数据集，其大小为213.3 MB，上传时间为16小时前。页面右侧有“云解压”按钮，以及“喜欢”“评论”“复制实例下载命令”等操作按钮。该图片与文档中“上传数据集压缩包到`数据集`”的操作步骤相关，展示了上传后的数据集页面情况。](../../en/images/d28-02.png)

## 在云GPU实例的命令行中运行

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## 上传数据集到HuggingFace

创建`upload_dataset.py`文件，内容如下

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

运行文件

```Shell
python upload_dataset.py
```

![这张图片展示了在云GPU实例命令行中运行上传数据集操作的过程，显示名为lerobot2的用户执行了python upload.py命令。界面中呈现了处理文件的进度信息，共需处理6个文件，所有文件的处理进度均达到100%，还标注了各文件的传输大小，总数据传输进度为100%。底部提示没有文件自上次提交以来被修改，因此跳过提交以防止创建空提交，该内容对应文档中运行upload_dataset.py文件的步骤场景。](../../en/images/d28-03.png)

- 另一种上传方法（不推荐）

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![这张图片展示了使用HuggingFace的`hf upload`命令上传数据集的过程，命令为`hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`。图片中显示上传已进入最终阶段，当前所有文件的处理进度均为100%，其中包含多个视频格式文件与parquet格式文件，各自的上传体积也与对应本地文件大小完全匹配，同步显示了上传的文件总大小与传输速度，底部还附带了此次上传提交的HuggingFace数据集页面链接，体现了上传任务已完成的状态。](../../en/images/d28-04.png)

# 查看HuggingFace上的数据集

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![这张图片是Hugging Face平台上Juxi-Technology团队的soarm_amazing_hand_pick数据集详情页面截图，属于文档中查看HuggingFace上的数据集对应的内容。页面顶部显示了该数据集的相关导航选项，以及数据集的作者、标签等基础信息，中间的Dataset Viewer区域展示了该数据集split 1的训练数据部分内容，包含action、observation_state、timestamp等数据字段及对应数值，还标注了单条数据的大小、总数据条数和总大小等信息，页面下方还提及了基于该数据训练的相关模型。](../../en/images/d28-05.png)

![图片展示的是Hugging Face平台上的soarm_amazing_hand_pick数据集页面。页面上方有搜索框及导航栏，可搜索模型、数据集等。数据集信息部分显示所属组织为Juxi - Technology，包含标签如robotics、imitation-learning等。下方“Files and versions”标签下，列出data、meta、videos等文件夹及README.md文件，显示上传者、上传方式、时间等信息，如“Upload README.md with huggingface_hub”等。该图与文档中查看HuggingFace数据集的内容相关，直观呈现了数据集的文件及版本情况。](../../en/images/d28-06.png)