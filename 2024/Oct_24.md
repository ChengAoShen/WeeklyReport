---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Oct 24, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Experiment Report & Model Refinement

ChengAo Shen
Oct 25, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Experiment Report
- Model Refinement

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Experiment report

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Report**  *Model Refinement* -->
### 1.1 Model Comparison

**Auto_MPG**

|             Methods             | FPR  | FNR  | Precision |  F1  | SHD  |
| :-----------------------------: | :--: | :--: | :-------: | :--: | :--: |
|          Baseline(PC)           | 0.4  | 0.8  |   0.11    | 0.14 |  8   |
|      PC + Constrain agent       | 0.15 | 0.2  |   0.57    | 0.66 |  3   |
| PC + Web agent+ Constrain agent | 0.1  | 0.2  |   0.66    | 0.72 |  2   |

Multi-Agent Causality Framework best score: SHD(4)

**DWD_climate**

|             Methods              | FPR  | FNR  | Precision |  F1  | SHD  |
| :------------------------------: | :--: | :--: | :-------: | :--: | :--: |
|           Baseline(PC)           | 0.2  | 0.83 |   0.14    | 0.15 |  9   |
|       PC + Constrain agent       | 0.03 | 0.83 |    0.5    | 0.25 |  6   |
| PC + Web agent + Constrain agent | 0.03 | 0.66 |   0.66    | 0.44 |  5   |

Multi-Agent Causality Framework best score: SHD(5)

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Report**  *Model Refinement* -->

### 1.1 Model Comparison

**Sachs**

|             Methods              | FPR  | FNR  | Precision |  F1  | SHD  |
| :------------------------------: | :--: | :--: | :-------: | :--: | :--: |
|           Baseline(PC)           | 0.15 | 0.47 |   0.38    | 0.44 |  24  |
|       PC + Constrain agent       | 0.12 | 0.94 |   0.07    | 0.06 |  26  |
| PC + Web agent + Constrain agent | 0.04 | 0.84 |   0.38    | 0.22 |  17  |

Multi-Agent Causality Framework best score: SHD(18)

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Report**  *Model Refinement* -->
### 1.2. Model Strengths & Weaknesses

- **Strengths**: Using the simple framework to achieve SOTA performance and using fewer resources than others.
- **Weaknesses**:
  - The use of LLMs is too isolated to be considered a complete agent.
  - Using web search to obtain domain knowledge, the model structure may not be stable at different periods.

---

<!-- _class: navbar cols-2 -->
<!-- _header: \ ***Weekly Report*** **Experiment Report**  *Model Refinement* -->
### 1.3. Model Details

<div class="limg">

![new_workflow](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/new_workflow.svg)

</div>

<div class="rdiv">
Information LLMs pattern

- Default: Please provide detailed information of dataset {dataset} and its variables, including {', '.join(labels)}.
- More info inquire: Please provide detailed information of dataset {dataset} and its variables, including {', '.join(labels)}. Try your best to search more web information if possible. Then please provide more detailed information if possible.
- Less info inquire: Please provide a summary of dataset {dataset} and its variables.

</div>

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Model Refinement

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment Report*  **Model Refinement** -->

### 2.1. Problems

- **Question**: The reason for choosing these patterns is that some datasets are not well-known when searching on Google, and more relaxed restrictions are needed to obtain web pages. But this content is also input to LLMs, which can easily lead to LLMs being unable to output well.
  - Adding an LLM earlier to summarize what key should be queried ->This can also form an agent
- **Question**: Our framework is not a traditional LLM agent(which contains Memory, Planning, Action, etc., like humans.)
  - LLM can be used to determine which causal algorithm to use, and adding this section is more convincing
  - Using the output from **domain knowledge LLMs** as memory.
  - Consider Domain knowledge LLMs and Constrain generation LLMs as a cooperative agent

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment Report*  **Model Refinement** -->

### 2.1. Problems

- **Question**: Using web search to obtain domain knowledge, the model structure may not be stable at different periods.
- **Question**: Uncertainty estimation does not affect the performance even if the temperature is really high. Whatever the number we inquire about, they will continuously give the same answers.
  - Remove this part.
  - Using debate agent methods. One positive constrains generation LLMs, one negative constrains generation LLMs to debate about the output from domain knowledge, and let other decision-makers LLMs generate yes/no.

---

### 2.2. Others

1. 能不能多次循环优化图？（将前面已经得到的Prompt也放进去作为一部分）
2. introduction里面多阐述一些对于agent和传统算法的不同，表达出agent更加符合人类思维，可以更加理解因果关系而不是暑假的相关性
3. 记得最后要说明自己没有去网上查询真实的Graph来使用。
