# Awesome-KV-Cache-Alternatives
## Alternatives to KV Caching in LLM Inference

Standard Key-Value (KV) caching accelerates Large Language Model (LLM) generation by storing pre-computed attention states. However, it consumes massive amounts of memory during long-context tasks. 

Modern alternatives optimize infrastructure by focusing on **architectural changes**, **algorithmic compression**, and **system-level offloading**.

---

## 1. Architectural Alternatives (Replacing Attention)
Instead of relying on the standard Transformer attention mechanism—which inherently demands a KV cache—these architectures process sequences linearly to bypass the bottleneck entirely.

* **State Space Models (SSMs):** Frameworks like **Mamba** use continuous-time dynamic systems that map sequences into discrete, recurrent operations. They compress history into fixed hidden state vectors, yielding $\mathcal{O}(1)$ memory complexity.
* **Retention Networks (RetNet):** This architecture combines the parallel training advantages of transformers with the low-cost inference of recurrent networks using constant decay mechanisms.
* **Multi-Head Latent Attention (MLA):** Utilized in models like **DeepSeek-V3**, MLA compresses traditional KV tensors into a shared, lower-rank latent space, radically shrinking the memory footprint at runtime.

## 2. Algorithmic Compression & Eviction
If you are working with standard Transformer architectures, you can use these software-level techniques to compress or drop less important cache data.

* **Quantization (e.g., KIVI):** Compresses KV tensors to lower bit-widths (e.g., 2-bit or 4-bit) during inference. This allows massive batch-size scaling with negligible drops in generation quality.
* **StreamingLLM (Attention Sinks):** Employs a rolling/sliding window approach. It retains only the earliest tokens (the attention anchor points) and the most recent tokens while safely evicting the middle context.
* **Prompt Caching:** Stores and reuses entire pre-computed prompt prefixes across multiple distinct user sessions instead of caching changing per-token states in GPU memory.
* **Depth-Dimension Merging (e.g., MiniCache):** Merges highly redundant KV caches across neighboring layers, shrinking the required memory footprint by up to 41%.

## 3. System-Level Offloading & Disaggregation
These alternatives do not modify the model itself. Instead, they reorganize how hardware handles the physical cache footprint to eliminate Out-Of-Memory (OOM) errors.

* **PagedAttention (vLLM):** Organizes the KV cache into non-contiguous blocks (like virtual memory pages) rather than massive contiguous arrays, eliminating internal fragmentation.
* **Distributed Caching (e.g., Mooncake, LMCache):** Implements disaggregated KV management. It offloads and scales cache storage away from expensive HBM/VRAM to standard CPU RAM or secondary NVMe tiers.

