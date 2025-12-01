# Awesome-Context-Compression-LLMs 🗜️

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

**[English](README.md)** | **[中文](README_CN.md)**

> 一个精心整理的研究论文合集，专注于通过上下文压缩技术提升大语言模型（LLM）的效率。这些方法旨在**减少Token使用量**、**压缩潜在状态**以及**优化内存占用（KV缓存）**。

## 📑 目录

- [简介](#-简介)
- [分类体系](#-分类体系)
- [显式上下文压缩（提示/输入层面）](#-显式上下文压缩提示输入层面)
- [隐式上下文压缩（潜在/推理层面）](#-隐式上下文压缩潜在推理层面)
- [推理时KV压缩（内存/缓存层面）](#-推理时kv压缩内存缓存层面)
- [综述论文](#-综述论文)
- [如何贡献](#-如何贡献)
- [Star历史](#-star历史)

## 🎯 简介

随着大语言模型处理更长的上下文和更复杂的任务，高效的上下文管理变得至关重要。本仓库根据压缩发生的**位置**和**方式**将论文组织为三个不同的类别：

1. **显式压缩**：在编码之前/期间对输入Token进行操作
2. **隐式压缩**：压缩为潜在表示
3. **KV压缩**：在推理期间优化键值缓存

## 🗂️ 分类体系

```
上下文压缩方法
├── 显式上下文压缩（输入层面）
│   ├── Token剪枝（LLMLingua, Selective-Context）
│   ├── 基于摘要的方法
│   └── 信息论选择
│
├── 隐式上下文压缩（潜在层面）
│   ├── 软提示 / Gist Tokens
│   ├── 基于自编码器（ICAE, CoCom）
│   └── 潜在推理（Coconut）
│
└── 推理时KV压缩（缓存层面）
    ├── 驱逐策略（H2O, TOVA, SnapKV）
    ├── 量化（KIVI）
    └── 稀疏注意力（StreamingLLM）
```

---

## 📝 显式上下文压缩（提示/输入层面）

**定义**：主要对输入文本或Token进行操作的方法。它们在**初始编码之前或期间**选择、剪枝或总结上下文，以缩短输入序列长度。目标通常是在窗口内容纳更多上下文或降低API成本。

**关键词**：`提示压缩`、`Token剪枝`、`摘要`、`信息熵`、`Token选择`、`粗粒度剪枝`

| 论文标题 | 发表/日期 | 标签 | 代码 | 简述 |
| :--- | :--- | :--- | :--- | :--- |
| [LLMLingua-2: Data Distillation for Efficient and Faithful Task-Agnostic Prompt Compression](https://arxiv.org/abs/2403.12968) | ACL 2024 | `剪枝`, `蒸馏` | [GitHub](https://github.com/microsoft/LLMLingua) | 从GPT-4标注中学习压缩，比LLMLingua快3-6倍 |
| [LongLLMLingua: Accelerating and Enhancing LLMs in Long Context Scenarios via Prompt Compression](https://arxiv.org/abs/2310.06839) | ACL 2024 | `剪枝`, `RAG` | [GitHub](https://github.com/microsoft/LLMLingua) | 针对RAG场景的问题感知压缩，按相关性重排检索文档 |
| [Keyformer: KV Cache Reduction through Key Tokens Selection for Efficient Generative Inference](https://arxiv.org/abs/2403.09054) | MLSys 2024 | `剪枝` | [GitHub](https://github.com/d-matrix-ai/keyformer) | 在每一层识别关键Token进行选择性保留 |
| [In-context Autoencoder for Context Compression in a Large Language Model](https://arxiv.org/abs/2307.06945) | ICLR 2024 | `自编码器` | [GitHub](https://github.com/getao/ICAE) | 使用LoRA适配的LLaMA将长上下文压缩到记忆槽中 |
| [RECOMP: Improving Retrieval-Augmented LMs with Compression and Selective Augmentation](https://arxiv.org/abs/2310.04408) | ICLR 2024 | `摘要`, `RAG` | [GitHub](https://github.com/carriex/recomp) | 为检索文档训练抽取式/生成式压缩器 |
| [Nugget: Neural Compression for Efficient Prompt Decoding](https://arxiv.org/abs/2310.04749) | ICLR 2024 | `剪枝` | - | 学习识别和保留"金块"Token进行压缩 |
| [Walking Down the Memory Maze: Beyond Context Limit through Interactive Reading](https://arxiv.org/abs/2310.05029) | NAACL 2024 | `摘要` | - | MemWalker：迭代总结和导航长文档 |
| [LLMLingua: Compressing Prompts for Accelerated Inference of Large Language Models](https://arxiv.org/abs/2310.05736) | EMNLP 2023 | `剪枝` | [GitHub](https://github.com/microsoft/LLMLingua) | 使用小型LM计算困惑度并剪枝信息量少的Token，实现高达20倍压缩 |
| [Selective-Context: Compressing Contexts for Efficient Inference](https://arxiv.org/abs/2310.06201) | EMNLP 2023 | `剪枝`, `自信息` | [GitHub](https://github.com/liyucheng09/Selective_Context) | 使用小型LM过滤低自信息内容 |
| [Adapting Language Models to Compress Contexts](https://arxiv.org/abs/2305.14788) | EMNLP 2023 | `摘要` | [GitHub](https://github.com/princeton-nlp/AutoCompressors) | AutoCompressor：递归将片段压缩为摘要向量 |
| [Learning to Compress Prompts with Gist Tokens](https://arxiv.org/abs/2304.08467) | NeurIPS 2023 | `软提示`, `Gist` | [GitHub](https://github.com/jayelm/gisting) | 训练"gist tokens"将任务指令压缩为1-10个虚拟Token |

---

## 🧠 隐式上下文压缩（潜在/推理层面）

**定义**：将上下文压缩为**软向量、嵌入或潜在状态**的方法。这包括将长文本编码为紧凑的向量表示，以及**潜在推理**——"思维链"或中间推理步骤在潜在空间中执行（不输出Token）以减少生成开销。

**关键词**：`软提示`、`Gist Tokens`、`自编码器`、`记忆向量`、`潜在空间推理`、`连续思维链`、`内部状态压缩`

| 论文标题 | 发表/日期 | 标签 | 代码 | 简述 |
| :--- | :--- | :--- | :--- | :--- |
| [Coconut: Chain of Continuous Thought](https://arxiv.org/abs/2412.06769) | arXiv 2024.12 | `潜在推理`, `CoT` | - | 在连续潜在空间中进行推理，无需输出Token |
| [xRAG: Extreme Context Compression for Retrieval-Augmented Generation](https://arxiv.org/abs/2405.13792) | arXiv 2024.05 | `RAG`, `压缩` | - | 将检索文档压缩为密集表示 |
| [PEARL: Prompting Large Language Models to Plan and Execute Actions Over Long Documents](https://arxiv.org/abs/2305.14564) | ACL 2024 | `规划`, `长文档` | [GitHub](https://github.com/SimengSun/pearl) | 将长文档问答分解为规划和执行 |
| [In-context Autoencoder for Context Compression in a Large Language Model](https://arxiv.org/abs/2307.06945) | ICLR 2024 | `自编码器`, `记忆槽` | [GitHub](https://github.com/getao/ICAE) | ICAE将512个Token压缩为128个记忆槽，实现4倍压缩 |
| [Scaling Latent Reasoning via Thinking Tokens](https://arxiv.org/abs/2311.04254) | arXiv 2023.11 | `潜在推理`, `暂停Token` | - | 使用"思考Token"进行隐式推理步骤 |
| [Focused Transformer: Contrastive Training for Context Scaling](https://arxiv.org/abs/2307.03170) | NeurIPS 2023 | `对比学习`, `长上下文` | [GitHub](https://github.com/CStanKonrad/long_llama) | LongLLaMA：使用对比学习聚焦相关上下文 |
| [Learning to Compress Prompts with Gist Tokens](https://arxiv.org/abs/2304.08467) | NeurIPS 2023 | `Gist`, `指令` | [GitHub](https://github.com/jayelm/gisting) | 将指令压缩为可学习的gist tokens |
| [Adapting Language Models to Compress Contexts](https://arxiv.org/abs/2305.14788) | EMNLP 2023 | `递归`, `软提示` | [GitHub](https://github.com/princeton-nlp/AutoCompressors) | AutoCompressor：递归将长文档压缩为摘要向量 |
| [Parallel Context Windows for Large Language Models](https://arxiv.org/abs/2212.10947) | ACL 2023 | `并行`, `记忆` | - | PCW：在并行窗口中处理上下文并聚合 |
| [Training Language Models with Memory Augmentation](https://arxiv.org/abs/2205.12674) | EMNLP 2022 | `记忆`, `TRIME` | [GitHub](https://github.com/princeton-nlp/TRIME) | TRIME：在训练期间检索和整合记忆Token |
| [Compressive Transformers for Long-Range Sequence Modelling](https://arxiv.org/abs/1911.05507) | ICLR 2020 | `压缩`, `记忆` | - | 使用压缩记忆扩展上下文窗口限制 |

---

## ⚡ 推理时KV压缩（内存/缓存层面）

**定义**：专门针对生成阶段**键值（KV）缓存**的方法。它们旨在通过驱逐"不重要"的KV对、量化缓存或使用稀疏注意力模式来减少GPU内存使用和延迟。这些方法通常在**推理期间实时**发生。

**关键词**：`KV缓存驱逐`、`Heavy Hitters`、`稀疏注意力`、`缓存量化`、`预算约束生成`、`无限上下文`、`流式推理`

| 论文标题 | 发表/日期 | 标签 | 代码 | 简述 |
| :--- | :--- | :--- | :--- | :--- |
| [Quest: Query-Aware Sparsity for Efficient Long-Context LLM Inference](https://arxiv.org/abs/2406.10774) | ICML 2024 | `稀疏`, `查询感知` | [GitHub](https://github.com/mit-han-lab/Quest) | 基于页面的KV管理与查询感知选择 |
| [PyramidKV: Dynamic KV Cache Compression based on Pyramidal Information Funneling](https://arxiv.org/abs/2406.02069) | arXiv 2024.06 | `驱逐`, `分层` | [GitHub](https://github.com/Zefan-Cai/PyramidKV) | 不同层保留不同数量的KV对（金字塔结构） |
| [MiniCache: KV Cache Compression in Depth Dimension for Large Language Models](https://arxiv.org/abs/2405.14366) | arXiv 2024.05 | `驱逐`, `层合并` | - | 跨相似层合并KV缓存以减少内存 |
| [SnapKV: LLM Knows What You are Looking for Before Generation](https://arxiv.org/abs/2404.14469) | arXiv 2024.04 | `驱逐`, `观察窗口` | [GitHub](https://github.com/FasterDecoding/SnapKV) | 使用提示末尾的观察窗口识别重要的KV对 |
| [CaM: Cache Merging for Memory-efficient LLMs Inference](https://arxiv.org/abs/2403.17696) | ICLR 2024 | `合并` | - | 合并相似的KV对而非硬性驱逐 |
| [Dynamic Memory Compression: Retrofitting LLMs for Accelerated Inference](https://arxiv.org/abs/2403.09636) | ICML 2024 | `压缩`, `学习` | - | DMC：学习动态决定保留/丢弃什么 |
| [Gear: An Efficient KV Cache Compression Recipe for Near-Lossless Generative Inference](https://arxiv.org/abs/2403.05527) | arXiv 2024.03 | `量化`, `残差` | [GitHub](https://github.com/HaoKang-Timmy/Gear) | 量化大部分 + 低秩处理异常值 + 稀疏残差 |
| [KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache](https://arxiv.org/abs/2402.02750) | ICML 2024 | `量化`, `2-bit` | [GitHub](https://github.com/jy-yuan/KIVI) | 非对称量化：键和值使用不同的2-bit方案 |
| [Anchor-based Large Language Models](https://arxiv.org/abs/2402.07616) | ACL 2024 | `锚点`, `压缩` | [GitHub](https://github.com/lancopku/Anchor-LLM) | 分组和锚定Token进行并行压缩 |
| [InfLLM: Unveiling the Intrinsic Capacity of LLMs for Understanding Extremely Long Sequences](https://arxiv.org/abs/2402.04617) | arXiv 2024.02 | `块`, `记忆` | [GitHub](https://github.com/thunlp/InfLLM) | 使用块级记忆单元处理超长序列 |
| [KVQuant: Towards 10 Million Context Length LLM Inference with KV Cache Quantization](https://arxiv.org/abs/2401.18079) | arXiv 2024.01 | `量化`, `通道级` | [GitHub](https://github.com/SqueezeAILab/KVQuant) | 通道级量化与异常值处理实现极致压缩 |
| [LoMA: Lossless Compressed Memory Attention](https://arxiv.org/abs/2401.09486) | arXiv 2024.01 | `压缩`, `无损` | - | 通过高效内存管理实现无损压缩 |
| [TOVA: Token-wise Attention for Optimal KV-Cache Reduction](https://arxiv.org/abs/2401.06104) | arXiv 2024.01 | `驱逐`, `Token级` | [GitHub](https://github.com/schwartz-lab-NLP/TOVA) | 根据每个生成步骤中收到的注意力驱逐Token |
| [Efficient Streaming Language Models with Attention Sinks](https://arxiv.org/abs/2309.17453) | ICLR 2024 | `流式`, `注意力汇` | [GitHub](https://github.com/mit-han-lab/streaming-llm) | StreamingLLM：保留初始"汇"Token + 最近窗口实现无限流式推理 |
| [H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models](https://arxiv.org/abs/2306.14048) | NeurIPS 2023 | `驱逐`, `Heavy Hitter` | [GitHub](https://github.com/FMInference/H2O) | 仅保留"heavy-hitter" Token（高累积注意力）加最近Token |
| [Scissorhands: Exploiting the Persistence of Importance Hypothesis for LLM KV Cache Compression](https://arxiv.org/abs/2305.17118) | NeurIPS 2023 | `驱逐`, `持久性` | - | 重要Token保持重要；基于历史重要性剪枝 |
| [FastGen: Adaptive KV Cache Compression for Efficient LLM Inference](https://arxiv.org/abs/2310.01801) | arXiv 2023.10 | `驱逐`, `自适应` | - | 基于注意力模式的自适应压缩策略 |

---

## 📚 综述论文

| 论文标题 | 发表/日期 | 关注点 |
| :--- | :--- | :--- |
| [Prompt Compression for Large Language Models: A Survey](https://arxiv.org/abs/2410.12388) | NAACL 2025 | 提示压缩综合综述 |
| [A Survey on Efficient Inference for Large Language Models](https://arxiv.org/abs/2404.14294) | arXiv 2024 | 涵盖KV缓存和其他推理优化 |
| [A Survey on Model Compression for Large Language Models](https://arxiv.org/abs/2308.07633) | TACL 2024 | 通用LLM压缩（量化、剪枝、蒸馏） |
| [Beyond the Limits: A Survey of Techniques to Extend the Context Length in Large Language Models](https://arxiv.org/abs/2402.02244) | arXiv 2024 | 专注于扩展上下文长度 |
| [Length Extrapolation of Transformers: A Survey from the Perspective of Positional Encoding](https://arxiv.org/abs/2312.17044) | arXiv 2023 | 长上下文位置编码综述 |

---

## 🤝 如何贡献

欢迎贡献！请按照以下步骤操作：

1. Fork本仓库
2. 按照表格格式添加论文
3. 确保论文正确分类
4. 提交Pull Request

### 分类指南

- **类别1（显式）**：如果论文讨论在发送到模型/API之*前*压缩提示
- **类别2（隐式）**：如果论文压缩为潜在向量或执行潜在推理
- **类别3（KV缓存）**：如果论文在推理*期间*通过操作KV对来管理GPU内存

---

## ⭐ Star历史

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/Awesome-Context-Compression-LLMs&type=Date)](https://star-history.com/#yourusername/Awesome-Context-Compression-LLMs&Date)

---

## 📄 许可证

本项目采用MIT许可证 - 详见[LICENSE](LICENSE)文件。

---

## 🙏 致谢

特别感谢本仓库中收录的所有研究者。您们为使LLM更高效所做的贡献惠及整个社区。

---

<p align="center">
  <i>如果您觉得这个仓库有帮助，请考虑给它一个 ⭐！</i>
</p>

