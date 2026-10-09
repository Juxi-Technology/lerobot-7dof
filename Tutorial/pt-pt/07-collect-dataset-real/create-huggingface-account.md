[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | Português (PT)

# Registar uma conta Hugging Face (opcional)

## Configurar um espelho do HuggingFace na China

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Adicione isto no final do ficheiro
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# Saída
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# Adicione isto no final do ficheiro
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Saída
# https://hf-mirror.com
```



## Criar um token

https://huggingface.co/settings/tokens

![A imagem mostra a página "Settings / Tokens" do sítio do Hugging Face. Do lado esquerdo estão o avatar e a informação de perfil do utilizador; do lado direito está a área "User Access Tokens", que explica que os tokens de acesso servem para autenticação e permitem que as aplicações executem operações com base nas permissões do token, sublinhando que não se deve partilhar o token com outras pessoas. No canto superior direito há um botão "+ Create new token" para criar um novo token. Esta imagem está relacionada com a secção "Criar um token" do documento, apresentando visualmente o ponto de entrada para criar um token.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![Esta imagem mostra a página de criação de uma nova credencial de acesso (Token) na plataforma Hugging Face. Na página, a área "Token type" realça a opção "Write", o campo "Token name" está preenchido com "lerobot_zihao_token" e, na parte inferior, existe um botão "Create token" para submeter a criação. Do lado esquerdo da página aparece a informação da conta do utilizador com sessão iniciada.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![A imagem mostra a janela pop-up "Save your Access Token" do sítio do Hugging Face. O pop-up lembra o utilizador de guardar o valor do token num local seguro, porque depois de fechar já não o poderá ver novamente. Abaixo apresenta o valor do token "<your-huggingface-token>" e um botão "Copy" para copiar o token. Mostra ainda o nome do token "lerobot_zihao_token" e a permissão "WRITE". Esta imagem está relacionada com o passo "Criar um token" do documento e serve para orientar o utilizador a registar o Token gerado.](../../en/images/d24-03.png)
</column>
</grid>

## Registar o token

Por exemplo, o meu é:

```Shell
<your-huggingface-token>
```

## Associar o token

```Shell
hf auth login

hf auth whoami
```

![A imagem mostra a interface de operação de uma conta Hugging Face a partir da linha de comandos no sistema Mac. Primeiro executa-se o comando "hf auth login", que indica que já existe um token e que se pode usar "hf auth whoami" para obter informação ou "hf auth logout" para terminar a sessão. Em seguida introduz-se o token; após confirmação, mostra que o token foi guardado no caminho especificado e que o token atualmente ativo é "lerobot zihao token". Por fim executa-se o comando "hf auth whoami", que mostra o utilizador como "TommyZihao". Esta imagem corresponde à secção "Criar um token" do documento, apresentando visualmente a operação de criar o token e o seu resultado.](../../en/images/d24-04.png)

## Criar um repositório de conjunto de dados

<grid>
<column width-ratio="0.480604">
![Esta é uma captura de ecrã da plataforma Hugging Face. No canto superior esquerdo estão entradas como Community, Docs e Pricing; a informação da conta pessoal TommyZihao está na área central e um retângulo verde assinala o ícone do avatar do utilizador no canto superior direito. No menu central, um retângulo vermelho realça a opção "+ New Dataset", que corresponde ao passo "Criar um repositório de conjunto de dados" do contexto — clicar nesta opção inicia a criação do repositório de conjunto de dados.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![A imagem mostra a interface de criação de um novo repositório de conjunto de dados no Hugging Face. No campo "Dataset name" está introduzido "lerobot_zihao_dataset_a", a "License" está definida como "mit" e a opção "Public" está assinalada. Na parte inferior há um botão "Create dataset". Esta imagem corresponde à secção "Criar um repositório de conjunto de dados" do documento, apresentando visualmente a informação essencial a preencher ao criar um repositório de conjunto de dados e ajudando o utilizador a compreender a interface e os campos necessários.](../../en/images/d24-06.png)
</column>
</grid>

![Esta é a interface web da plataforma Hugging Face, mostrando o conteúdo de um repositório de Dataset chamado lexobot_zihao_dataset_a criado pelo utilizador TommyZhao, em modo de tema Dark. Do lado esquerdo há uma opção de edição "Edit Dataset Card" e são também apresentadas várias formas de enviar os ficheiros do conjunto de dados, com exemplos de comandos para CLI, Python, HTTPS e SSH, além de indicar que a License do conjunto de dados é mit.](../../en/images/d24-07.png)
