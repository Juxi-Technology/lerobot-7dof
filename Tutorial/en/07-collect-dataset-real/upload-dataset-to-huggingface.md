English | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>Upload Dataset to HuggingFace (Optional)</title>

# Method 1: Upload Locally (Not Recommended; Slow Upload Speed)

- Automatic upload

Set `push_to_hub=true` when collecting the dataset, and it uploads automatically once collection finishes

![This image shows a command line interface, part of the run log for the dataset upload process. At the top it shows environment information for tools such as SVN and treet W2; in the middle there is a processing message like "Starting the second pass: moving the mov atom to the beginning of the file", and below there are errors from the run such as "error messaging the mach port for IMCRunLoopWakeUpReliable", while on the right it lists processing progress and data transfer figures, such as the data amount and speed for different entries. Overall it presents a record of the running state during dataset upload processing.](../../en/images/d28-01.png)

- Manual upload

Set `push_to_hub=false` when collecting the dataset, and upload manually once collection finishes

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Whether automatic or manual, the upload speed is very slow (about 100 KB per second)

because the HuggingFace servers are overseas

# Method 2: Upload from a Cloud GPU Platform (Recommended)

## Log In to the Cloud GPU Platform Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Start a Cloud GPU Instance

## Upload the Dataset Archive to `Datasets`

## Copy the Instance Download Command

![The image shows the datasets page of the Featurize platform. At the top it shows the "Datasets" title, and below is a dataset named "soarm_amazing_hand_pick.zip", 213.3 MB in size, uploaded 16 hours ago. On the right there is a "Cloud Unzip" button, along with buttons such as "Like", "Comment" and "Copy Instance Download Command". This image relates to the step "Upload the dataset archive to `Datasets`", showing the datasets page after uploading.](../../en/images/d28-02.png)

## Run in the Command Line of the Cloud GPU Instance

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Upload the Dataset to HuggingFace

Create an `upload_dataset.py` file with the following content

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

Run the file

```Shell
python upload_dataset.py
```

![This image shows the process of running the dataset upload in the command line of a cloud GPU instance, where a user named lerobot2 ran the python upload.py command. It shows the processing progress for the files, with 6 files to process and all of them at 100% progress, and marks each file's transfer size, with total data transfer progress at 100%. At the bottom it notes that no files have been modified since the last commit, so the commit is skipped to prevent creating an empty commit. This content corresponds to the step of running the upload_dataset.py file.](../../en/images/d28-03.png)

- Another upload method (not recommended)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![This image shows the process of uploading a dataset using HuggingFace's `hf upload` command, with the command `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. The image shows that the upload has entered its final stage, with all files at 100% progress, including several video files and parquet files whose upload sizes exactly match the corresponding local file sizes, along with the total uploaded file size and transfer speed, and at the bottom a link to the HuggingFace dataset page for this upload commit, indicating that the upload task is complete.](../../en/images/d28-04.png)

# View the Dataset on HuggingFace

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![This image is a screenshot of the soarm_amazing_hand_pick dataset details page of the Juxi-Technology team on the Hugging Face platform, corresponding to the "View the Dataset on HuggingFace" content. At the top it shows navigation options for the dataset and basic information such as the author and tags; in the middle the Dataset Viewer area shows part of the training data for split 1 of the dataset, including fields such as action, observation_state and timestamp and their values, and also marks the size of a single record, the total number of records and the total size. At the bottom it also mentions related models trained on this data.](../../en/images/d28-05.png)

![The image shows the soarm_amazing_hand_pick dataset page on the Hugging Face platform. At the top there is a search box and navigation bar for searching models, datasets and so on. The dataset information section shows the owning organization Juxi - Technology and tags such as robotics and imitation-learning. Under the "Files and versions" tab it lists folders such as data, meta and videos and the README.md file, showing the uploader, upload method and time, such as "Upload README.md with huggingface_hub". This image relates to viewing a HuggingFace dataset, visually presenting the files and versions of the dataset.](../../en/images/d28-06.png)