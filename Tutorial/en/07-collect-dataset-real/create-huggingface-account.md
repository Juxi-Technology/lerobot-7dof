English | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Register a Hugging Face Account (Optional)

## Set Up a China-Based HuggingFace Mirror

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Add this at the end of the file
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# Output
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# Add this at the end of the file
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Output
# https://hf-mirror.com
```



## Create a Token

https://huggingface.co/settings/tokens

![The image shows the "Settings / Tokens" page of the Hugging Face website. On the left is the user's avatar and profile information; on the right is the "User Access Tokens" area, which explains that access tokens are used for authentication and allow applications to perform operations based on the token's permissions, and stresses not to share the token with others. In the top-right corner there is a "+ Create new token" button for creating a new token. This image relates to the "Create a Token" section of the document, visually presenting the entry point for creating a token.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![This image shows the page for creating a new access credential (Token) on the Hugging Face platform. In the page, the "Token type" area highlights the "Write" option, the "Token name" input field is filled with "lerobot_zihao_token", and at the bottom there is a "Create token" button to submit the creation. The left side of the page shows the current logged-in user's account information.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![The image shows the "Save your Access Token" pop-up on the Hugging Face website. The pop-up reminds users to save the token value somewhere safe, because it cannot be viewed again after closing. Below it shows the token value "<your-huggingface-token>" with a "Copy" button to copy the token. It also shows the token name "lerobot_zihao_token" and the permission "WRITE". This image relates to the "Create a Token" step in the document and is used to guide users in recording the generated Token.](../../en/images/d24-03.png)
</column>
</grid>

## Record the Token

For example, mine is:

```Shell
<your-huggingface-token>
```

## Bind the Token

```Shell
hf auth login

hf auth whoami
```

![The image shows operating a Hugging Face account from the command line on macOS. The "hf auth login" command is run first, prompting that a token already exists and that you can use "hf auth whoami" to fetch information or "hf auth logout" to log out. Then the token is entered; after confirmation it shows the token has been saved to the specified path and the currently active token is "lerobot zihao token". Finally the "hf auth whoami" command is run, showing the user as "TommyZihao". This image corresponds to the "Create a Token" section of the document, visually presenting the token-creation operation and its result.](../../en/images/d24-04.png)

## Create a Dataset Repo

<grid>
<column width-ratio="0.480604">
![This is a screenshot of the Hugging Face platform. In the top-left corner are entries such as Community, Docs and Pricing; the personal account TommyZihao's information is in the middle area, and a green box marks the user-avatar icon in the top-right corner. In the middle menu, a red box highlights the "+ New Dataset" option, which corresponds to the "Create a Dataset Repo" step in the context — clicking it starts creating a dataset repository.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![The image shows the interface for creating a new dataset repository on Hugging Face. The "Dataset name" field is filled with "lerobot_zihao_dataset_a", "License" is set to "mit", and the "Public" option is checked. At the bottom there is a "Create dataset" button. This image corresponds to the "Create a Dataset Repo" section of the document, visually presenting the key information to fill in when creating a dataset repository and helping users understand the interface and the required fields.](../../en/images/d24-06.png)
</column>
</grid>

![This is the Hugging Face platform web interface, showing the contents of a Dataset repository named lexobot_zihao_dataset_a created by user TommyZhao, in Dark theme mode. On the left there is an "Edit Dataset Card" option, and it also provides several ways to push dataset files, with example commands for CLI, Python, HTTPS and SSH, and notes that the dataset's License is mit.](../../en/images/d24-07.png)
