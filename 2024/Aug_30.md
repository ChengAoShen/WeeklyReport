---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Aug 23, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Experiment Results and Workflow Discussion

ChengAo Shen
Aug 30, 2024

---
<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Experiment Results
- Ideas Discussion
- Future Work

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. Experiment Results

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Results** *Workflow Discussion* *Future Work* -->

### 1.1 Experiment Methods

- PC_algorithms: Only use PC algorithm to generate causal graph.
- Llama_predicted: Only use Llama3.1 to predict the causal graph.
- Llama_optimized: Using PC algorithm to generate causal graph, then using Llama3.1 to optimize.
- GPT_optimized: Using PC algorithm to generate causal graph, then using GPT-4o-mini to optimize.
- GPT_optimized (only yes): Only accept the edge when LLMs give yes.
- GPT_optimized(only yes+ likelihood 0.5): Leting LLMs to predict the likelihood of each edge, and only accept the edge when the likelihood is greater than 0.5.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Results** *Workflow Discussion* *Future Work* -->

### 1.2 Auto MGP

|                  Methods                | Precision | FNR  | FPR  | F1 Score | SHD  |
| :-------------------------------------: | :-------: | :--: | :--: | :------: | :--: |
|              PC_algorithms              |   0.11    | 0.8  | 0.4  |   0.14   |  8   |
|             Llama_predicted             |   0.08    | 0.8  | 0.55 |   0.11   |  11  |
|             Llama_optimized             | **0.25**  | 0.6  | 0.3  |   0.30   |  6   |
|              GPT_optimized              | **0.50**  | 0.2  | 0.2  |   0.61   |  4   |
|        GPT_optimized (only yes)         |   0.55    | 0.0  | 0.2  |   0.71   |  3   |
| GPT_optimized(only yes+ likelihood 0.5) |   0.50    | 0.2  | 0.2  |   0.61   |  4   |

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Results** *Workflow Discussion* *Future Work* -->

### 1.3 DWD Climate

|               Methods                   | Precision | FNR  | FPR  | F1 Score | SHD  |
| :-------------------------------------: | :-------: | :--: | :--: | :------: | :--: |
|              PC_algorithms              |   0.14    | 0.8  | 0.2  |   0.15   |  9   |
|             Llama_predicted             |    0.0    | 1.0  | 0.4  |   0.0    |  14  |
|             Llama_optimized             |    0.0    | 1.0  | 0.16 |   0.0    |  8   |
|              GPT_optimized              |  **0.5**  | 0.8  | 0.03 |   0.25   |  6   |
|        GPT_optimized (only yes)         |  **0.25** | 0.66 | 0.2  |   0.28   |  8   |
| GPT_optimized(only yes+ likelihood 0.5) |   0.28    | 0.66 | 0.16 |   0.30   |  8   |

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Results** *Workflow Discussion* *Future Work* -->

### 1.4 Sachs

|              Methods                    | Precision | FNR  | FPR  | F1 Score | SHD  |
| :-------------------------------------: | :-------: | :--: | :--: | :------: | :--: |
|              PC_algorithms              | **0.38**  | 0.47 | 0.15 |   0.44   |  24  |
|             Llama_predicted             |   0.12    | 0.63 | 0.46 |   0.19   |  49  |
|             Llama_optimized             |   0.16    | 0.78 | 0.19 |   0.18   |  29  |
|              GPT_optimized              |    0.2    | 0.89 | 0.07 |   0.13   |  22  |
|        GPT_optimized (only yes)         |   0.16    | 0.78 | 0.20 |   0.18   |  29  |
| GPT_optimized(only yes+ likelihood 0.5) |   0.14    | 0.78 | 0.22 |   0.17   |  29  |

> This dataset is about biology, which may have no domain knowledge in LLMs.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. Paper Reading

---

<!-- _class: navbar cols-2 -->
<!-- _header: \ ***Weekly Report*** *Experiment Results* **Workflow Discussion** *Future Work* -->

### 2.1 Workflow 1

<div class="limg">

![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/workflow1(1).svg)

</div>

<div class="rdiv">

1. Using PC algorithm to generate original causal graph and using LLMs to optimize the causal graph.
2. Using causal graph to obtain the root cause
3. Using uncertainty estimation LLMs with Log datasets to get the uncertainty of graph(root cause)
4. Output: root cause, uncertainty

</div>

---

<!-- _class: navbar cols-2-->
<!-- _header: \ ***Weekly Report*** *Experiment Results* **Workflow Discussion** *Future Work* -->
### 2.2 Workflow 2

<div class="limg">

![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/workflow2.svg)

</div>

<div class="rdiv">

1. Using PC algorithm to generate original causal graph.
2. Using donmain knowledge LLMs to generate the domain knowledge.
3. Multiple judgments on whether edges should exist and calculate the likelihood of each edge.
4. Using likelihood as constrain to optimize the causal graph.

</div>

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment Results* **Workflow Discussion** *Future Work* -->
### 2.3 Question

1. How to use the log dataset?
    - as RAG
    - as Prompt after summarized

2. The aim of uncertainty estimation?
    - obtimize the causal graph
    - as results?

3. the uncertainty is about what?
    - the correctness of the edge
    - the stability of the graph / root cause?

4. Other datasets contain log data?

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 3. Future Work

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment Results* *Workflow Discussion* **Future Work** -->

### 3.1 Future Work

1. Familiar with RAG
2. check whether the new causal discovery algorithm can use likelihood as a constrain
3. Think about how to summary log data
