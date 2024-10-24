---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Oct 11, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Project debriefing

ChengAo Shen
Oct 11, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Ranking comparison
- Question

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. Ranking comparison

---
## 1.1 Ranking comparison

<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Ranking comparison**  *Question* -->
| Datasets | Ground Truth         | Original(DirectLiNGAM+Random Walk c=0.4) | Original+LLMs | Original+LLMs+Log | New Codebase |
| -------- | -------------------- | ---------------------------------------- | ------------- | :---------------: | :-----------: |
| 0517     | Catalogue            | 13                                       | 12            |        12         |       1       |
| 0524     | Catalogue            | 4                                        | 4             |         4         |       1       |
| 1203     | mongodb              | 35                                       | 32            |        31         |       1       |
| 0606     | istio-ingressgateway | 26                                       | 25            |        26         |       1       |

---
<!-- _class: navbar-->
<!-- _header: \ ***Report*** **Ranking comparison**  *Question* -->
## 1.2 Analysis

1. The different between our original code and new code may be caused by the causal graph construction method(DirectLiNGAM vs GNN).
2. The new codebase has more parameter fine-tuning.

> I want to replace GNN with simpler algorithms like PC, and reduce the amount of fine-tuning to lower the Ranking performance, then use LLMs to improve it.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. Question

---

<!-- _class: navbar-->
<!-- _header: \ ***Report*** *Ranking comparison* **Question** -->
## 2.1 Question

1. The current codebase uses multi-metric fusion. Should we use multiple metrics in our final project, or continue to use only the `cpu_usage` metric?
2. During the code migration process, it was discovered that the algorithm's ranking is sensitive to the initial order of input pods.
3. LLMs are unable to provide domain knowledge for DAG, which will affect the final construction of the Causal Graph.
