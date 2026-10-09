[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | Português (BR) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Registrar uma Conta no Hugging Face (Opcional)

## Configurar um Espelho do HuggingFace Baseado na China

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Adicione isto no final do arquivo
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

# Adicione isto no final do arquivo
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Saída
# https://hf-mirror.com
```



## Criar um Token

https://huggingface.co/settings/tokens

![A imagem mostra a página "Settings / Tokens" do site do Hugging Face. À esquerda estão o avatar e as informações de perfil do usuário; à direita está a área "User Access Tokens", que explica que os tokens de acesso são usados para autenticação e permitem que os aplicativos executem operações com base nas permissões do token, destacando que ele não deve ser compartilhado com outras pessoas. No canto superior direito há um botão "+ Create new token" para criar um novo token. Esta imagem está relacionada à seção "Criar um Token" do documento, apresentando visualmente o ponto de entrada para criar um token.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![Esta imagem mostra a página de criação de uma nova credencial de acesso (Token) na plataforma Hugging Face. Na página, a área "Token type" destaca a opção "Write", o campo de entrada "Token name" já está preenchido com "lerobot_zihao_token", e na parte inferior há um botão "Create token" para enviar a criação. O lado esquerdo da página mostra as informações da conta do usuário conectado no momento.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![A imagem mostra a janela "Save your Access Token" no site do Hugging Face. A janela lembra o usuário de guardar o valor do token em um lugar seguro, pois ele não poderá mais ser visualizado depois que for fechada. Abaixo, mostra o valor do token "<your-huggingface-token>", com um botão "Copy" para copiar o token. Também mostra o nome do token "lerobot_zihao_token" e a permissão "WRITE". Esta imagem está relacionada à etapa "Criar um Token" do documento e serve para orientar o usuário a registrar o Token gerado.](../../en/images/d24-03.png)
</column>
</grid>

## Registrar o Token

Por exemplo, o meu é:

```Shell
<your-huggingface-token>
```

## Vincular o Token

```Shell
hf auth login

hf auth whoami
```

![A imagem mostra a operação de uma conta do Hugging Face pela linha de comando no sistema Mac. Primeiro é executado o comando "hf auth login", que avisa que já existe um token e que você pode usar "hf auth whoami" para obter informações ou "hf auth logout" para sair. Em seguida, o token é inserido; após a confirmação, mostra que o token foi salvo no caminho especificado e que o token ativo no momento é "lerobot zihao token". Por fim, o comando "hf auth whoami" é executado, mostrando o usuário como "TommyZihao". Esta imagem corresponde à seção "Criar um Token" do documento, apresentando visualmente a operação de criação do token e seu resultado.](../../en/images/d24-04.png)

## Criar um Repositório de Dataset

<grid>
<column width-ratio="0.480604">
![Esta é uma captura de tela da plataforma Hugging Face. No canto superior esquerdo há entradas como Community, Docs e Pricing; as informações da conta pessoal TommyZihao estão na área central, e uma caixa verde marca o ícone do avatar do usuário no canto superior direito. No menu central, uma caixa vermelha destaca a opção "+ New Dataset", que corresponde à etapa "Criar um Repositório de Dataset" no contexto — clicar nela inicia a criação de um repositório de conjunto de dados.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![A imagem mostra a interface de criação de um novo repositório de conjunto de dados no Hugging Face. O campo "Dataset name" está preenchido com "lerobot_zihao_dataset_a", "License" está definida como "mit" e a opção "Public" está marcada. Na parte inferior há um botão "Create dataset". Esta imagem corresponde à seção "Criar um Repositório de Dataset" do documento, apresentando visualmente as informações-chave a preencher ao criar um repositório de conjunto de dados e ajudando os usuários a entender a interface e os campos necessários.](../../en/images/d24-06.png)
</column>
</grid>

![Esta é a interface web da plataforma Hugging Face, mostrando o conteúdo de um repositório de Dataset chamado lexobot_zihao_dataset_a criado pelo usuário TommyZhao, no modo de tema Dark. À esquerda há a opção "Edit Dataset Card", e também são oferecidas várias formas de enviar os arquivos do conjunto de dados, com exemplos de comandos para CLI, Python, HTTPS e SSH, além de indicar que a License do conjunto de dados é mit.](../../en/images/d24-07.png)
