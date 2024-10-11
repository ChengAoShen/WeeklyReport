---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Sep 13, 2024
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# Weekly Report

## Recent progress and discussions

ChengAo Shen
Sep 13, 2024

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Question
- Discussion

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** **Question** *Discussion* -->

## Question

- Why do we need the down-sample and normalization steps?
- The CPU values are strange, some of them are larger than 100 (20210517) while some of them are really small (20211203).
- The template is similar in some pods.

---

<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Discussion* **Question** -->

## Discussion

- The KPI doesn't have the Log file. Can we find a substitute? 
- Remove the domain knowledge prompt from the next step (Only give the result representing the domain knowledge). This step will remove normal graph information from the edge LLMs.
- How can anomaly detection with the time series be done?
- Store the results of the first two LLMs separately to avoid duplicate operations.
- Use entity in the prompt and statement the entity is a pod at the beginning of the prompts.

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Discussion* **Question** -->
![image-20240911234230225](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202409112342463.png)

---
<!-- _class: navbar-->
<!-- _header: \ ***Weekly Report*** *Discussion* **Question** -->
![](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202409112341503.png)

