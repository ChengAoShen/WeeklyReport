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

## Using LLMs to optimize the PC

ChengAo Shen
Aug 23, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Methods
- Experiment
- Discussion

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 1. Methods

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Methods** *Experiment* *Discussion* -->

### 1.1 Llama3.1

Llama3.1 is the latest version of Llama, which comprises 3 base models and 5 fine-tuned models. Here are some key features that explain why we chose Llama3.1:

- A large context window of 128K tokens
- Tool usage capabilities, including Brave search, Wolfram Alpha, and others

The model we use in flowing experiments is **Llama3.1-8B-instruct**. This model needs 16GB of GPU memory when inference with FP16, 8GB when inference with FP8.

To enable the use of tools, the instruct version of the model has a new role **ipython**. This role is used as the output of a tool call when sent back to the LLM.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Methods** *Experiment* *Discussion* -->

### 1.2 Domain Knowledge Prompts (Dataset Part)

The first part of domain knowledge prompts is used to describe the dataset and the causal discovery result.

```python
    dataset_prompt = (
        f"We want to carry out causal inference on {theme}, considering {', '.join(labels)} as variables.\n\n"
        f"First, we have conducted the statistical causal discovery with {scd_algorithm}, "
        f"using a fully standardized dataset on {description_of_dataset}.\n\n"
        f"All of the directed edges suggested by the statistic causal discovery are below:\n\n"
        f"{matrix_to_text(graph, labels)}\n\n"
        f"a -> b means a is the cause of b,\n"
        f"a -- b means a and b are independent,\n"
        f"a <-> b means a and b are dependent.\n\n"
    )
```

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Methods** *Experiment* *Discussion* -->

### 1.2 Domain Knowledge Prompts (Edge Part)

The second part of domain knowledge prompts is used to describe the causal relationship between two variables.

```python
# Generate appropriate prompt based on the edge type in the graph
if graph[i, j] == 0:
    edge_prompt = f"Based on the results above, it seems that changes in {labels[i]} do not directly affect {labels[j]}.\n"
elif graph[i, j] == 1:
    edge_prompt = f"Based on the results above, it appears that changes in {labels[i]} directly affect {labels[j]}.\n"
else:
    edge_prompt = f"Based on the results above, there might be a direct causal relationship between {labels[i]} and {labels[j]}.\n\n"

finally_prompt = (
    f"Then, your task is to interpret this result from a domain knowledge perspective "
    f"and determine whether this statistically suggested hypothesis is plausible in "
    f"the context of the domain.\n\n"
    f"Please provide an explanation that leverages your expert knowledge on the causal "
    f"relationship between {labels[i]} and {labels[j]}, "
    f"and assess the naturalness of this causal discovery result. Your response should "
    f"consider the relevant factors and provide a reasoned explanation based on your "
    f"understanding of the domain."
)
```

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Methods** *Experiment* *Discussion* -->

### 1.2 Domain Knowledge Prompts (Generation part)

When using the domain knowledge prompts, we set the system role as the expert in the theme.

```python
domain_knowledge_messages = [
    {"role": "system", "content": f"You are an expert in {theme}."},
    {"role": "user", "content": domain_knowledge_prompt},
]
domain_knowledge_output = pipeline(
    domain_knowledge_messages,
    max_new_tokens=512,
)[0]['generated_text'][-1]['content']
```

The example output looks like this:

```
As an expert in gasoline consumption, I'll provide an interpretation of the statistically suggested hypothesis that changes in Displacement do not directly affect Mpg.
In the context of internal combustion engines, Displacement is a key factor in determining the engine's size and capacity.
However, the relationship between Displacement and Mpg is not straightforward. Several factors can influence the actual fuel efficiency of an engine, including:
1. **Engine design and technology**: Modern engines often incorporate advanced technologies like direct fuel injection, turbocharging, and variable valve timing, which can improve fuel efficiency even with larger displacement engines.
2. **Engine efficiency**: The actual efficiency of the engine, measured in terms of its ability to convert fuel energy into usable power, can vary significantly depending on factors like combustion efficiency, friction losses, and heat transfer.
3. **Transmission and drivetrain**: The type of transmission and drivetrain used can also impact fuel efficiency, as they can affect the engine's ability to transmit power to the wheels.
4. **Driving conditions**: Real-world driving conditions, such as driving style, road terrain, and weather, can also influence fuel efficiency.
```

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Methods** *Experiment* *Discussion* -->

### 1.3 Edge Prompts

When using the model to evaluate the connection between two variables, we will use edge prompts which include all the previous information. Finally, the model will export yes or no.

```python
edge_prompt = (
    f"An expert was asked the following question:\n"
    f"{domain_knowledge_prompt}\n\n"
    f"Then, the expert replied with their domain knowledge:\n"
    f"{domain_knowledge_output}\n\n"
    f"Considering this discussion objectively, if {causal_node} is modified, will it have a direct or indirect impact on {result_node}?\n"
    f"Please answer this question with ⟨yes⟩ or ⟨no⟩.\n"
    f"No answers except these two responses are needed.\n"
    f"Your response should be in the following format:\n"
    f"⟨yes⟩ or ⟨no⟩\n"
    f"Please provide your response in the format specified above.\n"
    f"Your response:\n"
)

edge_messages = [
    {
        "role": "system",
        "content": "You are a helpful assistant for causal inference.",
    },
    {"role": "user", "content": edge_prompt},
]
```

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Experiment

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Methods* **Experiment** *Discussion* -->

### 2.1 Data

To evaluate the method’s ability, we use the dataset from different fields. The details information on the dataset is shown in the following table.

|                       |       Auto_MPG       |  DWD_Climate   |  Sachs  |
| :-------------------: | :------------------: | :------------: | :-----: |
|         Theme         | Gasoline consumption | Climate change | Biology |
|       Variables       |          5           |       6        |   11    |
| Length of time series |         392          |      349       |  7467   |

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Methods* **Experiment** *Discussion* -->
### 2.2 Metrics

To evaluate the methods, we use some metrics to test the performance. The results of the metrics are shown in the following table.

|           |   Auto_MPG   | DWD_Climate |    Sachs     |
| :-------: | :----------: | :---------: | :----------: |
|    FPR    |  0.4 -> 0.3  | 0.2 -> 0.16 | 0.15 -> 0.19 |
|    FNR    |  0.8 -> 0.6  | 0.83 -> 1.0 | 0.47 -> 0.78 |
| Precision | 0.11 -> 0.25 |  0.14 -> 0  | 0.38 -> 0.16 |
| F1 Score  | 0.14 -> 0.31 |  0.15 -> 0  | 0.44 -> 0.18 |
|    SHD    |    8 -> 5    |   9 -> 8    |   24 -> 29   |


---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->
## 3. Discussion

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Methods* *Experiment* **Discussion** -->
### 3.1 Problems

- The original PC results have a big impact when the LLMs inference.
- The model tends to give a `yes` answer.
- When dealing with a large dataset, the time is uncontrollable. (one pair of nodes need merely 190s)
- PC algorithm limits the capability of methods.
- How to obtain and evaluate the root cause after having the causal graph

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Methods* *Experiment* **Discussion** -->
### 3.2 Ideas

- Add brave search to enable model to learn the details information of variables and the relationship between them.
- Let the model give the strength of causality which can be used as the distance of the node in the graph
- Using two steps to determine the edge
  1. Determine the edge existence (only need judge half of dataset)
  2. Determine the edge direction

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Methods* *Experiment* **Discussion** -->
### 3.3 Future Work

- Add uncertainty evaluation
- Attempt to conduct ablation experiments
  - Only use the LLMs outputs as results
  - Design the prompts without the original PC results
- Let LLMs determine which is more important for multivariate problems(LEMMA-RCA)
