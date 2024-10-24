---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Oct 25, 2024
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
- Method details
- Weakness & Refinement

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Experiment report

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Experiment Report** *Method Details*  *Weakness&Refinement* -->
### 1.1 Model Comparison(Auto_MPG)

|             Methods             | FPR  | FNR  | Precision |  F1  | SHD  |
| :-----------------------------: | :--: | :--: | :-------: | :--: | :--: |
|          Baseline(PC)           | 0.4  | 0.8  |   0.11    | 0.14 |  8   |
|      PC + Constrain agent       | 0.15 | 0.2  |   0.57    | 0.66 |  3   |
| PC + Web agent+ Constrain agent | 0.1  | 0.2  |   0.66    | 0.72 |  2   |

Multi-Agent Causality Framework best score: SHD(4)

---
<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Experiment Report** *Method Details*  *Weakness&Refinement* -->
### 1.2 Model Comparison(DWD_climate)

|             Methods              | FPR  | FNR  | Precision |  F1  | SHD  |
| :------------------------------: | :--: | :--: | :-------: | :--: | :--: |
|           Baseline(PC)           | 0.2  | 0.83 |   0.14    | 0.15 |  9   |
|       PC + Constrain agent       | 0.03 | 0.83 |    0.5    | 0.25 |  6   |
| PC + Web agent + Constrain agent | 0.03 | 0.66 |   0.66    | 0.44 |  5   |

Multi-Agent Causality Framework best score: SHD(5)

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Experiment Report** *Method Details*  *Weakness&Refinement* -->

### 1.3 Model Comparison(Sachs)

|             Methods              | FPR  | FNR  | Precision |  F1  | SHD  |
| :------------------------------: | :--: | :--: | :-------: | :--: | :--: |
|           Baseline(PC)           | 0.15 | 0.47 |   0.38    | 0.44 |  24  |
|       PC + Constrain agent       | 0.12 | 0.94 |   0.07    | 0.06 |  26  |
| PC + Web agent + Constrain agent | 0.04 | 0.84 |   0.38    | 0.22 |  17  |

Multi-Agent Causality Framework best score: SHD(18)

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Method details

---

<!-- _class: navbar cols-2 -->
<!-- _header: \ ***Report*** *Experiment Report* **Method Details** *Weakness&Refinement* -->
### 2.1 Model Details

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

<!-- _class: navbar -->
<!-- _header: \ ***Report*** *Experiment Report* **Method Details** *Weakness&Refinement* -->
### 2.2 Strengths & Weaknesses

- **Strengths**:
  - Model introduce Web information to improve the performance of causal discovery by searching more domain knowledge.
  - Using the simple framework to achieve SOTA performance and using fewer resources than others.
- **Weaknesses**:
  - The Web LLMs need choose the prompt by user due to the query process.
  - The use of LLMs is too isolated to be considered a complete agent.
  - Using web search to obtain domain knowledge, the model structure may not be stable at different periods.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 3. Weakness & Refinement

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Experiment Report* *Method Details* **Weakness&Refinement** -->
### 2.1 Web Search Agent

- **Weakness**: The Web LLMs need choose the prompt by user due to the query process.
![Web Search Agent](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Web%20Search%20Agent.svg)

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Experiment Report* *Method Details* **Weakness&Refinement** -->
### 2.2 Constrain Debating Agent

![Constrain Debating Agent](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Constrain%20Debating%20Agent.svg)

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Experiment Report* *Method Details* **Weakness&Refinement** -->
### 2.3 Offline Domain Knowledge Provider

- **Weakness**: Model may be unstable due to the change of domain knowledge from web search, offline version is needed.
![offline](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/new.svg)

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Experiment Report* *Method Details* **Weakness&Refinement** -->

### 2.4 Overall Workflow

![Work_flowOCT24](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Work_flowOCT24.svg)