---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Sep 27, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Encountered Problems

ChengAo Shen
Sep 27, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- LEMMA-Dataset
- Method
- Evaluation

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. LEMMA-Dataset

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **LEMMA-Dataset** *Method* *Evaluation* -->

### 1.1 Input&Output

- Dataset: The `20211203` dataset from the Product review system is used in the experiment.
- Input: The input includes CPU_Usage and KPI time series data for all Pods, as well as Log data (including template and structured parts) for all Pods.
- Output: Predict the Root Cause list for the KPI node (including possible Pod names and corresponding probabilities).

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **LEMMA-Dataset** *Method* *Evaluation* -->
### 1.2 Ground Truth

The current Ground Truth is obtained from the PPT.

<img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/QQ_1727352162283.png" alt="Root cause" style="zoom:25%;" />

> Question1: The Root Cause labeled in the PPT is `外部ストレージ` (External Storage), not a Pod. How should we select it? Should we choose `Mongodb-v1` which is connected to the external cause?

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. Method

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *LEMMA-Dataset* **Method** *Evaluation* -->
### 2.1 LLMs Optimizer

During the process of using LLM to optimize all pairs as originally planned, it was found that retaining 50 Pods after anomaly detection would lead to excessive time and cost, with a single domain knowledge query taking nearly 10 hours. Now it has been modified to only query the edges predicted in the first step of Causal Discovery, and only delete edges, not add them.

> Question2: Do we need to modify the original prompt? Should we instruct the LLM to delete edges instead of asking if there is a correlation?
>
> Question3: The results predicted by the model may not be a directed acyclic graph and cannot be used as prior knowledge for causal discovery. How should we handle this? Delete edges or perform other operations?

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *LEMMA-Dataset* **Method** *Evaluation* -->
### 2.2 Random Walk

- Input: Causal Graph, Anomaly item(KPI)
- Output: The probability of all pods being the root cause, except for the specified anomaly item

> Question4: Do we need to use Weight and the weights of the Graph?

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 3. Evaluation

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *LEMMA-Dataset* *Method* **Evaluation** -->
### 3.1 Formula

- **Precision@K**

$$
\mathrm{PR@K}=\frac{1}{|\mathbb{A}|} \sum_{a \in \mathbb{A}} \frac{\sum_{i<k}R_a(i)\in V_a}{\mathrm{min}(K,|v_a|)}
$$

Where $\mathbb{A}$ is the set of system faults, $a$ is a one fault, $V_a$ is the real root cause of $a$, $R_a$ is the predicted root cause of $a$ and $i$ is the $i-th$ predicted cause of $R_a$.

> Question5: In the current situation, we only use the `20211203` dataset which contains 1 system fault, thus $\mathbb{A}=1$. Furthermore, if only `mongodb-v1` is the ground truth, $V_a$ is `mongodb-v1`. Thus the equation will be:
> $$
> \mathrm{PR@K}=\sum_{i<k}R_a(i)\in V_a
> $$
> This will cause the PR@K to be 0 if the root cause (`mongodb-v1`) is not included in the top K predictions, and to 1 if it is included. There are only two situations. How to evaluate the performance of the model in this case? I believe that in this case, PR (Precision) has lost its meaning, and we can only evaluate the model using MRR (Mean Reciprocal Rank).
