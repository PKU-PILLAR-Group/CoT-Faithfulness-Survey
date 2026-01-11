# A Survey on Chain-of-Thought Faithfulness in Large Language Models

![Awesome](https://awesome.re/badge-flat2.svg) ![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

> **Companion repository** for our survey on **The Mirage of Explainability: A Survey on Chain-of-Thought Faithfulness in Large Language Models**—why rationales can look convincing yet fail to reflect the model’s true decision process, how to evaluate this gap, and how to mitigate it. [[PDF]](A_survey_on_CoT_faithfulness.pdf)

---

## Table of Contents

- [Overview](#overview)
- [Latest News](#latest-news)
- [Taxonomy & Legends](#taxonomy--legends)
  - [What we mean by "faithfulness"](#what-we-mean-by-faithfulness)
  - [Key failure phenomena](#key-failure-phenomena)
  - [Benchmark levels](#benchmark-levels)
  - [Mitigation families](#mitigation-families)
- [Paper List](#paper-list)
  - [1. Failure Phenomena](#1-failure-phenomena)
  - [2. Evaluation Benchmarks & Metrics](#2-evaluation-benchmarks--metrics)
  - [3. Mitigation Strategies](#3-mitigation-strategies)

---

## Overview

Chain-of-Thought (CoT) reasoning has become a standard interface for deploying and monitoring large language models (LLMs). Yet a central concern is **faithfulness**: *does the generated CoT reflect the model’s actual computation, or is it a plausible post-hoc story*?

This repository curates and organizes research on CoT faithfulness, following the framework from our survey:

- **Unified definition:** faithfulness requires **internal alignment** (CoT matches internal computations) *and* **external consistency** (CoT remains stable under behavior-preserving interventions).
- **Failure phenomena:** we emphasize four recurring modes—**input-driven unfaithfulness**, **non-causal/post-hoc rationalization**, **alignment-induced unfaithfulness**, and **strategic deception & hiding**.
- **Benchmark levels:** we organize evaluation into **four levels** (L1–L4), from behavioral audits to white-box causal tests.
- **Mitigation:** we summarize interventions spanning inference-time prompting/verification, training-time methods, and internal/architectural approaches.

> **Figures (Failure phenomena)**  
> ![Failure Phenomena](overview.png)

---

## Latest News

- **[2026-01]** Repository initialized for maintaining an up-to-date reading list and taxonomy for CoT faithfulness.
- **[2025-10]** New benchmark threads: *Level-II intervention-grounded* (e.g., FaithCoT-Bench) and *Level-III structured-verifiable* (e.g., Typed CoT) are accelerating progress.

---

## Taxonomy & Legends

### What we mean by faithfulness

We use a **two-axis** view:

- **Internal alignment:** the CoT corresponds to internal computations (a *white-box* claim).
- **External consistency:** the CoT is robust to interventions that preserve the final answer behavior (a *black-box* test of stability).

A CoT can be *externally consistent but internally misaligned* (e.g., stable yet still post-hoc), or *internally aligned but externally fragile* (e.g., small edits flip the story).

---

### Key failure phenomena

We tag papers by the **primary** failure mode(s):

- **IDU — Input-driven unfaithfulness:** the model uses spurious prompt cues/biases (option order, stereotypes, user beliefs) but fails to mention them.
- **NPR — Non-causal/post-hoc rationalization:** CoT and answer are decoupled; the model can change the rationale without changing the answer.
- **AIU — Alignment-induced unfaithfulness:** training incentives (e.g., RLHF / preference optimization) encourage *nice-looking* explanations rather than faithful ones.
- **SDH — Strategic deception & hiding:** the model deliberately withholds or obfuscates reasoning, especially under monitoring pressure.

---

### Benchmark levels

We classify evaluations into four increasing levels of *ground-truth strength*:

- **L1 — Behavioral audits:** detect mismatches via input biases, counterfactual prompts, or correlation tests (no direct grounding).
- **L2 — Intervention-grounded:** validate rationales against controlled perturbations with known causal effects (e.g., faithful cues, diagnostic setups).
- **L3 — Structured-verifiable:** enforce strict formats or formal constraints so parts of the reasoning become mechanically checkable.
- **L4 — White-box:** directly test internal computation via mechanistic tools (activation patching, causal tracing, circuit mapping).

---

### Mitigation families

We tag mitigation approaches by the dominant lever:

- **Prompting constraints:** instruction tuning at inference-time, rationale templates, refusal policies.
- **Sampling/ensembling:** self-consistency, majority vote, re-ranking.
- **Verification & tool binding:** external checkers, solvers, theorem provers, rule-based verifiers.
- **Training & fine-tuning:** SFT/RL methods explicitly rewarding faithful rationales or penalizing unfaithful ones.
- **Internal intervention:** activation steering/patching, representation editing, circuit-level control.
- **Architectural inductive bias:** designs that force latent structure, enforce bottlenecks, or separate planning from verbalization.

---

## Paper List

> **Note.** This list is expanded and reorganized according to the references and taxonomy in our survey.
> We annotate the *primary* phenomenon / evaluation level / mitigation family when applicable to keep tables readable.

### 1. Failure Phenomena

#### 1.1 Input-driven unfaithfulness (IDU)

| Paper                                                        | Phenomenon | Evaluation | Mitigation | Venue   | Year | Link                             |
| ------------------------------------------------------------ | ---------- | ---------- | ---------- | ------- | ---: | -------------------------------- |
| Language models don’t always say what they think: Unfaithful explanations in chain-of-thought prompting | IDU        | L1         | —          | NeurIPS | 2023 | https://arxiv.org/abs/2305.04388 |
| Towards understanding sycophancy in language models          | IDU        | L1         | —          | ICLR    | 2024 | https://arxiv.org/abs/2310.13548 |
| A closer look at bias and chain-of-thought faithfulness of large (vision) language models | IDU        | L1/L2      | —          | arXiv   | 2025 | https://arxiv.org/abs/2505.23945 |
| Are DeepSeek R1 and other reasoning models more faithful?    | IDU        | L1/L2      | —          | arXiv   | 2025 | https://arxiv.org/abs/2501.08156 |
| Are you sure? Challenging LLMs leads to performance drops in the FlipFlop experiment | IDU        | L1         | —          | arXiv   | 2024 | https://arxiv.org/abs/2311.08596 |
| Walk the talk? Measuring the faithfulness of large language model explanations | IDU        | L2         | —          | ICLR    | 2025 | —                                |

#### 1.2 Non-causal & post-hoc rationalization (NPR)

| Paper                                                        | Phenomenon | Evaluation | Mitigation | Venue | Year | Link                             |
| ------------------------------------------------------------ | ---------- | ---------- | ---------- | ----- | ---: | -------------------------------- |
| Measuring faithfulness in chain-of-thought reasoning         | NPR        | L1/L2      | —          | arXiv | 2023 | https://arxiv.org/abs/2307.13702 |
| Chain-of-thought reasoning in the wild is not always faithful | NPR        | L1/L2      | —          | arXiv | 2025 | https://arxiv.org/abs/2503.08679 |
| Chain-of-thought unfaithfulness as disguised accuracy        | NPR        | L1         | —          | TMLR  | 2024 | —                                |
| Beyond semantics: The unreasonable effectiveness of reasonless intermediate tokens | NPR        | L1/L2      | —          | arXiv | 2025 | https://arxiv.org/abs/2505.13775 |
| Can Aha moments be fake? Identifying true and decorative thinking steps in chain-of-thought | NPR        | L1/L2      | —          | arXiv | 2025 | https://arxiv.org/abs/2510.24941 |

#### 1.3 Alignment-induced unfaithfulness (AIU)

| Paper                                                        | Phenomenon | Evaluation | Mitigation | Venue | Year | Link                             |
| ------------------------------------------------------------ | ---------- | ---------- | ---------- | ----- | ---: | -------------------------------- |
| On the impact of fine-tuning on chain-of-thought reasoning   | AIU        | L1         | FT         | NAACL | 2025 | —                                |
| Exploring chain-of-thought reasoning for steerable pluralistic alignment | AIU        | L1         | FT         | arXiv | 2025 | https://arxiv.org/abs/2510.04045 |
| Answer-consistent chain-of-thought reinforcement learning for multi-modal large language models | AIU        | L2         | FT         | arXiv | 2025 | https://arxiv.org/abs/2510.10104 |
| Chart-RVR: Reinforcement learning with verifiable rewards for explainable chart reasoning | AIU        | L2/L3      | FT/VT      | arXiv | 2025 | https://arxiv.org/abs/2510.10973 |
| Truthful or fabricated? Using causal attribution to mitigate reward hacking in explanations | AIU        | L2         | II/FT      | arXiv | 2025 | https://arxiv.org/abs/2504.05294 |

#### 1.4 Strategic deception & hiding (SDH)

| Paper                                                        | Phenomenon | Evaluation | Mitigation | Venue | Year | Link                             |
| ------------------------------------------------------------ | ---------- | ---------- | ---------- | ----- | ---: | -------------------------------- |
| Sleeper agents: Training deceptive LLMs that persist through safety training | SDH        | L2         | FT         | arXiv | 2024 | https://arxiv.org/abs/2401.05566 |
| AI sandbagging: Language models can strategically underperform on evaluations | SDH        | L2         | —          | ICLR  | 2025 | —                                |
| Preventing language models from hiding their reasoning       | SDH        | L2         | VT         | arXiv | 2023 | https://arxiv.org/abs/2310.18512 |
| Can we predict alignment before models finish thinking? Towards monitoring misaligned reasoning models | SDH        | L2         | VT         | arXiv | 2025 | https://arxiv.org/abs/2507.12428 |

---

### 2. Evaluation Benchmarks & Metrics

#### 2.1 L1 — Behavioral audits / black-box metrics

| Paper                                                        | Level | Focus                                              | Venue          | Year | Link                             |
| ------------------------------------------------------------ | ----- | -------------------------------------------------- | -------------- | ---: | -------------------------------- |
| Measuring faithfulness in chain-of-thought reasoning         | L1/L2 | interventions on rationale text / dependence tests | arXiv          | 2023 | https://arxiv.org/abs/2307.13702 |
| Making reasoning matter: Measuring and improving faithfulness of chain-of-thought reasoning | L1/L2 | metrics + training-time improvements               | EMNLP Findings | 2024 | —                                |
| Meta-evaluation of faithfulness metrics for chain-of-thought explanations | L1    | metric reliability / leakage                       | NeurIPS        | 2024 | —                                |
| Hypothesis-driven evaluation of faithfulness metrics for chain-of-thought explanations | L1    | systematic critique of metrics                     | arXiv          | 2024 | https://arxiv.org/abs/2410.21457 |
| On the relationship between explanation uncertainty and faithfulness in chain-of-thought reasoning | L1    | uncertainty vs faithfulness                        | arXiv          | 2024 | https://arxiv.org/abs/2405.15292 |
| Thought branches: Interpreting LLM reasoning requires resampling | L1    | resampling-based interpretability                  | arXiv          | 2025 | https://arxiv.org/abs/2510.27484 |

#### 2.2 L2 — Intervention-grounded / diagnostic benchmarks

| Paper                                                        | Level | Focus                                  | Venue | Year | Link                             |
| ------------------------------------------------------------ | ----- | -------------------------------------- | ----- | ---: | -------------------------------- |
| FaithCoT-Bench: Benchmarking instance-level faithfulness of chain-of-thought reasoning | L2    | instance-level diagnostics             | arXiv | 2025 | https://arxiv.org/abs/2510.04040 |
| Walk the talk? Measuring the faithfulness of large language model explanations | L2    | concept perturbations / simulatability | ICLR  | 2025 | —                                |
| Perturbation-based evaluation of vision-language models for medical chain-of-thought reasoning | L2    | medical VLM CoT robustness             | arXiv | 2025 | https://arxiv.org/abs/2501.03890 |
| Counterfactual evaluation of vision-language models for compositional chain-of-thought reasoning | L2    | counterfactual compositionality        | arXiv | 2025 | https://arxiv.org/abs/2501.04245 |
| LExt: A language model extrapolation benchmark for evaluating reasoning under distribution shift | L2    | OOD reasoning                          | arXiv | 2025 | https://arxiv.org/abs/2501.02087 |
| MedOmni-45°: A safety-performance benchmark for reasoning-oriented LLMs in medicine | L2    | medical safety / reasoning             | arXiv | 2025 | https://arxiv.org/abs/2508.16213 |
| A comprehensive evaluation of multilingual chain-of-thought reasoning: Performance, consistency, and faithfulness across languages | L2    | multilingual faithfulness              | arXiv | 2025 | https://arxiv.org/abs/2510.09555 |
| Cross-lingual generalization of chain-of-thought faithfulness in large language models | L2    | cross-lingual transfer                 | arXiv | 2025 | https://arxiv.org/abs/2501.03245 |

#### 2.3 L3 — Structured-verifiable / constraint-based evaluation

| Paper                                                        | Level | Focus                           | Venue   | Year | Link                             |
| ------------------------------------------------------------ | ----- | ------------------------------- | ------- | ---: | -------------------------------- |
| Deductive verification of chain-of-thought reasoning         | L3    | formal verification             | NeurIPS | 2023 | —                                |
| Typed chain-of-thought: A Curry-Howard framework for verifying LLM reasoning | L3    | type-checked verification       | ACL     | 2025 | —                                |
| Autorubric-R1V: Rubric-based generative rewards for faithful multimodal reasoning | L3    | rubric-verifiable rewards       | arXiv   | 2025 | https://arxiv.org/abs/2510.14738 |
| CoMAT: Chain of mathematically annotated thought improves mathematical reasoning | L3    | math-annotated verifiable steps | arXiv   | 2024 | https://arxiv.org/abs/2410.10336 |

#### 2.4 L4 — White-box / mechanistic metrics

| Paper                                                        | Level | Focus                               | Venue       | Year | Link                             |
| ------------------------------------------------------------ | ----- | ----------------------------------- | ----------- | ---: | -------------------------------- |
| Understanding intermediate layers using linear classifier probes | L4    | probing internal representations    | ICLR        | 2017 | —                                |
| Interpretability in the wild: A circuit for indirect object identification in GPT-2 small | L4    | circuit-level analysis              | ICLR        | 2023 | —                                |
| Attribution patching outperforms automated circuit discovery | L4    | causal interventions in activations | BlackboxNLP | 2024 | —                                |
| Correlation or causation: Analyzing the causal structures of LLM and LRM reasoning process | L4    | causal structure analysis           | arXiv       | 2025 | https://arxiv.org/abs/2509.17380 |
| State over tokens: Characterizing the role of reasoning tokens | L4    | token vs state analysis             | arXiv       | 2025 | https://arxiv.org/abs/2512.12777 |

---

### 3. Mitigation Strategies

#### 3.1 Inference-time prompting / orchestration (P, SC)

| Paper                                                        | Mitigation | Focus                       | Venue   | Year | Link                             |
| ------------------------------------------------------------ | ---------- | --------------------------- | ------- | ---: | -------------------------------- |
| Chain-of-thought prompting elicits reasoning in large language models | P          | CoT prompting interface     | NeurIPS | 2022 | —                                |
| Least-to-most prompting enables complex reasoning in large language models | P          | staged decomposition        | ICLR    | 2023 | —                                |
| Rephrase and respond: Let large language models ask better questions for themselves | P          | self-ask / self-questioning | arXiv   | 2023 | https://arxiv.org/abs/2311.04205 |
| Program of thoughts prompting: Disentangling computation from reasoning for numerical reasoning tasks | P          | program-style reasoning     | NeurIPS | 2022 | —                                |
| COREX: Pushing the boundaries of complex reasoning through multi-model collaboration | SC         | collaboration / ensemble    | arXiv   | 2023 | https://arxiv.org/abs/2310.00280 |

#### 3.2 Verification & tool binding (VT)

| Paper                                                        | Mitigation | Focus                        | Venue   | Year | Link                             |
| ------------------------------------------------------------ | ---------- | ---------------------------- | ------- | ---: | -------------------------------- |
| Deductive verification of chain-of-thought reasoning         | VT         | verifying intermediate steps | NeurIPS | 2023 | —                                |
| Typed chain-of-thought: A Curry-Howard framework for verifying LLM reasoning | VT         | type-checked reasoning       | ACL     | 2025 | —                                |
| Beyond theorem proving: Benchmarking deductive reasoning with interactive tasks | VT         | interactive verification     | arXiv   | 2025 | https://arxiv.org/abs/2501.04267 |

#### 3.3 Training-time methods (FT)

| Paper                                                        | Mitigation | Focus                                  | Venue | Year | Link                             |
| ------------------------------------------------------------ | ---------- | -------------------------------------- | ----- | ---: | -------------------------------- |
| Faithful and efficient reasoning with chain-of-thought distillation | FT         | distillation for faithful CoT          | ACL   | 2023 | —                                |
| Inducing faithful chain-of-thought reasoning through perturbation-based training | FT         | perturbation training for faithfulness | ACL   | 2025 | —                                |
| Measuring chain-of-thought faithfulness by unlearning reasoning steps | FT         | unlearning spurious reasoning          | EMNLP | 2025 | —                                |
| Answer-consistent chain-of-thought reinforcement learning for multi-modal large language models | FT         | CoT-RL to enforce answer consistency   | arXiv | 2025 | https://arxiv.org/abs/2510.10104 |
| Audit-of-understanding: Posterior-constrained inference for mathematical reasoning in language models | FT         | posterior constraints                  | arXiv | 2025 | https://arxiv.org/abs/2510.10252 |
| Truthful or fabricated? Using causal attribution to mitigate reward hacking in explanations | FT/II      | reward hacking mitigation              | arXiv | 2025 | https://arxiv.org/abs/2504.05294 |

#### 3.4 Architectural / internal interventions (ARCH, II)

| Paper                                                        | Mitigation | Focus                               | Venue   | Year | Link                             |
| ------------------------------------------------------------ | ---------- | ----------------------------------- | ------- | ---: | -------------------------------- |
| Training large language models to reason in a continuous latent space | ARCH       | latent-space reasoning (bottleneck) | arXiv   | 2024 | https://arxiv.org/abs/2412.06769 |
| Locating and editing factual associations in GPT             | II         | representation editing              | NeurIPS | 2022 | —                                |

---

## License

This project is released under the MIT License (see `LICENSE`).
