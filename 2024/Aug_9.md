---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Aug 9, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Exploring the DYNOTRARS and Datasets

ChengAo Shen
Aug 9, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- DYNOTRARS
- Datasets
- LLMs in Causal Discovery
- Discussion

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. DYNOTRARS

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **DYNOTRAES** *Datasets* *LLMs in Causal Discovery* *Discussion* -->

### 1.1 Overview

DYNOTRARS revisits the structure learning problem for dynamic Bayesian networks and proposes a method that simultaneously estimates contemporaneous (intra-slice) and time-lagged (interslice) relationships between variables in a time series.

This is a Score-based Method. Revolves around minimizing a penalized loss subject to an acyclicity constraint.

<img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/image-20240808123254581.png" alt="Intra-slice and inter-slice" style="zoom:60%;" />

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **DYNOTRAES** *Datasets* *LLMs in Causal Discovery* *Discussion* -->

### 1.2 Method’s details

Consider $M$ independent time series, the $m$th time series given by $\{\mathbf{x}_{m,t}\}_{t\in\{0,\dots,T\}}$ for $\mathbf{x}_{m,t}\in \mathbb{R}^d$, where $d$ represent the number of variables.
$$
\mathbf{x}_{m,t}^\top=\mathbf{x}_{m,t}^\top\mathbf{W}+\mathbf{x}_{m,t-1}^\top\mathbf{A}_1+\dots+\mathbf{x}_{m,t-p}^\top\mathbf{A}_p+\mathbf{z}_{m,t}^\top
$$

- $\mathbf{W}$ is an acyclic matrix used to represent the intra-slice connection
- $\mathbf{A}_i$ used to represent $i$th ahead inter-slice connection.
- $p$ is *autoregressive order*.
- $\mathbf{z}_{m,t}^\top$ is a vector of centered error variables.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **DYNOTRAES** *Datasets* *LLMs in Causal Discovery* *Discussion* -->

### 1.2 Method’s details

Assuming that the network structure represented by $\mathbf{W},\mathbf{A}_i$ is constant across time, the previous equation can be written as follows:
$$
\mathbf{X}=\mathbf{X}\mathbf{W}+\mathbf{Y}_1\mathbf{A}_1+\dots+\mathbf{Y}_p\mathbf{A}_p+\mathbf{Z}
$$

- $\mathbf{X}$ is an $n\times d$ matrix whose rows are $\mathbf{x}_{m,t}^\top$ and $n=M(T+1-p)$.
- $\mathbf{Y}_1, \dots,Y_p$ are time-lagged versions of $\mathbf{X}$.

Finally, the equation can be represented  as:
$$
\mathbf{X}=\mathbf{X}\mathbf{W}+\mathbf{Y}\mathbf{A}+\mathbf{Z}
$$

- $\mathbf{Y}=[\mathbf{Y_1|\cdots|\mathbf{Y_p}}]$ is the $n\times pd$ matrix of time-lagged data.
- $\mathbf{A}=[\mathbf{a_1|\cdots|\mathbf{A_p}}]$ is the $pd\times d$ matrix of inter-slice weights.
- This representation can make the previous time-lagged not necessarily continuous

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **DYNOTRAES** *Datasets* *LLMs in Causal Discovery* *Discussion* -->
### 1.3 Optimization

DYNOTRAR is semi-parameters methods, which use $\mathbf{W}$ and $\mathbf{A}$ to represent the Dynamic Bayesian Network. Thus, the optimization problem is that given data $\mathbf{X,Y}$ to estimate weighted adjacency matrices. Specially, the $\mathbf{W}$ is acyclic.
$$
\mathop{\mathrm{min}}\limits_{\mathbf{W},\mathbf{A}}\quad \mathscr{l}(\mathbf{W},\mathbf{A}) \ \mathrm{s.t.}\  \mathbf{W}\ \mathrm{is\ acyclic},
$$
$$
\mathrm{where}\ \mathscr{l}(\mathbf{W},\mathbf{A})=\frac{1}{2n}||\mathbf{X}-\mathbf{XW}-\mathbf{YA}||_F^2
$$
To enforce the sparsity of parameters, this papaer use $\mathscr{l}_1$ penalties in the objective function, as:
$$
\mathop{\mathrm{min}}\limits_{\mathbf{W},\mathbf{A}}\quad f(\mathbf{W},\mathbf{A}) \ \mathrm{s.t.}\  \mathbf{W}\ \mathrm{is\ acyclic},
$$
$$
\mathrm{where}\ f(\mathbf{W},\mathbf{A})= \mathscr{l}(\mathbf{W},\mathbf{A})+\lambda_{\mathbf{W}}||\mathbf{W}||_1+\lambda_{\mathbf{A}}||\mathbf{A}||_1
$$

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **DYNOTRAES** *Datasets* *LLMs in Causal Discovery* *Discussion* -->

### 1.3 Optimization

According to the previsous theory, if and only if $\mathbf{W}$ is acyclic, $h(\mathbf{W})=\mathrm{tr}\ e^{\mathbf{W}\circ\mathbf{W}}-d=0$, this the overall optimization goal is
$$
\mathop{\mathrm{min}}\limits_{\mathbf{W},\mathbf{A}}\quad F(\mathbf{W},\mathbf{A})
$$
$$
F(\mathbf{W},\mathbf{A})=f(\mathbf{W,\mathbf{A}})+\frac{\rho}{2}h(\mathbf{W})^2+\alpha h(\mathbf{W})
$$

Furthermore, DYNOTEAR set to thresholding $\tau_\mathbf{W},\tau_\mathbf{A}$ to control the number of weights. When number small thean thresholdig, it will set as zero.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **DYNOTRAES** *Datasets* *LLMs in Causal Discovery* *Discussion* -->
### 1.4 Question

- How to obtain the causal graph from $\mathbf{W,A}$？
- Does this method build the causal graph having each note across every time?
- This final step of optimization.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. Datasets

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* **Datasets** *LLMs in Causal Discovery* *Discussion* -->
### 2.1 DYNOTEAR simulated dataset

To test the intra and inter connection, this paper follows equation $\mathbf{X}=\mathbf{X}\mathbf{W}+\mathbf{Y}\mathbf{A}+\mathbf{Z}$ to simulate data by following steps:

- Generating the weighted graphs $\mathcal{G}_\mathbf{W}$ and $\mathcal{G}_\mathbf{A}$.
- Generating the data matrices $\mathbf{X}$ and $\mathbf{Y}$.

This paper separate the generation of two types of test datasets:

- Intra-slice model
  - ER Model
  - BA Model
- Inter-slice model
  - Directed ER Model
  - Simplified Stochastic Block Model

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* **Datasets** *LLMs in Causal Discovery* *Discussion* -->

### 2.2 DYNOTRAR Real Dataset

- S&P 100 stock returns
  - This datasets obtain from `yahoofinancials` Python package.
  - Contains of daily closing prices from 1 July 2014 to 30 June 2019.
  - After preprocessing, this dataset contains $n=1257$ samples, and $d=97$ variables.
- DREAM4 gene expression data
  - This datasets come from the same name challenge which contain 5 independent sub-datasets.
  - Each with 10 different time-series recording from 100 genes across 21 time steps.

> Question:
>
> - The simulations details.
> - The metrics of DYNOTEAR

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* **Datasets** *LLMs in Causal Discovery* *Discussion* -->

### 2.3 LEMMA-RLEMMA-RCA Structured (Product Review)

Here are some problems about the LEMMA-RLEMMA-RCA dataset when formatting the data:

- What is the difference between 'Book_Info_product' and 'Book_Info_product_testuseer'?
- 20211203 has more datas than others.
- The pod number are various in different metrcis within the same day.
- The KPI csv have same timesteps.
- The Sequences have one more column than the node/pod.
- The npy files for each date are not consistent.
- **A lot of missing log files**.

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* **Datasets** *LLMs in Causal Discovery* *Discussion* -->

### 2.3 LEMMA-RLEMMA-RCA Structured (Cloud Computing)

- What's metric_error and metric_latency?
- Different days have various data structure, such as `20231221` missing node data, `20231207` don't includ metric_error and metric_latency.
- `20240115` log data only have one part(message).

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 3. LLMs in Causal Discovery

---
<!-- _class: navbar cols-2 -->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* *Datasets* **LLMs in Causal Discovery** *Discussion* -->

### 3.1 Overview

<div class="ldiv">

This method synthesis the LLMs which can inference the causal by domain knowledge and traditional statistical methods.
The overall workflow includings:

1. Using statistical causal discovery algorithms to obtain the original causal graph.
2. Traverse each pair of nodes and input them into LLMs to generate a domain knowledges.
3. Continually ask to the LLMs, inquire about whether the node should be connected.
4. After obtain the constrain, do the SCD again.

</div>

<div class="rimg">

![Overall framework of the statistical causal prompting in a large language model (LLM)](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/image-20240807171040770.png)

</div>

---

<!-- _class: navbar  tinytext-->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* *Datasets* **LLMs in Causal Discovery** *Discussion* -->
### 3.2 Prompt template (Domain Knowledge Prompting)

We want to carry out causal inference on `< blank 1. The theme >`, considering `< blank 2. The description of all variables >` as variables.

First, we have conducted the statistical causal discovery with `< blank 3. The name of the SCD algorithm >`, using a fully standardized dataset on `< blank 4. The description of the dataset >`.

`< blank 5. Including here the information of SCD results. The detail of the contents depends on prompting patterns. >`

According to the results shown above, it has been determined that there may be `< blank 6. a/no >` direct impact of a change in `< blank 7. The name of one node >` on `< blank 8. The name of another node >` `< blank 9. The value of causal coefficients or bootstrap probability >`. Then, your task is to interpret this result from a domain knowledge perspective and determine whether this statistically suggested hypothesis is plausible in the context of the domain.

Please provide an explanation that leverages your expert knowledge on the causal relationship between `< blank 7. The name of one node >` and `< blank 8. The name of another node >`, and assess the naturalness of this causal discovery result. Your response should consider the relevant factors and provide a reasoned explanation based on your understanding of the domain.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* *Datasets* **LLMs in Causal Discovery** *Discussion* -->
### 3.2 Prompt template (Knowledge Intergration)

An expert was asked the question below:
`< blank 10. Previous prompt >`

Then, the expert replied with its domain knowledge:
`< blank 11. Previous answer >`

Considering objectively this discussion above, if `< blank 12. The name of one node >` is modified, will it have a direct or indirect impact on `< blank 13. The name of 𝑥𝑖 >`?

Please answer this question with ⟨yes⟩ or ⟨no⟩.

No answers except these two responses are needed

---

<!-- _class: navbar -->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* *Datasets* **LLMs in Causal Discovery** *Discussion* -->

### 3.3 Pattern

Related to the `< blank 5 >` and `< blank 9 >`, this paper test different pattern to fill in the blank.

0. Without SCP.
1. With the list of edges that appeared in the first SCD.
2. With the list of the edges with their non-zero bootstrap probabilities
3. With the list of edges that emerged in the first SCD with the calculated causal coefficients (only for DirectLiNGAM)
4. With the list of edges that emerged in the first SCD with the calculated causal coefficients (only for DirectLiNGAM)

---

<!-- _class: navbar cols-2 -->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* *Datasets* **LLMs in Causal Discovery** *Discussion* -->

### 3.3 Pattern

<div class="limg">

![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/image-20240808202950721.png)

</div>

<div class="rimg">

![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/image-20240808203000605.png)

</div>

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 4. Discussion

---
<!-- _class: navbar -->
<!-- _header: \ ***Weekly Report*** *DYNOTRAES* *Datasets* *LLMs in Causal Discovery* **Discussion** -->
### Discussion

- Why need to do the SCD before let the LLMs generate the domain knowledge?
- How to combine DYNOTRAR and LLMs frameworks?
- May we need to discover more previous LLM4caual graph methods and do a review. Currently have no idea to improve them.
- Need to discovery DirectLiNGAM.
