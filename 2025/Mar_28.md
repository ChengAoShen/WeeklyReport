---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Mar 28, 2025
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# ViTF: Vision to improve Time-series Forecasting


ChengAo Shen
Mar 28, 2025

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Methods
- Experiments
- Discussion
---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Methods

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Methods** *Experiments* *Discussion* -->
### 1.1 Review of Previous Issues

* Applying attention at the very start leads to poor model performance  
* Using Attention at the beginning shows limited effectiveness, possibly due to gradient vanishing
* Need to increase content windows to better utilize vision characteristics

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Methods** *Experiments* *Discussion* -->

### 1.2 New Methods

<div class="img" style="width: 90%; margin: auto;">

![image-20250327180855005](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503271808078.png)

</div>

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Experiments

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF***  *Methods* **Experiments** *Discussion* -->
### 2.1 Experiments Results

<div class="img" style="width: 90%; margin: auto;">

![image-20250327181000352](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503271810384.png)

</div>

---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 3. Discussion

---

<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***ViTF*** *Methods* *Experiments* **Discussion** -->

### 3.1 Different Vision Models

<div class="ldiv">
1. Reconstruction maybe important, maybe we can try reconstruction version of ViT, including SimMIM, MAE, etc.

</div>

<div class="rimg">


![image-20250328092703091](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503280927164.png)

</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Methods* *Experiments* **Discussion** -->

### 3.2 How Vision model works in Time-series Forecasting

1. Can ViT models effectively extract features into embeddings? Are these embeddings useful?
2. Wheather only vision model can deal with long context window?
3. The reason why VisionTS work is feature extraction or reconstruction?
