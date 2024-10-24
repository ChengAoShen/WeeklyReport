---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Oct 4, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Encountered Problems

ChengAo Shen
Oct 4, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Experiment Results
- Experiment analysis
- Other question
---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. Experiment Results

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Results**  *Random Walk* *LLMs Output Issues* *Other question* -->

### 1.1 Basic Method

The ranking of root cause (MongoDB-v1) in the output of the model.

| Methods                                                      | Ranking |
| ------------------------------------------------------------ | ------- |
| Baseline (DirectLiNGAM+Original Random Walk)                 | 32      |
| DirectLiNGAM+Domain Knowledge LLMs+Original Random Walk      | 29      |
| DirectLiNGAM+Domain Knowledge LLMs+Log LLMs+Original Random Walk | 30      |

Using the new sampling strategy

| Methods                                                      | Ranking |
| ------------------------------------------------------------ | ------- |
| DirectLiNGAM+Domain Knowledge LLMs+Node sampling+Original Random Walk | 30      |
| DirectLiNGAM+Domain Knowledge LLMs+Edge sampling+Original Random Walk | 30      |

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Experiment Results**  *Random Walk* *LLMs Output Issues* *Other question* -->
### 1.2 Matrix Random Walk Strategy

| Methods                                                      | Ranking |
| ------------------------------------------------------------ | ------- |
| Baseline (DirectLiNGAM+Matrix Random Walk)                   | 43      |
| DirectLiNGAM+Domain Knowledge LLMs+Matrix Random Walk        | 42      |
| DirectLiNGAM+Domain Knowledge LLMs+Log LLMs+Matrix Random Walk | 42      |

---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Experiment analysis

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* **Random Walk** *LLMs Output Issues* *Other question* -->

### 2.1 Random walk

> Key Question: Random Walk is used to find the connection probability from the start node to others. But how to prove the node has a tight connection with the start node is the root cause?


How do we construct the transfer matrix in the directed graph?

   Example: The causal graph is `C->B->A, D->A`, where A is the KPI. During the random walk, the direction of all edges needs to be reversed. `A->B->C, A->D`.

   Using an adjacency matrix can be represented as

   ```python
   W=np.array([[0,0,0,0],
               [1,0,0,0],
               [0,1,0,0],
               [1,0,0,0]])              
   ```

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* **Random Walk** *LLMs Output Issues* *Other question* -->

### 2.1 Random walk

The iterative equation is $\mathbf{r}_i=c\mathbf{W}\mathbf{r}_i+(1-c)\mathbf{e}_i$. If we use the previous matrix, when $c$ approaches 1, all probabilities will approach 0. All probability will transmitted to the final node, and finally become zero. **The direct graph has no return edge.**

   > Key Question: Should we add the edge so that nodes can stay in their current place?
   >
   > ```python
   > W=np.array([[1,0,0,0],
   >             [1,1,0,0],
   >             [0,1,1,0],
   >             [1,0,0,1]])
   > ```

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* **Random Walk** *LLMs Output Issues* *Other question* -->
### 2.1 Random walk
2. The graph is totally controlled by the return probability $c$. When $c$ is near 1, the node is the final of the chain will be large, if $c$ is near to 0, the score of the node near with start is large.

   Example(After adding the edge to node self)

   | C     | Finally probability                                          |
   | ----- | ------------------------------------------------------------ |
   | 0.999 | [0.00149925, 0.0009975, 0.49825287, 0.49925037]              |
   | 0.5   | [0.6, 0.13333333, 0.06666667, 0.2]                           |
   | 0.001 | [9.99333111e-01, 3.33277676e-04, 1.66805644e-07, 3.33444481e-04] |

| c       | 0.1  | 0.2  | 0.3  | 0.4  | 0.5  | 0.6  | 0.7  | 0.8  | 0.9  |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Ranking of baseline| 40   | 40   | 38   | 37   | 35   | 39   | 40   | 40   | 43   |


---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* *Random Walk* **LLMs Output Issues** *Other question* -->
### 3.1 LLMs Output Issues

When using edge deletion (without modifying the prompt), it results in over 90% of the edges being deleted in the domain knowledge, as shown in the following image:

   <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202409272027549.png" alt="output" style="zoom: 30%;" />

   - -1 represents edges considered non-existent in the first graph creation
   - 0 indicates edges that the LLM believes should not exist
   - 1 represents edges that both the algorithm and LLM believe should exist

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* *Random Walk* **LLMs Output Issues** *Other question* -->

### 3.1 LLMs Output Issues

In the above situation, it leads to the inability to perform the second causal discovery (as almost all edges are specified as non-existent). Therefore, based on the logic that "yes" is more worth referencing, **the current approach is to ignore cases where LLMs believe edges do not exist**. Only retaining cases where LLMs believe edges exist, the domain knowledge is transformed into the following graph:

   <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202409272033701.png" alt="output" style="zoom:30%;" />

   However, this approach **easily leads to the LLMs' output having no effect at all, degenerating into a mode where only the causal discovery algorithm is performed.**

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 4. Other question

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* *Random Walk* *LLMs Output Issues* **Other question** -->

### 4.1 Other question

1. The problem of idea3/4

> The 1-hop neighbor of LPI: Add inquiring about KPI with others.
>
> After finishing step 3,  inquire about the node that has a connection with KPI with others.

   The current LLMs are optimized for graphs without KPIs to obtain prior knowledge. Without incorporating KPIs into the initial graph construction, only add KPI node when finally graph construction.

2. The likelihood can not be used as constraint. The ICML paer also use 1/0 as prior knowledge. Furthermore, the circle graph can not be used as prior knowledge. Currently, we need the prior knowledge formated as DAG rather than finally graph.


---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Experiment analysis* *Random Walk* *LLMs Output Issues* **Other question** -->
### 4.2 Ideas

1. Can we use the correlation between metrics to determine? If the correlation is too high and considered to be present, and if the correlation is too low and considered to be absent, leave the intermediate part to LLMs for processing?
2. Current we build the causal graph, but the LLMs noramlly will learn as the call graph. We need add the architecture of the system as the prompt, such as the node is in the same container.