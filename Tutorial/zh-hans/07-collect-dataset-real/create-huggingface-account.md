[English](../en/07-collect-dataset-real/create-huggingface-account.md) | 简体中文 | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# 注册Hugging Face账号（可选）

## 设置HuggingFace国内镜像

- Ubuntu

```Shell
sudo nano ~/.bashrc

# 在文件末尾加入
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# 输出
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# 在文件末尾加入
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# 输出
# https://hf-mirror.com
```



## 创建Token

https://huggingface.co/settings/tokens

![图片展示的是Hugging Face网站中“设置令牌”页面。页面左侧有用户头像及个人信息，右侧是“用户访问令牌”区域，说明访问令牌用于身份验证并允许应用程序基于令牌权限执行操作，强调不与他人分享令牌。页面右上角有“+ Create new token”按钮，用于创建新令牌。该图片与文档中“创建Token”部分内容相关，直观呈现了创建Token的操作入口。](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![这张图片展示了Hugging Face平台中创建新访问凭证（Token）的页面。页面中“Token type”区域突出显示了“Write”选项，同时“Token name”输入框中已填写内容“lerobot_zihao_token”，页面底部还有用于提交创建操作的“Create token”按钮，页面左侧显示当前登录用户的账号信息。](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![图片展示的是Hugging Face网站上“Save your Access Token”弹窗界面。弹窗提示用户将token值保存在安全的地方，关闭后无法再次查看。下方显示token值“<your-huggingface-token>”，并有“Copy”按钮用于复制token。下方还有token名称“lerobot_zihao_token”及权限“WRITE”。该图片与文档中“创建Token”步骤相关，用于指导用户记录生成的Token。](../../en/images/d24-03.png)
</column>
</grid>

## 记录Token

例如，我的是：

```Shell
<your-huggingface-token>
```

## 绑定Token

```Shell
hf auth login

hf auth whoami
```

![图片展示了在Mac系统中使用命令行操作Hugging Face账号的界面。首先执行“hf auth login”命令，提示已有token，可使用“hf auth whoami”获取信息或“hf auth logout”登出。接着输入token，确认后显示token已保存至指定路径，且当前活跃token为“lerobot zihao token”，最后执行“hf auth whoami”命令，显示用户为“TommyZihao”。该图片与文档中“创建Token”部分对应，直观呈现了创建Token的操作及结果。](../../en/images/d24-04.png)

## 创建Dataset Repo

<grid>
<column width-ratio="0.480604">
![这是Hugging Face平台的界面截图，页面左上角有Community、Docs、Pricing等功能入口，个人账号TommyZihao的信息在界面中间区域，被绿色框标注的是页面右上角的用户头像图标。界面中间菜单中，用红色框突出标注了“+ New Dataset”选项，该选项对应上下文里“创建Dataset Repo”的操作步骤，点击此选项即可开始创建数据集代码库。](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![图片展示的是在Hugging Face创建新数据集仓库的界面。界面中“Dataset name”处输入了“lerobot_zihao_dataset_a”，“License”选择为“mit”，“Public”选项被勾选。底部有“Create dataset”按钮。该图片与文档中“创建Dataset Repo”部分内容对应，直观呈现了创建数据集仓库时需填写的关键信息，帮助用户了解在Hugging Face上创建数据集仓库的操作界面及所需填写的字段。](../../en/images/d24-06.png)
</column>
</grid>

![这是Hugging Face平台的网页界面，页面展示了用户TommyZhao创建的名为lexobot_zihao_dataset_a的Dataset仓库内容，处于Dark主题模式下。页面左侧有“Edit Dataset Card”的编辑选项，还提供了推送数据集文件的多种操作方式，包含CLI、Python、HTTPS、SSH的相关命令示例，同时标注了该数据集的License为mit。](../../en/images/d24-07.png)
