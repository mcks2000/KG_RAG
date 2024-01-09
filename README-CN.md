<p align="center">
  <img src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/0b2f5b42-761e-4d5b-8d6f-77c8b965f017" width="450">
</p>



[中文](https://github.com/mcks2000/KG_RAG/blob/main/README-CN.md) ｜ [英文](https://github.com/mcks2000/KG_RAG/blob/main/README-EN.md)
## 目录
[什么是 KG-RAG](https://github.com/mcks2000/KG_RAG#what-is-kg-rag)

[KG-RAG 使用实例](https://github.com/mcks2000/KG_RAG#example-use-case-of-kg-rag)
 - [不使用 KG-RAG 提示 GPT](https://github.com/mcks2000/KG_RAG#without-kg-rag)  
 - [使用 KG-RAG 提示 GPT](https://github.com/mcks2000/KG_RAG#with-kg-rag)

[如何运行 KG-RAG](https://github.com/mcks2000/KG_RAG#how-to-run-kg-rag)
 - [第 1 步：克隆 repo](https://github.com/mcks2000/KG_RAG#step-1-clone-the-repo)
 - [第 2 步：创建虚拟环境](https://github.com/mcks2000/KG_RAG#step-2-create-a-virtual-environment)
 - [第 3 步：安装依赖项](https://github.com/mcks2000/KG_RAG#step-3-install-dependencies)
 - [第 4 步：更新 config.yaml](https://github.com/mcks2000/KG_RAG#step-4-update-configyaml)
 - [第 5 步：运行设置脚本](https://github.com/mcks2000/KG_RAG#step-5-run-the-setup-script)
 - [第 6 步：从终端运行 KG-RAG](https://github.com/mcks2000/KG_RAG#step-6-run-kg-rag-from-your-terminal)
    - [使用 GPT](https://github.com/mcks2000/KG_RAG#using-gpt)
    - [使用 GPT 交互模式](https://github.com/mcks2000/KG_RAG/blob/main/README.md#using-gpt-interactive-mode)
    - [使用 Llama](https://github.com/mcks2000/KG_RAG#using-llama)
    - [使用 Llama 交互模式](https://github.com/mcks2000/KG_RAG/blob/main/README.md#using-llama-interactive-mode)

## 什么是 KG-RAG?

KG-RAG 代表基于知识图谱的检索增强生成。

### 从观看 KG-RAG 视频开始

<video src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/86e5b8a3-eb58-4648-95a4-271e9c69b4ed" controls="controls" style="max-width: 730px;">
</video>

该视频讲述了，它将知识图谱（Knowledge Graph, KG）的显式知识与大型语言模型（Large Language Model, LLM）的隐式知识结合起来，详情可以参考论文[arXiv preprint](https://arxiv.org/abs/2311.17330) 

在这里，我们利用了一个名为 [SPOKE](https://spoke.ucsf.edu/) 的大型生物医学知识图谱作为生物医学背景的提供者。SPOKE整合了来自不同领域的40多个生物医学知识库，每个知识库都专注于基因、蛋白质、药物、化合物、疾病等生物医学概念及其之间的关系。SPOKE包含了超过2700万个节点，分为21种不同类型，以及5500万条边，分为55种类型[[参考文献](https://doi.org/10.1093/bioinformatics/btad080)]。


KG-RAG 的主要特点是从 SPOKE 知识图谱中提取“prompt-aware context”（提示感知上下文），其定义为：

**足以响应用户提示的最小上下文。** 

因此，这个框架通过从生物医学知识图谱中获得优化的领域特定的“prompt-aware context”（提示感知上下文），增强了通用目的的大型语言模型。



## KG-RAG 使用实例
以下摘要显示了FDA [网站](https://www.fda.gov/drugs/news-events-human-drugs/fda-approves-treatment-weight-management-patients-bardet-biedl-syndrome-aged-6-or-older)  上关于药物**"西瓜甙"**获得批准用于治疗巴代特-*比德尔综合症患者* 的体重管理的新闻。你可以通过此链接查看完整信息。


<img src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/fc4d0b8d-0edb-461d-86c5-9d0d191bd97d" width="600" height="350">

### 请向 GPT-4 询问有关上述药物的情况：

### 不带 KG-RAG

*注：本例使用 KG-RAG v0.3.0 运行。我们是从终端而不是 chatGPT 浏览器提示 GPT。所有分析中的温度参数都设置为 0。参数设置请参阅 [this](https://github.com/mcks2000/KG_RAG/blob/main/config.yaml) yaml 文件*。

<video src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/dbabb812-2a8a-48b6-9785-55b983cb61a4" controls="controls" style="max-width: 730px;">
</video>

### 带 KG-RAG

*注：本例使用 KG-RAG v0.3.0 运行。所有分析中的温度参数都设置为 0。参数设置请参阅 [this](https://github.com/mcks2000/KG_RAG/blob/main/config.yaml) yaml 文件*。

<video src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/acd08954-a496-4a61-a3b1-8fc4e647b2aa" controls="controls" style="max-width: 730px;">
</video>

可以看出，KG-RAG 能够提供有关 FDA 批准的 [药物](https://www.fda.gov/drugs/news-events-human-drugs/fda-approves-treatment-weight-management-patients-bardet-biedl-syndrome-aged-6-or-older) 的正确信息。



## 如何运行 KG-RAG

**注：目前，KG-RAG 专为运行与疾病有关的提示而设计。我们正在积极改进其多功能性。**

### 第 1 步：克隆 repo

克隆此资源库。论文中使用的所有生物医学数据都已上传到此资源库，因此您无需单独下载。

### 第 2 步：创建虚拟环境
注：本资源库中的脚本使用 python 3.10.9 运行
```
conda create -n kg_rag python=3.10.9
conda activate kg_rag
cd KG_RAG
```

### 第 3 步：安装依赖项

```
pip install -r requirements.txt
```

### 第 4 步：更新 config.yaml
[config.yaml](https://github.com/mcks2000/KG_RAG/blob/main/config.yaml)  包含运行您机器上脚本所需的所有必要信息。请确保相应地填充 [此](https://github.com/mcks2000/KG_RAG/blob/main/config.yaml) yaml 文件。

注意：还有另一个名为 [system_prompts.yaml](https://github.com/mcks2000/KG_RAG/blob/main/system_prompts.yaml) 的 yaml 文件。这个文件已经填充完毕，它包含 KG-RAG 框架中使用的所有系统提示。

- 修改 data 文件路径
- 配置 openAI （如果使用）

### 第 5 步：运行设置脚本
注意：确保您位于 KG_RAG 文件夹内。

设置脚本以交互式方式运行。

运行设置脚本将会：

- 为 KG-RAG 创建疾病向量数据库
- 在您的机器上下载 Llama 模型（可选，您可以跳过这一步，完全没问题）。

```
python -m kg_rag.run_setup
```

### 第 6 步：从终端运行 KG-RAG
注意：确保您在 KG_RAG 文件夹中

您可以使用 GPT 和 Llama 模型运行 KG-RAG。

#### Using GPT

```
python -m kg_rag.rag_based_generation.GPT.text_generation -g <your favorite gpt model - "gpt-4" or "gpt-35-turbo">
```

示例：

注：以下示例在 AWS p3.8xlarge EC2 实例上运行，并使用 KG-RAG v0.3.0。

<video src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/defcbff7-e777-4db6-b028-10f54c76b234" controls="controls" style="max-width: 730px;">
</video>

#### 使用 GPT 交互模式
这使用户可以以交互式方式逐步查看整个流程。

```
python -m kg_rag.rag_based_generation.GPT.text_generation -i True -g <your favorite gpt model - "gpt-4" or "gpt-35-turbo">
```

#### Using Llama
注意：如果在 [setup](https://github.com/mcks2000/KG_RAG#step-5-run-the-setup-script) 步骤中没有下载 Llama，那么在运行以下程序时，可能会花费一些时间，因为它会先下载模型。

```
python -m kg_rag.rag_based_generation.Llama.text_generation -m <method-1 or method2, if nothing is mentioned it will take 'method-1'>
```

示例：

注：以下示例在 AWS p3.8xlarge EC2 实例上运行，使用的是 KG-RAG v0.3.0。

<video src="https://github.com/BaranziniLab/KG_RAG/assets/42702311/94bda923-dafb-451a-943a-1d7c65f3ffd4" controls="controls" style="max-width: 730px;">
</video>

#### 使用 Llama 互动模式

这允许用户以交互式的方式逐步查看整个过程的每一步。

```
python -m kg_rag.rag_based_generation.Llama.text_generation -i True -m <method-1 or method2, if nothing is mentioned it will take 'method-1'>
```















