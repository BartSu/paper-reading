# Cross-Model Causal Mediation Analysis for Fine-Tuning

This note answers three practical questions:

1. **What can cross-model causal mediation analysis (CMA) be used to study before vs. after fine-tuning?**
2. **Has anyone used closely related methods already?**
3. **What problems have they studied with these methods?**

Short answer:

- **Yes, but usually not under the exact name "cross-model CMA before/after fine-tuning".**
- The closest prior work already uses **causal mediation analysis** to:
  - localize bias-related components,
  - compare **before vs. after debiasing / fine-tuning** changes,
  - discover components to **selectively fine-tune**,
  - study how model internals mediate linguistic knowledge, reasoning, and downstream decisions,
  - and more recently, identify **where to intervene** for activation steering in generative LMs.
- What still looks **less standardized and still open** is a systematic framework that compares:
  - a **base model**,
  - one or more **fine-tuned checkpoints**,
  - and optionally **different model families / tuning recipes**
  
  using the **same mediator definition and the same causal effect metrics** across checkpoints.

## 1) What cross-model CMA can analyze around fine-tuning

Here "cross-model" can mean:

- **same model before vs. after fine-tuning**,
- **same base model with different fine-tuning methods** (SFT vs. LoRA vs. DPO vs. RLHF),
- or **different model families** evaluated with the same mediator set and prompts.

### A. Mediator migration after fine-tuning

Question:

> Which layers / heads / MLP blocks / latent directions become more or less causally important after fine-tuning?

Useful outputs:

- per-component **NIE** (natural indirect effect) before vs. after fine-tuning,
- rank shifts of top mediators,
- "old mediators kept vs. new mediators created" overlap analysis.

This tells you whether fine-tuning mostly:

- reuses pre-trained circuits,
- amplifies existing pathways,
- suppresses old pathways,
- or creates new task-specific mechanisms.

### B. Direct-vs-indirect effect shift

Question:

> Did fine-tuning improve behavior by strengthening mediated reasoning pathways, or by introducing shortcuts?

Useful outputs:

- **TE / NDE / NIE** change for the same task behavior,
- indirect-effect concentration by layer or module type.

This is useful for distinguishing:

- genuine mechanism formation,
- superficial output steering,
- and brittle shortcut learning.

### C. Inherited vs. newly created circuits

Question:

> Is the fine-tuned model's successful behavior inherited from the base model, or newly formed during tuning?

Useful outputs:

- cross-checkpoint activation patching,
- mediator overlap across checkpoints,
- asymmetric transfer: base -> fine-tuned vs. fine-tuned -> base.

This is especially useful for:

- instruction tuning,
- safety tuning,
- domain adaptation,
- and post-training alignment.

### D. Utility-gain vs. side-effect coupling

Question:

> Are the same mediators responsible for both task improvement and unwanted side effects?

Examples of side effects:

- gender / social bias,
- toxicity,
- refusal over-triggering,
- hallucination,
- loss of factual recall,
- spurious annotation bias.

Useful outputs:

- mediator overlap between **target capability** and **undesired behavior**,
- effect trade-off curves when ablating top mediators,
- per-layer conflict analysis.

### E. Fine-tuning recipe comparison

Question:

> Do full fine-tuning, LoRA, adapters, SFT, DPO, or RLHF change the model through the same causal pathways?

Useful outputs:

- mediator stability across tuning methods,
- effect concentration under parameter-efficient tuning,
- whether PEFT edits a narrow set of mediators or just reweights readout behavior.

### F. Cross-model transferability

Question:

> Are the same mediators present across model sizes or model families?

Useful outputs:

- mediator Jaccard overlap,
- cross-model rank correlation of causal effects,
- family-specific vs. universal mediator patterns.

This is useful for asking whether a phenomenon is:

- architecture-specific,
- scale-sensitive,
- or robust across families.

### G. Robustness before and after fine-tuning

Question:

> Did fine-tuning make the mediated mechanism more robust under paraphrase, adversarial prompting, or multi-turn interaction?

Useful outputs:

- NIE stability under paraphrase / prompt variation,
- mediator drift under distribution shift,
- comparison of easy vs. hard examples.

### H. Localization for selective editing or selective fine-tuning

Question:

> Can CMA tell us *where* to fine-tune or edit instead of updating the whole model?

Useful outputs:

- top-k mediator sets for targeted PEFT,
- layer/head selection for intervention,
- causal budget estimates for minimal updates.

This is one of the strongest practical uses of CMA-like analysis.

## 2) Evidence-based answer to your question

### Has this kind of method been used?

**Yes, partially.**

The literature already contains three strong precedents:

1. **CMA on pre-trained LMs** to find which internal components mediate a behavior such as gender bias.
2. **CMA before vs. after debiasing / fine-tuning** to ask what changed internally.
3. **CMA-guided localization followed by targeted fine-tuning / editing / steering**.

### What has it been used to study?

Mostly these topics:

- **gender bias / fairness / debiasing**,
- **annotation bias in NLU**,
- **semantic competence** (e.g. distributivity),
- **mechanistic reasoning** (e.g. arithmetic),
- **downstream explanation** (e.g. rumour detection),
- **activation steering / behavior control** in generative LMs.

### What seems less explored?

What I did **not** find as a mature, standard line yet is:

> a benchmarked "cross-model CMA" workflow that systematically compares a base model and multiple post-fine-tuning checkpoints with the same mediator definitions, the same prompts, and the same causal metrics.

So the answer is:

- **The ingredients exist and have definitely been used.**
- **Your exact framing is plausible and timely.**
- **There is still room for a more explicit checkpoint-to-checkpoint / tuning-method-to-tuning-method framework.**

## 3) Most direct prior work for before/after fine-tuning or cross-model comparison

### 1) What changed? Investigating Debiasing Methods using Causal Mediation Analysis
- **Authors:** Sullam Jeoung; Jana Diesner
- **Venue:** GeBNLP 2022 / arXiv:2206.00701
- **Why it matters for your question:** This is one of the clearest precedents for comparing **model internals before vs. after debiasing / fine-tuning-like intervention** using CMA.
- **What it studied:** How debiasing methods change the internal behavior of language models with respect to **gender bias** in downstream toxicity detection, including which layers and attention heads change the most.
- **Closest use case:** Pre-/post-intervention CMA; cross-model comparison across debiasing setups.
- **Link:** https://aclanthology.org/2022.gebnlp-1.26/

### 2) Identifying and Adapting Transformer-Components Responsible for Gender Bias in an English Language Model
- **Authors:** Abhijith Chintam; Rahel Beloch; Willem Zuidema; Michael Hanna; Oskar van der Wal
- **Venue:** BlackboxNLP 2023 / arXiv:2310.12611
- **Why it matters for your question:** This work compares **causal mediation analysis**, automated circuit discovery, and differential masking to locate components responsible for gender bias, then uses those discovered components for **parameter-efficient fine-tuning**.
- **What it studied:** Which GPT-2 Small components causally mediate gender bias, and whether selectively fine-tuning those components can mitigate bias with less damage than full fine-tuning.
- **Closest use case:** CMA-guided selective fine-tuning; "locate first, then tune."
- **Link:** https://arxiv.org/abs/2310.12611

### 3) Reducing Bias in Sentiment Analysis Models Through Causal Mediation Analysis and Targeted Counterfactual Training
- **Authors:** Yifei Da; Matias Nicolas Bossa; Abel Diaz Berenguer; Hichem Sahli
- **Venue:** IEEE Access 2024
- **Why it matters for your question:** This is another direct example where CMA is used to identify bias-mediating components in **fine-tuned BERT sentiment models**, followed by targeted mitigation.
- **What it studied:** Bias reduction in sentiment analysis under counterfactual gender perturbations, while preserving predictive performance.
- **Closest use case:** Fine-tuned model analysis plus targeted post-hoc retraining.
- **Link:** https://researchportal.vub.be/en/publications/reducing-bias-in-sentiment-analysis-models-through-causal-mediati/

### 4) Identifying and Mitigating Annotation Bias in Natural Language Understanding using Causal Mediation Analysis
- **Authors:** Sitiporn Sae Lim; Can Udomcharoenchaikit; Peerat Limkonchotiwat; Ekapol Chuangsuwanich; Sarana Nutanong
- **Venue:** Findings of ACL 2024
- **Why it matters for your question:** Shows that CMA can be used on **fine-tuned NLU models** to locate annotation-bias-mediating components and then design targeted mitigation methods.
- **What it studied:** Spurious annotation artifacts in NLU tasks and how to reduce them with causal-grounded masking and gradient unlearning.
- **Closest use case:** Fine-tuned downstream models; mediator localization before targeted mitigation.
- **Link:** https://aclanthology.org/2024.findings-acl.686/

### 5) Locating and Mitigating Gender Bias in Large Language Models
- **Authors:** Yuchen Cai; Ding Cao; Rongxi Guo; Yaqin Wen; Guiquan Liu; Enhong Chen
- **Venue:** arXiv preprint, 2024
- **Why it matters for your question:** Uses causal mediation ideas to locate bias-mediating components in LLMs and couples localization with mitigation.
- **What it studied:** Gender bias in occupational pronoun completion and knowledge-editing-style mitigation after component localization.
- **Closest use case:** LLM-scale mediator localization for post-training intervention.
- **Link:** https://arxiv.org/abs/2403.14409

## 4) Closely related CMA work on model internals, reasoning, and linguistic competence

### 6) Investigating Gender Bias in Language Models Using Causal Mediation Analysis
- **Authors:** Jesse Vig; Sebastian Gehrmann; Yonatan Belinkov; Sharon Qian; Daniel Nevo; Yaron Singer; Stuart Shieber
- **Venue:** NeurIPS 2020 / arXiv:2004.12265
- **Why it matters for your question:** This is the foundational NLP/Transformer CMA paper. It established the basic recipe of measuring how neurons and attention heads **mediate** model behavior.
- **What it studied:** Gender bias in pre-trained Transformer language models, showing bias effects are sparse, synergistic, and decomposable into direct and indirect effects.
- **Closest use case:** Foundational method reference for later cross-model / pre-post studies.
- **Link:** https://proceedings.neurips.cc/paper/2020/hash/92650b2e92217715fe312e6fa7b90d82-Abstract.html
- **Code:** https://github.com/sebastianGehrmann/CausalMediationAnalysis

### 7) Testing Pre-trained Language Models' Understanding of Distributivity via Causal Mediation Analysis
- **Authors:** Pangbo Ban; Yifan Jiang; Tianran Liu; Shane Steinert-Threlkeld
- **Venue:** BlackboxNLP 2022 / arXiv:2209.04761
- **Why it matters for your question:** Good example of using CMA to compare **where** a capability lives across models and layers, even without fine-tuning.
- **What it studied:** Whether pre-trained LMs understand distributivity, how that ability correlates with model size, and where the mediating knowledge is concentrated across layers.
- **Closest use case:** Cross-model comparison of mediated semantic competence.
- **Link:** https://aclanthology.org/2022.blackboxnlp-1.26/
- **Code:** https://github.com/1fanj/CMA-distributivity

### 8) A Mechanistic Interpretation of Arithmetic Reasoning in Language Models using Causal Mediation Analysis
- **Authors:** Alessandro Stolfo; Yonatan Belinkov; Mrinmaya Sachan
- **Venue:** EMNLP 2023 / arXiv:2305.15054
- **Why it matters for your question:** Strong example of using CMA to recover the internal pathway for a specific reasoning skill.
- **What it studied:** How arithmetic information flows through attention and MLP modules during next-token prediction, identifying the mechanism that carries number information and result computation.
- **Closest use case:** Mechanism tracing for capability emergence and comparison across model components.
- **Link:** https://aclanthology.org/2023.emnlp-main.435/
- **Code:** https://github.com/alestolfo/lm-arithmetic

### 9) CMA-R: Causal Mediation Analysis for Explaining Rumour Detection
- **Authors:** Lin Tian; Xiuzhen Zhang; Jey Han Lau
- **Venue:** Findings of EACL 2024 / arXiv:2402.08155
- **Why it matters for your question:** Demonstrates that CMA is useful not only for LM internals but also for **explaining downstream neural decisions** via mediating posts, words, and evidence spans.
- **What it studied:** Which tweets and words causally mediate rumour detection predictions, with reported agreement with human explanations.
- **Closest use case:** Downstream-task mediator discovery.
- **Link:** https://aclanthology.org/2024.findings-eacl.116/
- **Code:** https://github.com/ltian678/cma-r

## 5) Recent CMA-like work for intervention and steering in generative LMs

### 10) Activation Steering in Generative Settings via Contrastive Causal Mediation Analysis
- **Authors:** Aruna Sankaranarayanan; Amir Zur; Atticus Geiger; Dylan Hadfield-Menell
- **Venue:** ICML 2025 Actionable Interpretability Workshop / NeurIPS 2025 Mech-Interp Workshop
- **Why it matters for your question:** This paper uses a contrastive causal mediation view to identify **which attention heads to steer** for a target behavior.
- **What it studied:** Behavior steering for refusal, sycophancy, and style transfer in generative LMs by ranking mediating attention heads.
- **Closest use case:** Post-training intervention based on mediator discovery.
- **Link:** https://openreview.net/forum?id=bUXa74EiOL

### 11) Surgical Activation Steering via Generative Causal Mediation
- **Authors:** Aruna Sankaranarayanan; Amir Zur; Atticus Geiger; Dylan Hadfield-Menell
- **Venue:** arXiv preprint, 2026
- **Why it matters for your question:** Extends mediation-guided steering to long-form generation with a more explicit generative mediation framework.
- **What it studied:** Sparse attention-head selection for steering refusal, sycophancy, and style-related behavior in generative models.
- **Closest use case:** Causal mediator discovery for targeted post-training control rather than full re-finetuning.
- **Link:** https://arxiv.org/abs/2602.16080

## 6) Methodology / framing reference

### 12) The Quest for the Right Mediator: Surveying Mechanistic Interpretability Through the Lens of Causal Mediation Analysis
- **Authors:** Aaron Mueller; Jannik Brinkmann; Millicent Li; Samuel Marks; Koyena Pal; Nikhil Prakash; Can Rager; Aruna Sankaranarayanan; Arnab Sen Sharma; Jiuding Sun; Eric Todd; David Bau; Yonatan Belinkov
- **Venue:** arXiv preprint, 2024
- **Why it matters for your question:** Useful as a framing paper for deciding **what should count as the mediator** in a cross-model fine-tuning study: neurons, heads, layers, residual directions, features, or circuits.
- **What it studied:** A unifying survey of mechanistic interpretability through the lens of causal mediation analysis.
- **Closest use case:** Design guidance for your own cross-model CMA pipeline.
- **Link:** https://arxiv.org/abs/2408.01416

---

## Practical takeaway for your project

If your concrete goal is:

> Compare a base model and its fine-tuned variants to understand what changed mechanistically

then CMA can support a very reasonable analysis pipeline:

1. **Choose a shared mediator definition**
   - attention heads,
   - MLP blocks,
   - residual stream directions,
   - or discovered sparse features.

2. **Evaluate the same behavior on the same prompt set**
   - task success,
   - bias/toxicity,
   - refusal,
   - hallucination,
   - or reasoning.

3. **Measure causal effects in each checkpoint**
   - TE / NIE / NDE,
   - top-k mediator rankings,
   - per-layer concentration.

4. **Compare checkpoints directly**
   - mediator overlap,
   - new vs. inherited mediators,
   - effect amplification / suppression,
   - utility-side-effect coupling.

5. **Optionally intervene**
   - selective ablation,
   - activation patching,
   - targeted PEFT,
   - or steering based on discovered mediators.

### Clean research questions enabled by this setup

- Does fine-tuning **reuse** pre-trained mediators or create new ones?
- Does SFT improve performance by strengthening **indirect reasoning pathways**, or by introducing a brittle shortcut?
- Which mediators support the target task but also mediate **bias or refusal side effects**?
- Are LoRA / adapters more **localized** than full fine-tuning in causal terms?
- Are the same mediators preserved across model sizes or families?

### Bottom-line conclusion

The literature says:

- **Yes, CMA-like methods have already been used in closely related ways.**
- The strongest existing evidence is in **bias/debiasing**, **semantic/arithmetical mechanism discovery**, **annotation-bias diagnosis**, and **steering/intervention**.
- A more explicit **cross-model, checkpoint-to-checkpoint CMA framework for pre/post fine-tuning analysis** still looks like a good and not-yet-saturated research angle.
