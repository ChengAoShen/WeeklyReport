---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Oct 18, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Previous Report & New Idea

ChengAo Shen
Oct 18, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Previous Report
- New Idea

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. Previous Report

---
<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Previous Report**  *New Idea* -->
## 1.1 Ranking comparison

| Datasets | baseline(PC) | LLMs domain knowledge+Log data | Zach Code |
| :------: | :----------: | :----------------------------: | :-------: |
| 20210517 |      1       |               /                |     1     |
| 20210524 |      1       |               /                |     1     |
| 20211203 |      2       |                                |     1     |
| 20220606 |      14      |               19               |    18     |

- The first three datasets need more room for improvement.
- **The Graph has little use in the SPOT + random walk work framework.**
  - The ranking is almost completely controlled by the abnormal score at the beginning
  - The root cause is even not in the chosen node.

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Previous Report**  *New Idea* -->
## 1.2 Question Example

- **The Graph has little use in the SPOT + random walk work framework.**

Example: 20210517(Ground truth:catalogue-8667bb6cbc-hqzfw)

![causal_graph_0517](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/causal_graph_0517.png)

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. New Idea

---
<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Previous Report*  **New Idea** -->
## 2.1 Background

**Motivation**: Only a small portion of the dataset contains both graph and log data, making it difficult to conduct sufficient experiments.

**Idea**: Keep the basic workflow unchanged and reduce the importance of log data. But instead, it introduces other new modalities from the internet.

---
<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***Report*** *Previous Report*  **New Idea** -->

## 2.2 Work Flow

1. Perform causal discovery to get the initial graph
2. Query internet for dataset and node info
3. Use Domain Knowledge LLMs to understand causal relationships
4. Run constraint LLMs for uncertainty judgments
5. Apply uncertainty wrapper to filter relationships
6. Run PC algorithm with constraints as background knowledge

![workflow](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/workflow.svg)

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Previous Report*  **New Idea** -->
## 2.3 Future Work

**Ablation experiment**

1. Remove the first round of PC algorithm, using LLM agent directly
2. Exclude online-queried information
3. Remove Uncertainty wrapper, only use the constain LLMs results
4. Remove finally PC algorithm, using prior knowledge as result(While Remove uncertainty wrapper)

**Question**

1. Should we directly incorporate the queried information as part of the prompt, or use it as RAG for querying? How can we utilize the agent's knowledge, and how can we query online content to supplement information?
2. When performing probability sampling, how should we set the temperature? How should we set the threshold?

---
![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/QQ_1729176204281.png)
