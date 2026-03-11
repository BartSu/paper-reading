# LLM Unlearning: Geometry-Aware & Data Augmentation Papers

Concise summaries of papers on **geometry-aware** and **data-augmentation** approaches to LLM/machine unlearning, with emphasis on **pre-unlearning data preparation** and **one-shot unlearning** (avoiding the unlearn–patch–unlearn cycle).

---

## Geometry-Aware

### 1. Geometric-disentanglement Unlearning (GU)

**Title:** Geometric-disentanglement Unlearning

**Authors:** Duo Zhou, Yuji Zhang, Tianxin Wei, Ruizhong Qiu, Ke Yang, Xiao Lin, Cheng Qian, Jingrui He (University of Illinois Urbana-Champaign), Hanghang Tong (University of Illinois Urbana-Champaign / Arizona State University), Chengxiang Zhai (University of Illinois Urbana-Champaign), Heng Ji (University of Illinois Urbana-Champaign), Huan Zhang (University of Illinois Urbana-Champaign)

**Venue:** ICML 2025

**TL;DR:** Forgetting updates often hurt retained knowledge; existing methods lack a principled way to avoid this. GU formalizes “no side effects” as local retain invariance and shows retain loss is invariant iff the update direction is orthogonal to the retain-gradient subspace. It provides a lightweight, plug-and-play projection for gradient-based unlearning.

---

### 2. Unified Gradient-Based Machine Unlearning with Remain Geometry Enhancement

**Title:** Unified Gradient-Based Machine Unlearning with Remain Geometry Enhancement

**Authors:** Zhehao Huang, Xinwen Cheng, JingHao Zheng, Haoran Wang, Zhengbao He, Tao Li, Xiaolin Huang (Shanghai Jiao Tong University)

**Venue:** NeurIPS 2024

**TL;DR:** Gradient-based unlearning methods are fragmented and ignore the geometry of retained data. This work unifies them via a decomposition into forgetting ascent, retaining descent, and a weight saliency term, and embeds updates in a manifold using second-order Hessian information from retained data to reduce interference.

---

### 3. AGTAO: Robust and Stabilized LLM Unlearning via Adaptive Orthogonality

**Title:** AGTAO: Robust and Stabilized LLM Unlearning via Adaptive Orthogonality

**Authors:** Pengyu Li, Lingling Zhang, Zhitao Gao, Yanrui Wu, Yuxuan Dong, Huan Liu, Bifan Wei, Jun Liu (Xi’an Jiaotong University)

**Venue:** arXiv:2602.01703 (under review)

**TL;DR:** Existing methods either over-forget (catastrophic forgetting) or under-forget (superficial forgetting, vulnerable to recovery). AGTAO uses adaptive orthogonality to resolve geometric gradient conflicts between forget and retain objectives, and adversarial gating to simulate and counter recovery attempts.

---

### 4. From Logits to Latents: Contrastive Representation Shaping for LLM Unlearning (CLReg)

**Title:** From Logits to Latents: Contrastive Representation Shaping for LLM Unlearning

**Authors:** Haoran Tang, Rajiv Khanna (Purdue University)

**Venue:** arXiv:2601.22028 (under review)

**TL;DR:** Most methods only suppress forgotten outputs in prediction space; forgotten concepts remain in representations and stay entangled with retained knowledge. CLReg shapes the representation space by pushing forget features away from retain features, reducing forget–retain entanglement with minimal shifts on retain features.

---

## Data Augmentation & Pre-Unlearning Preparation

### 5. ReLearn: Unlearning via Learning for Large Language Models

**Title:** ReLearn: Unlearning via Learning for Large Language Models

**Authors:** Haoming Xu, Ningyuan Zhao, Liming Yang, Sendong Zhao, Shumin Deng, Mengru Wang, Bryan Hooi, Nay Oo, Huajun Chen, Ningyu Zhang (Zhejiang University; National University of Singapore)

**Venue:** ACL 2025

**TL;DR:** Reverse-optimization unlearning disrupts token prediction and linguistic coherence. ReLearn replaces it with a data-augmentation and fine-tuning pipeline that achieves targeted forgetting while preserving output quality and coherence, avoiding the destructive side effects of gradient reversal.

---

### 6. LoReUn: Data Itself Implicitly Provides Cues to Improve Machine Unlearning

**Title:** LoReUn: Data Itself Implicitly Provides Cues to Improve Machine Unlearning

**Authors:** Xiang Li, Qianli Shen, Haonan Wang, Kenji Kawaguchi (National University of Singapore)

**Venue:** NeurIPS 2024 (Safe Generative AI Workshop)

**TL;DR:** Different forget samples have different unlearning difficulty, but methods treat them uniformly. LoReUn uses loss-based dynamic reweighting: it reweights forget data during unlearning based on implicit loss signals, closing the gap to exact unlearning with minimal extra cost.

---

### 7. What makes unlearning hard and what to do about it (RUM)

**Title:** What makes unlearning hard and what to do about it

**Authors:** Kairan Zhao, Meghdad Kurmanji, George-Octavian Bărbulescu, Eleni Triantafillou, Peter Triantafillou (University of Warwick)

**Venue:** NeurIPS 2024

**TL;DR:** Forget-set heterogeneity drives unlearning difficulty, but existing work uses random forget sets. RUM refines heterogeneous forget sets into homogenized subsets and applies unlearning per subset, then combines results—a **pre-unlearning data refinement** step that improves one-shot unlearning without iterative unlearn–patch cycles.

---

### 8. Reveal and Release: Iterative LLM Unlearning with Self-generated Data

**Title:** Reveal and Release: Iterative LLM Unlearning with Self-generated Data

**Authors:** Linxi Xie, Xin Teng, Shichang Ke, Hongyi Wen, Shengjie Wang (New York University Shanghai, Center for Data Science)

**Venue:** EMNLP 2025 Findings

**TL;DR:** Forget data is often unavailable or privacy-sensitive. This work uses a “Reveal” phase to prompt the model to generate forget data, then “Release” to iteratively unlearn with parameter-efficient modules—avoiding the need for external forget datasets while balancing forget quality and utility preservation.

---

## One-Shot Unlearning (Avoiding Unlearn–Patch–Unlearn)

### 9. UNO: Unlearning via Orthogonalization in Generative models

**Title:** UNO: Unlearning via Orthogonalization in Generative models

**Authors:** Pinak Mandal, Georg A. Gottwald (University of Sydney)

**Venue:** arXiv:2506.04712

**TL;DR:** Gradient-based unlearning is slow and often requires many steps. UNO uses loss-gradient orthogonalization for fast unlearning in generative models, achieving orders-of-magnitude speedup over gradient surgery while preserving generation fidelity and data influence.

---

### 10. PRUNE: A Patching Based Repair Framework for Certifiable Unlearning

**Title:** PRUNE: A Patching Based Repair Framework for Certifiable Unlearning of Neural Networks

**Authors:** Xuran Li, Jingyi Wang, Xiaohan Yuan, Peixin Zhang

**Venue:** arXiv:2505.06520

**TL;DR:** Retraining from scratch is costly; approximate unlearning lacks guarantees. PRUNE treats unlearning as neural network repair: it applies minimal patches to the model with certifiable guarantees, avoiding full retraining and the iterative unlearn–patch cycles typical of patch-based approaches.

---

## Summary Table

| Paper | Geometry | Data Aug / Pre-Refinement | One-Shot Focus |
|-------|----------|---------------------------|----------------|
| GU | ✓ | — | ✓ (plug-and-play) |
| Remain Geometry | ✓ | — | ✓ (50 steps) |
| AGTAO | ✓ | — | ✓ |
| CLReg | ✓ (representation) | — | ✓ | 
| ReLearn | — | ✓ | ✓ |
| LoReUn | — | ✓ (reweighting) | ✓ |
| RUM | — | ✓ (forget-set refinement) | ✓ |
| Reveal & Release | — | ✓ (self-generated) | Iterative |
| UNO | ✓ (orthogonalization) | — | ✓ |
| PRUNE | — | — | ✓ (patch-based) |

---

*Generated for the paper-reading research on LLM unlearning. See [AGENTS.md](../AGENTS.md) for retrieval instructions.*
