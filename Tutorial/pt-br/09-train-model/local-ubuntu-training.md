[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# Treinamento local no Ubuntu

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Observação

`\` pode ter apenas um espaço antes, e nenhum espaço depois

`--dataset.split` tem como padrão `train`, o que significa que todo o conjunto de dados é usado como conjunto de treinamento

O conjunto de dados é local, então `--dataset.streaming` precisa ser `false`, já que os dados já estão em disco e nenhuma leitura em streaming é necessária

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
![Esta imagem mostra o comando e a configuração de treinamento para executar o script lerobot_train.py em um ambiente Ubuntu local. O comando inclui parâmetros como o caminho do conjunto de dados, o ID do repositório e o branch, por exemplo `--dataset.repo_id` definido como Tommy/lerobot_zhao_dataset_a. Entre os valores de configuração, `--dataset.streaming` está definido como `false`, `--use_imagenet_stats` como `True`, `--batch_size` como 4 e `--val_n_episodes` como 1000. A imagem está intimamente relacionada ao contexto, apresentando visualmente o comando de treinamento e seus principais parâmetros de configuração.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![Esta imagem mostra o log de treinamento produzido pela execução do script lerobot_train.py em um ambiente Ubuntu local, centrado nos parâmetros de configuração do treinamento e no status ao vivo da execução. Ela marca claramente informações-chave do treinamento: `--dataset.split` tem como padrão `train`, o que significa que todo o conjunto de dados é usado como conjunto de treinamento, e, como o conjunto de dados está armazenado localmente, o estado de `--dataset.streaming` também fica fixo. O log também cobre o progresso do treinamento, o carregamento do conjunto de dados, a criação do otimizador e do scheduler do modelo, e os valores de loss e step durante o treinamento, dando uma visão clara de uma execução de treinamento local em andamento.](../../en/images/d34-02.png)
</column>
</grid>

![Esta imagem mostra o log de treinamento produzido ao treinar um modelo com o LeRobot em um ambiente Ubuntu. O log registra informações da execução do treinamento, como o horário, o conjunto de treinamento, o modelo, a loss e a acurácia. Os carimbos de tempo vão de 15:11:53 a 16:15:16 de 14 de janeiro de 2024, a loss oscila entre 0.68 e 0.65, e a acurácia (acc) entre 0.85 e 0.88. A imagem está relacionada ao contexto de treinamento do modelo LeRobot, apresentando visualmente como as métricas-chave mudam durante o treinamento.](../../en/images/d34-03.png)
