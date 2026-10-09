[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | Português (PT)

# Treino Local em Ubuntu

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Nota

`\` só pode ter um espaço antes de si e nenhum espaço depois

`--dataset.split` assume `train` por defeito, o que significa que todo o conjunto de dados é utilizado como conjunto de treino

O conjunto de dados é local, pelo que `--dataset.streaming` tem de ser `false`, uma vez que o conjunto de dados já está em disco e não é necessária leitura em streaming

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![Esta imagem mostra o comando de treino e a configuração para executar o script lerobot_train.py num ambiente Ubuntu local. O comando inclui parâmetros como o caminho do conjunto de dados, o id do repositório e o ramo, por exemplo `--dataset.repo_id` definido como Tommy/lerobot_zhao_dataset_a. Entre os valores de configuração, `--dataset.streaming` está definido como `false`, `--use_imagenet_stats` como `True`, `--batch_size` como 4 e `--val_n_episodes` como 1000. A imagem está estreitamente relacionada com o contexto, apresentando visualmente o comando de treino e os seus principais parâmetros de configuração.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![Esta imagem mostra o registo de treino produzido ao executar o script lerobot_train.py num ambiente Ubuntu local, centrado nos parâmetros de configuração do treino e no estado em direto da execução do treino. Assinala claramente informação de treino essencial: `--dataset.split` assume `train` por defeito, o que significa que todo o conjunto de dados é utilizado como conjunto de treino, e, como o conjunto de dados está armazenado localmente, o estado de `--dataset.streaming` também fica fixado. O registo abrange ainda o progresso do treino, o carregamento do conjunto de dados, a criação do otimizador e do scheduler do modelo, e os valores de loss e de step durante o treino, dando uma visão clara de um treino local em curso.](../../en/images/d34-02.png)
</column>
</grid>

![Esta imagem mostra o registo de treino produzido ao treinar um modelo com o LeRobot num ambiente Ubuntu. O registo regista informação da execução do treino, como a hora, o conjunto de treino, o modelo, a loss e a precisão. As marcas temporais vão das 15:11:53 às 16:15:16 de 14 de janeiro de 2024, a loss oscila entre 0.68 e 0.65, e a precisão (acc) entre 0.85 e 0.88. A imagem relaciona-se com o contexto de treino do modelo LeRobot, apresentando visualmente como as métricas principais variam durante o treino.](../../en/images/d34-03.png)
