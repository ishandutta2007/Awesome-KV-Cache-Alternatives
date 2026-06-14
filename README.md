# Awesome-KV-Cache-Alternatives
## Alternatives to KV Caching in LLM Inference

Standard Key-Value (KV) caching accelerates Large Language Model (LLM) generation by storing pre-computed attention states. However, it consumes massive amounts of memory during long-context tasks. 

Modern alternatives optimize infrastructure by focusing on **architectural changes**, **algorithmic compression**, and **system-level offloading**.

---

## 1. Architectural Alternatives (Replacing Attention)
Instead of relying on the standard Transformer attention mechanism—which inherently demands a KV cache—these architectures process sequences linearly to bypass the bottleneck entirely.

| Method | Description | Year | Paper Link |
| :--- | :--- | :--- | :--- |
| **State Space Models (SSMs)** | Frameworks like **Mamba** use continuous-time dynamic systems that map sequences into discrete, recurrent operations, yielding $\mathcal{O}(1)$ memory complexity. | 2023 | [arXiv:2312.00752](https://arxiv.org/abs/2312.00752) |
| **Retention Networks (RetNet)** | Combines parallel training of transformers with low-cost inference of recurrent networks using constant decay mechanisms. | 2023 | [arXiv:2307.08621](https://arxiv.org/abs/2307.08621) |
| **Multi-Head Latent Attention (MLA)** | Utilized in **DeepSeek-V3**, MLA compresses traditional KV tensors into a shared, lower-rank latent space, radically shrinking memory footprint. | 2024 | [arXiv:2405.04434](https://arxiv.org/abs/2405.04434) |

## 2. Algorithmic Compression & Eviction
If you are working with standard Transformer architectures, you can use these software-level techniques to compress or drop less important cache data.

| Method | Description | Year | Paper Link |
| :--- | :--- | :--- | :--- |
| **Quantization (KIVI)** | Compresses KV tensors to lower bit-widths (e.g., 2-bit or 4-bit) during inference, allowing massive batch-size scaling. | 2024 | [arXiv:2402.02750](https://arxiv.org/abs/2402.02750) |
| **StreamingLLM (Attention Sinks)** | Employs a rolling window approach, retaining earliest tokens (sinks) and most recent tokens while evicting the middle context. | 2023 | [arXiv:2309.17453](https://arxiv.org/abs/2309.17453) |
| **Prompt Caching** | Stores and reuses pre-computed prompt prefixes across distinct user sessions to reduce redundant computation. | 2023 | [arXiv:2311.04934](https://arxiv.org/abs/2311.04934) |
| **Depth-Dimension Merging (MiniCache)** | Merges redundant KV caches across neighboring layers, shrinking memory footprint by up to 41%. | 2024 | [arXiv:2405.14313](https://arxiv.org/abs/2405.14313) |

## 3. System-Level Offloading & Disaggregation
These alternatives do not modify the model itself. Instead, they reorganize how hardware handles the physical cache footprint to eliminate Out-Of-Memory (OOM) errors.

| Method | Description | Year | Paper Link |
| :--- | :--- | :--- | :--- |
| **PagedAttention (vLLM)** | Organizes KV cache into non-contiguous blocks (virtual memory pages), eliminating internal fragmentation. | 2023 | [arXiv:2309.06180](https://arxiv.org/abs/2309.06180) |
| **Distributed Caching (Mooncake, LMCache)** | Offloads and scales cache storage from HBM/VRAM to CPU RAM or secondary NVMe tiers using disaggregated management. | 2024 | [arXiv:2407.00079](https://arxiv.org/abs/2407.00079) |

