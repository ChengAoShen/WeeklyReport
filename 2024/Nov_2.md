---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Nov 2, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Using LLMs in Causal Discovery

## Discussion with Prof. DongKuan

ChengAo Shen
Nov 2, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Summary of the current project
- Questions about LLM's Agent

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Summary of current project

---

<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->

### 1.1 Abstract

**Background**: With the emergence of powerful models like GPT4 and llama3, LLMs are being used in various fields. However, there are notable limitations in using LLMs for causal graph construction and optimization. Most models still rely on direct judgment from standard LLMs **without fully utilizing the capabilities of LLM Agents**. Additionally, they have **limitations in accessing external information**, unable to utilize web information, log files, and other content.

**Motivation**: To address these limitations, we propose a novel framework that **leverages LLM Agents' reasoning and action capabilities to assist in causal graph construction**. By combining traditional Causal Discovery methods with the latest LLM Agents, and integrating external information with LLM's reasoning abilities, we aim to construct causal graphs that better reflect real-world situations.

---

<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->
### 1.2 Overall Framework

**Methods**: We propose a new LLMs Agent framework for causal discovery, which includes two interchangeable Agent modules to implement the introduction of external information and the processing of causal relationships. The goal is to achieve the most realistic Causal Graph construction possible.

<div style="text-align: center">
<img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Work%20flow%20Oct%2024.svg" width="50%">
</div>

---

<!-- _class: navbar cols-2-64 -->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->

### 1.2 Overall Framework

<div class="ldiv">

1. Use traditional Causal Discovery methods to construct an initial Causal graph (the graph that needs to be optimized)
2. Use Web Search Agent, etc., to summarize external information, obtaining Dataset Information, including:
   1. The summary of the Dataset itself
   2. The summary for each node in the causal graph
3. Put the initial causal graph and Dataset information into Constrain Debate Agent as a premise condition, and ask each pair of nodes to determine whether to add/delete an edge.
4. Use the results from Constrain Debate Agent as prior knowledge to rerun the causal graph algorithm to obtain the final graph

</div>

<div class="rimg">

![Work flow Oct 24](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Work%20flow%20Oct%2024.svg)

</div>

---

<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->

### 1.3 Web Search Agent

<div class="ldiv">

The agent's function is to accept two names that need to be queried

- Dataset name：Name of the dataset used
- Node name：Name of each node in the dataset

The output of the agent is the information of the dataset and each node in it

</div>

<div class="rimg">

![Web Search Agent](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Web%20Search%20Agent.svg)

</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->

### 1.3 Web Search Agent (LLMs)

This model includes three LLMs

- `Search Query LLM` : responsible for detecting whether the current queried web information is sufficient, if not, it will generate a new query
- `Review LLM` : used to check whether the content obtained from the web is correct and whether there is bias
- `Summary LLM`：Using the current information in the database, output the final summary information

---

<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->
### 1.4 Constrain Agent

The Constrain Agent accepts the original Causal Graph (converted to text) and the previously obtained information to make final judgments on whether each edge should exist. It contains two LLMs:

1. `Domain Knowledge LLM`: Accepts all previous information and provides sufficient argumentation from an expert's perspective on whether edges should exist
2. `Constrain LLM`: Makes final determinations (or probability) of existence based on expert argumentation

<div class="rimg">

<img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Constrain%20Agents.svg" width="70%" style="display: block; margin: auto;">

</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** **Summary of current project** *Question about LLM's Agent* -->

### 1.5 Experiment

**Auto_MPG**

|             Methods             | FPR  | FNR  | Precision |  F1  | SHD  | NHD  |
| :-----------------------------: | :--: | :--: | :-------: | :--: | :--: | :--: |
|          Baseline(PC)           | 0.4  | 0.8  |   0.11    | 0.14 |  8   | 0.32 |
|      PC + Constrain agent       | 0.15 | 0.2  |   0.57    | 0.66 |  3   | 0.12 |
| PC + Web agent+ Constrain agent | 0.1  | 0.2  |   0.66    | 0.72 |  2   | 0.08 |
|       MAC(Tinghua paper)        |  /   |  /   |     /     |  /   |  4   | 0.16 |

**DWD_Climate**

|             Methods              | FPR  | FNR  | Precision |  F1  | SHD  |  NHD  |
| :------------------------------: | :--: | :--: | :-------: | :--: | :--: | :---: |
|           Baseline(PC)           | 0.2  | 0.83 |   0.14    | 0.15 |  9   | 0.25  |
|       PC + Constrain agent       | 0.06 | 0.66 |    0.5    | 0.4  |  6   | 0.16  |
| PC + Web agent + Constrain agent | 0.03 | 0.66 |   0.66    | 0.44 |  5   | 0.138 |
|        MAC(Tinghua paper)        |  /   |  /   |     /     |  /   |  5   | 0.138 |

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Questions about LLM's Agent

---
<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** *Summary of current project* **Question about LLM's Agent** -->
### 2.1 The definition of LLM and Agent

Q1: What is the difference between Agent and a single LLM?

> My Opinion: LLM only accepts a series of texts and predicts the following content. The agent can use outside tools such as API to extend more information. Normal LLMs don’t have continuous discussion ability. And only can run ones. For example, ChatGPT should be recognized as an Agent inside of LLMs.

---
<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** *Summary of current project* **Question about LLM's Agent** -->
### 2.2 Different between of Prompt and the question

Q2: ReAct is a prompt technique or an Agent? How is it using tools? We should write the prompt in ReAct by ourself? or just use it.

```python
# %%
agent_executor = create_react_agent(model, tools)
response = agent_executor.invoke(
    {"messages": [HumanMessage(content="Hi,how are you?")]} # What the content is?
)
response["messages"]
```

Q3: If we use RAG to regenerate the prompt, it should be divided into the part of the agent or just the prompt technique.

---
<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** *Summary of current project* **Question about LLM's Agent** -->
### 2.3 Other questions

Q4: If we ask LLM for more than one, the prompt is different due to the outside information while the template of LLM is the same, it should be counted as one LLM or more than one?

Q5: In the current mainstream work, when using external information such as Web, is it more inclined to write your own prompt for summarization or use an API?

---

<!-- _class: navbar-->
<!-- _header: \ ***Presentation*** *Summary of current project* **Question about LLM's Agent** -->

### 2.5 Question about our work

Q6: Our Agent and React framework seem to be repetitive. Is it necessary to replace the entire framework with a React?

Q7: Our framework, can it be completely replaced by a REACT Agent? Directly use an Agent to automatically obtain information and provide Constrain. What is the difference between the two?
