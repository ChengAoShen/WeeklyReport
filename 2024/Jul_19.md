---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Jul 19, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Exploring the Lemma-RCA and Causal Graph

ChengAo Shen
Jul 19, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- LEMMA-RCA
- Causal Discovery Algorithms
- ALCM
- Discussion

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. LEMMA-RCA

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **LEMMA-RCA** *Causal Discovery Algorithms* *ALCM* *Discussion* -->
### 1.1 Overview

The overall LEMMA-RCA dataset includes IT and OT subdatasets. The OT dataset refers to other existing work, thus we only discuss the IT part here. The IT subdatasets can be divided into two parts: Cloud Computing and Product Review. Each of them contains Original data and preprocessed data.

- Cloud Computing: The dataset contains eleven system nodes and in six days that error happened.
- Product Review: The dataset contains six OpenShift nodes and 216 system pods. Totally four days of data.

Following, we will use the Product Review dataset as an example to illustrate the Preprocessed part and Original part.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **LEMMA-RCA** *Causal Discovery Algorithms* *ALCM* *Discussion* -->
### 1.2 Preprocessed Data

This format of data mainly contains `Metric Data` and `Log Data`.

- `Metirc Data`: The metric data contains the KPI, node-level resource utilization, and pod-level resource usage.
- `Log Data`: The log data contains the pod removed and node removed information.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **LEMMA-RCA** *Causal Discovery Algorithms* *ALCM* *Discussion* -->
### 1.3 Original Data

This part contains the original data, including the resource usage data, and other log data. Different days usually have a different structure, one normal structure is shown as follows:

- Metrics
- Jaeger
- Log
- log_preprocessed
- Manifest
- pod
- storage_log

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **LEMMA-RCA** *Causal Discovery Algorithms* *ALCM* *Discussion* -->
### 1.4 Questions

- How to pair the Metrics data and Log data?
- What do pod_removed and node_removed mean in the dataset?
- How to find the anomalies in KPI data?
- What's the workflow of the LEMMA-RCA dataset?
  Metric/Log data -> Causal Graph about Pods -> Root problematic pod ->? Mistake
- Can we find the call relationship between pods in the Log data?
- Should I find the detailed information about the pods? And how to do it?
- On different days, the pod number and node number are different.
- Where is the log feature extraction result?
- What is bookinfo?

---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 2. Causal Discovery Algorithms

---

<!-- _class: navbar -->
<!-- _header: \ ***Weekly Report*** *LEMMA-RCA* **Causal Discovery Algorithms** *ALCM* *Discussion* -->

## 2. Causal Discovery Algorithms

1. Constraint-Based Algorithm
2. Score-Based Algorithms
3. Function-Based Algorithms
4. Optimization-Based Algorithms

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 3. ALCM

---
<!-- _class: navbar cols-2-64-->
<!-- _header: \ ***Weekly Report*** *LEMMA-RCA* *Causal Discovery Algorithms* **ALCM** *Discussion* -->
### 3.1 ALCM Algorithm

<div class=limg>

![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202407181825790.png)

</div>

<div class=rdiv>

ALCM is a new framework to synergize data-driven causal discovery algorithms and LLMs, automating the generalization of a causal graph. This method can be divided into three parts:

1. Causal Structure Learning
2. Causal Wrapper
3. LLM-driven Refiner

</div>

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *LEMMA-RCA* *Causal Discovery Algorithms* **ALCM** *Discussion* -->

### 3.2 Benchmarks
This framework has been evaluated on several benchmark datasets formatted as a Bayesian network that can use the format is comprehensive understanding do not below code to load and sample:

```python
reader = BIFReader("./Dataset/cancer.bif")
model = reader.get_model()
sampler = BayesianModelSampling(model)
# Generate 1000 samples
data = sampler.forward_sample(size=1000)
node_name = data.columns.tolist()
# Convert categorical data to numerical data
data_np = data.apply(lambda x: pd.factorize(x)[0]).values
```

---
<!-- _class: navbar cols-2-->
<!-- _header: \ ***Weekly Report*** *LEMMA-RCA* *Causal Discovery Algorithms* **ALCM** *Discussion* -->

### 3.3 Formate transforms

<div class=ldiv>

```python
def matrix_to_text(M: np.ndarray, name_list: list) -> str:
    """
    Convert a matrix to a text representation.
    """
    text = ""
    for i in range(M.shape[0]):
        for j in range(M.shape[1]):
            if i == j:
                continue
            if M[j, i] == 1 and M[i, j] == -1:
                text += f"{name_list[i]}->{name_list[j]},"
            if M[j, i] == 1 and M[i, j] == 1:
                text += f"{name_list[i]}<->{name_list[j]},"
            if M[j, i] == -1 and M[i, j] == -1:
                text += f"{name_list[i]}--{name_list[j]}"

    return text
```

</div>

<div class=rdiv>

```python
def text_to_matrix(text: str, name_list: list) -> np.ndarray:
    """
    Convert a text representation back to a matrix using the provided name list.
    """
    # Split the text into individual relations
    relations = text.strip().split(",")

    # Create a name index dictionary
    name_index = {name: idx for idx, name in enumerate(name_list)}

    n = len(name_list)
    M = np.zeros((n, n), dtype=int)

    for relation in relations:
        if "->" in relation:
            a, b = relation.split("->")
            i, j = name_index[a], name_index[b]
            M[i, j] = -1
            M[j, i] = 1
        elif "<->" in relation:
            a, b = relation.split("<->")
            i, j = name_index[a], name_index[b]
            M[i, j] = 1
            M[j, i] = 1
        elif "--" in relation:
            a, b = relation.split("--")
            i, j = name_index[a], name_index[b]
            M[i, j] = -1
            M[j, i] = -1

    return M

```

</div>

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *LEMMA-RCA* *Causal Discovery Algorithms* **ALCM** *Discussion* -->
### 3.4 Prompt Generation

```python
prompt = [
    {
        "role": "system",
        "content": "You are an expert on Cancer Risk Factors, Genetic Cancer Relation, along with the domain of causal discovery.",
    },
    {
        "role": "user",
        "content": (
            "Consider yo have received the results from a causaldiscovery algorithm (PC) executed on a Cancer dataset,"
            "The input formate is [causal name1->result1, causal name2->result2,...]"
            f"The node's name list are {node_name}. "
            "Based on your current comprehensiveunderstanding of this field,"
            "You may modify, delete, or add nodes/edges, or change the orientation of the edges. "
            "Ensure that your modifications are grounded in actual data and avoid making unfounded assumptions. "
            f"The input is:[{input_text}]"
            "The output should be formatted as [causal name1->result1, causal name2->result2,...]. "
            "You are no need to provide details information. The output is:"
        ),
    },
]

```

---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 4. Discussion

---

<!-- _class: navbar -->
<!-- _header: \ ***Weekly Report*** *LEMMA-RCA* *Causal Discovery Algorithms* *ALCM* **Discussion** -->
### 4.1 Discussion

- In traditional causal graph generation algorithms, they find the relationship may not use the time information, how can we extract more time-related information from the data?
- In ALCM, the prompt may need adjusting when the data's background changes. Can we generate a prompt automatically?
- Can't precisely control the output of the LLMs.