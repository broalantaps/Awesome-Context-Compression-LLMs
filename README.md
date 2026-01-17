# Awesome-Context-Compression-LLMs 🗜️

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)]()

> A curated collection of research papers focused on enhancing the efficiency of Large Language Models (LLMs) through context compression techniques. These methods aim to **reduce token usage**, **compress latent states**, and **optimize memory footprints (KV Cache)**.

## 📑 Table of Contents

- [Introduction](#-introduction)
- [Taxonomy](#-taxonomy)
- [Explicit Context Compression (Prompt/Input Level)](#-explicit-context-compression-promptinput-level)
- [Implicit Context Compression (Latent/Reasoning Level)](#-implicit-context-compression-latentreasoning-level)
- [Inference-Time KV Compression (Memory/Cache Level)](#-inference-time-kv-compression-memorycache-level)
- [Surveys](#-surveys)
- [Contributing](#-contributing)
- [Star History](#-star-history)

## 🎯 Introduction

As LLMs scale to handle longer contexts and more complex tasks, efficient context management becomes crucial. This repository organizes papers into three distinct categories based on **where** and **how** compression occurs:

1. **Explicit Compression**: Operates on input tokens before/during encoding
2. **Implicit Compression**: Compresses into latent representations
3. **KV Compression**: Optimizes the Key-Value cache during inference

## 🗂️ Taxonomy

```
Context Compression Methods
├── Explicit Context Compression (Input Level)
│   ├── Token Pruning (LLMLingua, Selective-Context)
│   ├── Summarization-based
│   └── Information-theoretic Selection
│
├── Implicit Context Compression (Latent Level)
│   ├── Soft Prompt (AutoCompressor)
│   ├── Autoencoder-based (ICAE, CoCom)
│   └── Latent Reasoning (Coconut)
│
└── Inference-Time KV Compression (Cache Level)
    ├── Eviction Policies (H2O, TOVA, SnapKV)
    ├── Quantization (KIVI)
    └── Sparse Attention (StreamingLLM)
```

---

## 📝 Explicit Context Compression (Prompt/Input Level)

**Definition**: Methods that operate primarily on the input text or input tokens. They select, prune, or summarize the context **before or during the initial encoding** to shorten the input sequence length. The goal is often to fit more context into the window or reduce API costs.

**Keywords**: `Prompt Compression`, `Token Pruning`, `Summarization`, `Information Entropy`, `Token Selection`, `Coarse-grained Pruning`

| Paper Title | Venue/Date | Tags | Code | TL;DR |
| :--- | :--- | :--- | :--- | :--- |
| [TokenSkip: Controllable Chain-of-Thought Compression in LLMs](https://arxiv.org/abs/2502.12067) | EMNLP 2025 Main | `Token Pruning`, `Distillation` | [GitHub](https://github.com/hemingkx/TokenSkip) | A simple yet effective approach that enables LLMs to selectively skip less important tokens, allowing for controllable CoT compression. |
| [LLMLingua-2: Data Distillation for Efficient and Faithful Task-Agnostic Prompt Compression](https://arxiv.org/abs/2403.12968) | ACL 2024 | `Pruning`, `Distillation` | [GitHub](https://github.com/microsoft/LLMLingua) | Learns compression from GPT-4 annotations, 3x-6x faster than LLMLingua |
| [LongLLMLingua: Accelerating and Enhancing LLMs in Long Context Scenarios via Prompt Compression](https://arxiv.org/abs/2310.06839) | ACL 2024 | `Pruning`, `RAG` | [GitHub](https://github.com/microsoft/LLMLingua) | Question-aware compression for RAG scenarios, reorders retrieved documents by relevance |
| [Keyformer: KV Cache Reduction through Key Tokens Selection for Efficient Generative Inference](https://arxiv.org/abs/2403.09054) | MLSys 2024 | `Pruning` | [GitHub](https://github.com/d-matrix-ai/keyformer) | Identifies key tokens at each layer for selective retention |
| [RECOMP: Improving Retrieval-Augmented LMs with Compression and Selective Augmentation](https://arxiv.org/abs/2310.04408) | ICLR 2024 | `Summarization`, `RAG` | [GitHub](https://github.com/carriex/recomp) | Trains extractive/abstractive compressors for retrieved documents |
| [Nugget: Neural Compression for Efficient Prompt Decoding](https://arxiv.org/abs/2310.04749) | ICLR 2024 | `Pruning` | - | Learns to identify and preserve "nugget" tokens for compression |
| [Walking Down the Memory Maze: Beyond Context Limit through Interactive Reading](https://arxiv.org/abs/2310.05029) | NAACL 2024 | `Summarization` | - | MemWalker: iteratively summarizes and navigates long documents |
| [LLMLingua: Compressing Prompts for Accelerated Inference of Large Language Models](https://arxiv.org/abs/2310.05736) | EMNLP 2023 | `Pruning` | [GitHub](https://github.com/microsoft/LLMLingua) | Uses a small LM to calculate perplexity and prune less informative tokens, achieving up to 20x compression |
| [Selective-Context: Compressing Contexts for Efficient Inference](https://arxiv.org/abs/2310.06201) | EMNLP 2023 | `Pruning`, `Self-Information` | [GitHub](https://github.com/liyucheng09/Selective_Context) | Filters out low self-information content using a small LM |

---

## 🧠 Implicit Context Compression (Latent/Reasoning Level)

**Definition**: Methods that compress context into **soft vectors, embeddings, or latent states**. This includes encoding long text into compact vector representations and **Latent Reasoning** where the "Chain of Thought" or intermediate reasoning steps are performed in the latent space (not outputting tokens) to reduce generation overhead.

**Keywords**: `Soft Prompt`, `Autoencoder`, `Memory Vectors`, `Latent Space Reasoning`, `Continuous Chain of Thought`, `Internal State Compression`

| Paper Title | Venue/Date | Tags | Code | TL;DR |
| :--- | :--- | :--- | :--- | :--- |
| [CLaRa: Bridging Retrieval and Generation with Continuous Latent Reasoning](https://arxiv.org/pdf/2511.18659) | - | `Memory Slots`, `RAG` | [Github](https://github.com/apple/ml-clara) | CLaRa achieves significant compression rates (32x-64x) while preserving essential information for accurate answer generation. |
| [REFRAG: Rethinking RAG based Decoding](https://arxiv.org/abs/2509.01092) | - | `Autoencoder`, `Memory Slots`, `RAG` | [Github](https://github.com/facebookresearch/refrag) | Reduces TTFT delay of RAG system by 30 times. |
| [Enhancing RAG Efficiency with Adaptive Context Compression](https://aclanthology.org/anthology-files/pdf/findings/2025.findings-emnlp.1307.pdf) | EMNLP 2025 | `Autoencoder`, `RAG` | - | A framework utilizing offline hierarchical compression and dynamic inference-time selection. It achieves >4x faster inference efficiency compared to standard RAG while maintaining competitive performance.|
| [PCC: Pretraining Context Compressor for Large Language Models with Embedding-Based Memory](https://aclanthology.org/2025.acl-long.1394.pdf) | ACL 2025 Main | `Autoencoder`, `Memory Slots` | [Github](https://github.com/microsoft/AnthropomorphicIntelligence) | Explore the upper limit of implicit compression ratio and connect to downstream LLMs faster. |
| [500xCompressor: Generalized Prompt Compression for Large Language Models](https://arxiv.org/abs/2408.03094) | ACL 2025 Main | `Autoencoder`, `Memory Slots` | [Github](https://github.com/ZongqianLi/500xCompressor) | Compresss a maximum of 500 natural language tokens into only 1 special token. |
| [Coconut: Chain of Continuous Thought](https://arxiv.org/abs/2412.06769) | arXiv 2024.12 | `Latent Reasoning`, `CoT` | [Github](https://github.com/facebookresearch/coconut) | Performs reasoning in continuous latent space without outputting tokens |
| [xRAG: Extreme Context Compression for Retrieval-augmented Generation with One Token](https://arxiv.org/abs/2405.13792) | NeurIPS 2024 | `RAG`, `Compression` | [Github](https://github.com/Hannibal046/xRAG) | Compresses retrieved documents into dense representations |
| [PEARL: Prompting Large Language Models to Plan and Execute Actions Over Long Documents](https://arxiv.org/abs/2305.14564) | ACL 2024 | `Planning`, `Long Doc` | [GitHub](https://github.com/SimengSun/pearl) | Decomposes long document QA into planning and execution |
| [ICAE: In-context Autoencoder for Context Compression in a Large Language Model](https://arxiv.org/abs/2307.06945) | ICLR 2024 | `Autoencoder`, `Memory Slots` | [GitHub](https://github.com/getao/ICAE) | ICAE compresses 512 tokens into 128 memory slots for 4x compression |
| [AutoCompressor: Adapting Language Models to Compress Contexts](https://arxiv.org/abs/2305.14788) | EMNLP 2023 | `Soft Prompt` | [GitHub](https://github.com/princeton-nlp/AutoCompressors) | AutoCompressor: recursively compresses segments into summary vectors |
| [Scaling Latent Reasoning via Thinking Tokens](https://arxiv.org/abs/2311.04254) | arXiv 2023.11 | `Latent Reasoning`, `Pause Token` | - | Uses "thinking tokens" for implicit reasoning steps |
| [Focused Transformer: Contrastive Training for Context Scaling](https://arxiv.org/abs/2307.03170) | NeurIPS 2023 | `Contrastive`, `Long-Context` | [GitHub](https://github.com/CStanKonrad/long_llama) | LongLLaMA: uses contrastive learning to focus on relevant context |
| [Learning to Compress Prompts with Gist Tokens](https://arxiv.org/abs/2304.08467) | NeurIPS 2023 | `Soft Prompt` | [GitHub](https://github.com/jayelm/gisting) | Compresses instructions into learnable gist tokens |
| [Parallel Context Windows for Large Language Models](https://arxiv.org/abs/2212.10947) | ACL 2023 | `Parallel`, `Memory` | - | PCW: processes context in parallel windows and aggregates |
| [Training Language Models with Memory Augmentation](https://arxiv.org/abs/2205.12674) | EMNLP 2022 | `Memory`, `TRIME` | [GitHub](https://github.com/princeton-nlp/TRIME) | TRIME: retrieves and integrates memory tokens during training |
| [Compressive Transformers for Long-Range Sequence Modelling](https://arxiv.org/abs/1911.05507) | ICLR 2020 | `Compression`, `Memory` | - | Uses compressed memory to extend context beyond window limits |

---

## ⚡ Inference-Time KV Compression (Memory/Cache Level)

**Definition**: Methods that specifically target the **Key-Value (KV) Cache** during the generation phase. They aim to reduce GPU memory usage and latency by evicting "unimportant" KV pairs, quantizing the cache, or using sparse attention patterns. These methods usually happen **on-the-fly during inference**.

**Keywords**: `KV Cache Eviction`, `Heavy Hitters`, `Sparse Attention`, `Cache Quantization`, `Budget-constrained Generation`, `Infinite Context`, `Streaming Inference`

| Paper Title | Venue/Date | Tags | Code | TL;DR |
| :--- | :--- | :--- | :--- | :--- |
| [ParallelComp: Parallel Long-Context Compressor for Length Extrapolation](https://arxiv.org/pdf/2502.14317) | ICML 2025 | `Sparse` | [GitHub](https://github.com/menik1126/ParallelComp) | This method divides long texts into smaller chunks and processes them in parallel while automatically removing redundant or irrelevant parts, greatly improving efficiency and performance. |
| [UNComp: Can Matrix Entropy Uncover Sparsity? — A Compressor Design from an Uncertainty-Aware Perspective](https://arxiv.org/pdf/2410.03090) | EMNLP 2025 | `Sparse`, `KV Cache Eviction` | [GitHub](https://github.com/menik1126/UNComp) | An uncertainty-aware framework that leverages truncated matrix entropy to identify areas of low information content, thereby revealing sparsity patterns that can be used for adaptive compression. |
| [Quest: Query-Aware Sparsity for Efficient Long-Context LLM Inference](https://arxiv.org/abs/2406.10774) | ICML 2024 | `Sparse`, `Query-Aware` | [GitHub](https://github.com/mit-han-lab/Quest) | Page-based KV management with query-aware selection |
| [PyramidKV: Dynamic KV Cache Compression based on Pyramidal Information Funneling](https://arxiv.org/abs/2406.02069) | arXiv 2024.06 | `Eviction`, `Layer-wise` | [GitHub](https://github.com/Zefan-Cai/PyramidKV) | Different layers retain different amounts of KV pairs (pyramid structure) |
| [MiniCache: KV Cache Compression in Depth Dimension for Large Language Models](https://arxiv.org/abs/2405.14366) | arXiv 2024.05 | `Eviction`, `Layer Merge` | - | Merges KV caches across similar layers to reduce memory |
| [SnapKV: LLM Knows What You are Looking for Before Generation](https://arxiv.org/abs/2404.14469) | arXiv 2024.04 | `Eviction`, `Observation Window` | [GitHub](https://github.com/FasterDecoding/SnapKV) | Uses observation window at prompt end to identify important KV pairs |
| [CaM: Cache Merging for Memory-efficient LLMs Inference](https://arxiv.org/abs/2403.17696) | ICLR 2024 | `Merging` | - | Merges similar KV pairs instead of hard eviction |
| [Dynamic Memory Compression: Retrofitting LLMs for Accelerated Inference](https://arxiv.org/abs/2403.09636) | ICML 2024 | `Compression`, `Learned` | - | DMC: learns to decide what to keep/discard dynamically |
| [Gear: An Efficient KV Cache Compression Recipe for Near-Lossless Generative Inference](https://arxiv.org/abs/2403.05527) | arXiv 2024.03 | `Quantization`, `Residual` | [GitHub](https://github.com/HaoKang-Timmy/Gear) | Quantize majority + low-rank for outliers + sparse residual |
| [KIVI: A Tuning-Free Asymmetric 2bit Quantization for KV Cache](https://arxiv.org/abs/2402.02750) | ICML 2024 | `Quantization`, `2-bit` | [GitHub](https://github.com/jy-yuan/KIVI) | Asymmetric quantization: 2-bit for Keys, 2-bit for Values with different schemes |
| [Anchor-based Large Language Models](https://arxiv.org/abs/2402.07616) | ACL 2024 | `Anchor`, `Compression` | [GitHub](https://github.com/lancopku/Anchor-LLM) | Groups and anchors tokens for parallel compression |
| [InfLLM: Unveiling the Intrinsic Capacity of LLMs for Understanding Extremely Long Sequences](https://arxiv.org/abs/2402.04617) | arXiv 2024.02 | `Block`, `Memory` | [GitHub](https://github.com/thunlp/InfLLM) | Uses block-level memory units for extreme-length processing |
| [KVQuant: Towards 10 Million Context Length LLM Inference with KV Cache Quantization](https://arxiv.org/abs/2401.18079) | arXiv 2024.01 | `Quantization`, `Per-Channel` | [GitHub](https://github.com/SqueezeAILab/KVQuant) | Per-channel quantization with outlier handling for extreme compression |
| [LoMA: Lossless Compressed Memory Attention](https://arxiv.org/abs/2401.09486) | arXiv 2024.01 | `Compression`, `Lossless` | - | Achieves lossless compression via efficient memory management |
| [TOVA: Token-wise Attention for Optimal KV-Cache Reduction](https://arxiv.org/abs/2401.06104) | arXiv 2024.01 | `Eviction`, `Token-wise` | [GitHub](https://github.com/schwartz-lab-NLP/TOVA) | Evicts tokens based on attention received in each generation step |
| [Efficient Streaming Language Models with Attention Sinks](https://arxiv.org/abs/2309.17453) | ICLR 2024 | `Streaming`, `Attention Sink` | [GitHub](https://github.com/mit-han-lab/streaming-llm) | StreamingLLM: keeps initial "sink" tokens + recent window for infinite streaming |
| [H2O: Heavy-Hitter Oracle for Efficient Generative Inference of Large Language Models](https://arxiv.org/abs/2306.14048) | NeurIPS 2023 | `Eviction`, `Heavy Hitter` | [GitHub](https://github.com/FMInference/H2O) | Keeps only "heavy-hitter" tokens (high cumulative attention) plus recent tokens |
| [Scissorhands: Exploiting the Persistence of Importance Hypothesis for LLM KV Cache Compression](https://arxiv.org/abs/2305.17118) | NeurIPS 2023 | `Eviction`, `Persistence` | - | Important tokens remain important; prunes based on historical importance |
| [FastGen: Adaptive KV Cache Compression for Efficient LLM Inference](https://arxiv.org/abs/2310.01801) | arXiv 2023.10 | `Eviction`, `Adaptive` | - | Adaptive compression policies based on attention patterns |

---

## 📚 Surveys

| Paper Title | Venue/Date | Focus |
| :--- | :--- | :--- |
| [Prompt Compression for Large Language Models: A Survey](https://arxiv.org/abs/2410.12388) | NAACL 2025 | Comprehensive survey on prompt compression |
| [A Survey on Efficient Inference for Large Language Models](https://arxiv.org/abs/2404.14294) | arXiv 2024 | Covers KV cache and other inference optimizations |
| [A Survey on Model Compression for Large Language Models](https://arxiv.org/abs/2308.07633) | TACL 2024 | General LLM compression (quantization, pruning, distillation) |
| [Beyond the Limits: A Survey of Techniques to Extend the Context Length in Large Language Models](https://arxiv.org/abs/2402.02244) | arXiv 2024 | Focuses on extending context length |
| [Length Extrapolation of Transformers: A Survey from the Perspective of Positional Encoding](https://arxiv.org/abs/2312.17044) | arXiv 2023 | Survey on positional encoding for long contexts |

---

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Add your paper following the table format
3. Ensure the paper is correctly categorized
4. Submit a Pull Request

### Categorization Guidelines

- **Cat 1 (Explicit)**: If the paper discusses compressing prompts *before* sending to the model/API
- **Cat 2 (Implicit)**: If the paper compresses into latent vectors or performs latent reasoning
- **Cat 3 (KV Cache)**: If the paper manages GPU memory by manipulating KV pairs *during* inference

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=broalantaps/Awesome-Context-Compression-LLMs)](https://star-history.com/#broalantaps/Awesome-Context-Compression-LLMs)


---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

Special thanks to all researchers whose work is featured in this repository. Your contributions to making LLMs more efficient benefit the entire community.

---

<p align="center">
  <i>If you find this repository helpful, please consider giving it a ⭐!</i>
</p>
