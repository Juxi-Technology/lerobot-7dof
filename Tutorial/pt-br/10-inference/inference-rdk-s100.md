[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | Português (BR) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# Inferência no D-Robotics RDK S100

Para o fluxo de implementação detalhado, consulte este link<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## Implantação ponta a ponta do modelo ACT no RDK S100/S100P

Esta seção orienta você pelo ciclo completo de implantação do modelo ACT no hardware da série D-Robotics RDK S100. Todo o processo tem três estágios principais: **exportação do modelo**, **compilação de quantização** e **execução a bordo**.

<callout emoji="💡">
**Pré-requisitos:**
- **Máquina de desenvolvimento (Host):** usada para executar as etapas 1 e 2, geralmente sua máquina de treinamento de modelo (ela precisa de um desempenho razoável e do Docker instalado).
- **Placa (Edge):** a D-Robotics RDK S100/S100P, usada para executar a etapa 3.
- **Toolchain:** este artigo depende do repositório `rdk_LeRobot_tools`; consulte o [repositório do GitHub](https://github.com/D-Robotics/rdk_LeRobot_tools) para detalhes.
</callout>

<callout emoji="🚨">
**Observação importante de compatibilidade de versão (leitura obrigatória):** o fluxo atual de exportação ONNX do `rdk_LeRobot_tools` é totalmente compatível com **conjuntos de dados LeRobot v2.1**. Como a v3.0 mais recente altera a estrutura de dados, é **fortemente recomendado** que, antes de fazer o trabalho desta seção, você mude o repositório principal `lerobot` para o commit específico compatível com a v2.1, para que o fluxo de exportação funcione sem problemas. 
*Commit ID recomendado:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Etapa 1: Exporte o modelo para o formato ONNX 💻 (na máquina de desenvolvimento)

Primeiro, precisamos exportar o modelo **treinado em PyTorch** para um formato intermediário (ONNX).



#### **1. Clone o repositório da toolchain** 

Vá até seu diretório de trabalho `lerobot` e clone a toolchain específica do RDK:

```Bash
cd lerobot

# 1. Mude para a versão estável compatível com os conjuntos de dados v2.1
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Clone a toolchain específica do D-Robotics RDK
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Configure os parâmetros de exportação** 

Edite o arquivo `rdk_LeRobot_tools/bpu_export_config.yaml` e ajuste a configuração para corresponder aos seus caminhos reais:

```YAML
dataset:
  root: "data/so101_pick_place" # caminho absoluto ou relativo para o seu conjunto de dados
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # caminho para os pesos originais do modelo PyTorch
type: "nash-e" # arquitetura de hardware de destino; RDK S100 corresponde a nash-e / S100P corresponde a nash-m
```



#### 3. Execute o script de exportação

```Bash
# Exportar ONNX (máquina de desenvolvimento)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Indicador de sucesso**: uma pasta `bpu_export_output` é criada no diretório atual, contendo o script `build_all.sh` e os dados de calibração de quantização necessários mais tarde.



### Etapa 2: Compile o modelo BPU 🐳 (em um ambiente Docker na máquina de desenvolvimento)

Quantizar e compilar modelos BPU da D-Robotics exige um ambiente OpenExplorer (OE). Recomendamos usar Docker para isolar o ambiente.



#### **1.** **Prepare o ambiente Docker e a imagem** 

Certifique-se de que o Docker está instalado na máquina de desenvolvimento ([guia de instalação oficial](https://docs.docker.com/engine/install/)). Baixe a imagem de CPU recomendada e carregue-a:

```Bash
# Carregue o arquivo de imagem offline baixado
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Inicie o contêiner de compilação**

<callout emoji="⚠️">
**Aviso de armadilha**: a compilação do modelo exige uma grande quantidade de memória compartilhada. Certifique-se de adicionar o argumento `--shm-size=15g`, caso contrário erros de memória IPC são muito prováveis.
</callout>

Monte o diretório de trabalho da máquina de desenvolvimento (contendo a pasta que você acabou de exportar) no contêiner:

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Observação: substitua `<docker-image-name>` pelo nome real da imagem que você vê via `sudo docker images`.)



#### **3.** **Execute a compilação dentro do contêiner** 

Já dentro do contêiner, execute o script de compilação em um clique:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Verifique os artefatos de build** 

Depois que a compilação termina, uma pasta `bpu_output/` é criada sob `bpu_export_output`. Ela contém todos os arquivos principais necessários para rodar na placa RDK: 

- Clique para ver a estrutura do diretório `bpu_output/`

  - `BPU_ACTPolicy_TransformerLayers.hbm` (arquivo de modelo quantizado)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (arquivo de modelo quantizado)
  - `action_mean.npy` e vários outros parâmetros de normalização do conjunto de dados
  - `camera1_mean.npy` e outros parâmetros de estatísticas da câmera

---

### Etapa 3: Implantação a bordo e inferência 🤖 (no RDK S100)

<callout emoji="📌">
**Verificação de pré-requisitos:**
1. A placa RDK já tem o ambiente de execução `D-Robotics/lerobot` configurado, com `hbm_runtime` instalado.
2. Toda a pasta `bpu_output/` gerada na etapa anterior foi totalmente copiada para a placa RDK, via `scp`, um drive USB ou similar.
3. A configuração básica de teleoperação já está feita, garantindo que a porta serial do braço, a porta USB da câmera e o arquivo de calibração estejam configurados corretamente.
</callout>



#### **1.** **Execute a inferência acelerada por BPU**

No terminal da placa RDK, vá até o diretório da toolchain e inicie o script de controle:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Solução de problemas

Se você encontrar problemas durante uma implantação real, verifique a lista a seguir:

- **O braço não se move?**

  - Verifique se o dispositivo está montado: digite `ls /dev/ttyACM*` no terminal e confirme se a porta serial do braço está correta.
  - Verifique as permissões: tente executar o script de inferência com `sudo`, ou adicione o usuário atual ao grupo `dialout`.
- **Erro de streaming da câmera / imagem anormal / o braço treme no lugar?**

  - Confirme se o índice da câmera mudou por causa de um hot-plug, e verifique se os parâmetros da câmera no código correspondem ao `/dev/video*` real.
- **Copiar arquivos gerados pelo contêiner na máquina de desenvolvimento informa "permissões insuficientes"?**

  - Arquivos criados em um diretório montado no Docker pertencem ao root por padrão; execute `sudo chown -R $USER:$USER bpu_export_output` na máquina de desenvolvimento para corrigir.
