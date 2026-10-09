[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | Português (PT)

# Inferência D-Robotics RDK S100

Para o fluxo de implementação detalhado, consulte esta ligação<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## Implementação Ponta a Ponta do Modelo ACT no RDK S100/S100P

Nesta secção, guiamos você por todo o ciclo de implementação do modelo ACT no hardware da série D-Robotics RDK S100. Todo o processo tem três fases principais: **exportação do modelo**, **compilação por quantização** e **execução a bordo**.

<callout emoji="💡">
**Pré-requisitos:**
- **Máquina de desenvolvimento (Host):** utilizada para executar os passos 1 e 2, normalmente a sua máquina de treino de modelos (precisa de um desempenho razoável e do Docker instalado).
- **Placa (Edge):** a D-Robotics RDK S100/S100P, utilizada para executar o passo 3.
- **Toolchain:** este artigo baseia-se no repositório `rdk_LeRobot_tools`; consulte o [repositório GitHub](https://github.com/D-Robotics/rdk_LeRobot_tools) para mais detalhes.
</callout>

<callout emoji="🚨">
**Nota importante de compatibilidade de versões (leitura obrigatória):** o atual fluxo de exportação ONNX do `rdk_LeRobot_tools` é totalmente compatível com os **conjuntos de dados LeRobot v2.1**. Como a versão mais recente v3.0 altera a estrutura de dados, é **fortemente recomendado** que, antes de fazer o trabalho desta secção, mude o repositório principal `lerobot` para o commit específico compatível com a v2.1, para que o fluxo de exportação corra sem problemas. 
*Commit ID recomendado:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Fase 1: Exportar o Modelo para o Formato ONNX 💻 (na máquina de desenvolvimento)

Primeiro, precisamos de exportar o modelo **treinado em PyTorch** para um formato intermédio (ONNX).



#### **1. Clonar o Repositório da Toolchain** 

Vá para o seu diretório de trabalho `lerobot` e clone a toolchain específica do RDK:

```Bash
cd lerobot

# 1. Mudar para a versão estável compatível com os conjuntos de dados v2.1
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Clonar a toolchain específica do D-Robotics RDK
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Configurar os Parâmetros de Exportação** 

Edite o ficheiro `rdk_LeRobot_tools/bpu_export_config.yaml` e ajuste a configuração para corresponder aos seus caminhos reais:

```YAML
dataset:
  root: "data/so101_pick_place" # caminho absoluto ou relativo para o seu conjunto de dados
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # caminho para os pesos originais do modelo PyTorch
type: "nash-e" # arquitetura de hardware de destino; o RDK S100 corresponde a nash-e / o S100P corresponde a nash-m
```



#### 3. Executar o Script de Exportação

```Bash
# Exportar ONNX (máquina de desenvolvimento)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Indicador de sucesso**: é criada uma pasta `bpu_export_output` no diretório atual, contendo o script `build_all.sh` e os dados de calibração de quantização necessários mais tarde.



### Fase 2: Compilar o Modelo BPU 🐳 (num ambiente Docker na máquina de desenvolvimento)

A quantização e compilação de modelos BPU da D-Robotics requer um ambiente OpenExplorer (OE). Recomendamos usar Docker para isolar o ambiente.



#### **1.** **Preparar o Ambiente Docker e a Imagem** 

Certifique-se de que o Docker está instalado na máquina de desenvolvimento ([guia de instalação oficial](https://docs.docker.com/engine/install/)). Descarregue a imagem de CPU recomendada e carregue-a:

```Bash
# Carregar o arquivo de imagem offline descarregado
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Iniciar o Contentor de Compilação**

<callout emoji="⚠️">
**Aviso de armadilha**: a compilação do modelo precisa de uma grande quantidade de memória partilhada. Certifique-se de que adiciona o argumento `--shm-size=15g`, caso contrário é muito provável que ocorram erros de memória IPC.
</callout>

Monte o diretório de trabalho da máquina de desenvolvimento (que contém a pasta que acabou de exportar) no contentor:

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Nota: substitua `<docker-image-name>` pelo nome real da imagem que vê através de `sudo docker images`.)



#### **3.** **Executar a Compilação Dentro do Contentor** 

Quando estiver dentro do contentor, execute o script de compilação com um clique:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Verificar os Artefactos de Compilação** 

Depois de a compilação terminar, é criada uma pasta `bpu_output/` sob `bpu_export_output`. Contém todos os ficheiros essenciais necessários para executar na placa RDK: 

- Clique para ver a estrutura do diretório `bpu_output/`

  - `BPU_ACTPolicy_TransformerLayers.hbm` (ficheiro de modelo quantizado)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (ficheiro de modelo quantizado)
  - `action_mean.npy` e vários outros parâmetros de normalização do conjunto de dados
  - `camera1_mean.npy` e outros parâmetros estatísticos das câmaras

---

### Fase 3: Implementação e Inferência a Bordo 🤖 (no RDK S100)

<callout emoji="📌">
**Verificação de pré-requisitos:**
1. A placa RDK já tem o ambiente de execução `D-Robotics/lerobot` configurado, com `hbm_runtime` instalado.
2. Toda a pasta `bpu_output/` gerada no passo anterior foi totalmente copiada para a placa RDK, através de `scp`, de uma pen USB ou semelhante.
3. A configuração básica de teleoperação já está feita, garantindo que a porta série do braço, a porta USB da câmara e o ficheiro de calibração estão configurados corretamente.
</callout>



#### **1.** **Executar a Inferência Acelerada por BPU**

No terminal da placa RDK, vá para o diretório da toolchain e inicie o script de controlo:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Resolução de Problemas

Se encontrar problemas durante uma implementação real, verifique a seguinte lista:

- **O braço não se mexe?**

  - Verifique se o dispositivo está montado: escreva `ls /dev/ttyACM*` no terminal e confirme que a porta série do braço está correta.
  - Verifique as permissões: tente executar o script de inferência com `sudo`, ou adicione o utilizador atual ao grupo `dialout`.
- **Erro de streaming da câmara / imagem anormal / o braço treme no mesmo sítio?**

  - Confirme se o índice da câmara se desviou devido a uma ligação a quente e verifique se os parâmetros da câmara no código correspondem ao `/dev/video*` real.
- **Copiar ficheiros gerados pelo contentor na máquina de desenvolvimento dá "insufficient permissions"?**

  - Os ficheiros criados num diretório montado pelo Docker pertencem ao root por predefinição; execute `sudo chown -R $USER:$USER bpu_export_output` na máquina de desenvolvimento para corrigir.
