---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Mar 14, 2025
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# ViTF: Vision to improve Time-series Forecasting


ChengAo Shen
Mar 14, 2025

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Introduction
- Methods
- Current results

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Introduction

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Introduction** *Methods* *Results* -->
### 1.1 Abstract
Time series forecasting is a crucial task across many fields. While traditional methods use statistical and deep learning approaches, recent work explores vision-based methods like VisionTS and TimeVLM.

We propose ViTF, a vision transformer framework that addresses limitations in current approaches. Our plug-and-play module supports multiple image conversion types, utilizes cross-sequence information, and improves accuracy without requiring base model retraining.


---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Introduction** *Methods* *Results* -->
### 1.2 Motivation

Traditional time series models focus on direct sequence modeling or using language models for prediction. Recently, with advances in computer vision, researchers have started exploring vision-based approaches. Models like VisionTS and TimeVLM convert time series data into visual formats for processing with vision models.

Current vision-based approaches have three main limitations:

1. Limited image conversion methods (mainly simple HeatMaps), not fully utilizing vision models' capabilities
2. Insufficient consideration of relationships between multiple sequences in multivariate predictions
3. Lack of integration with existing time series models, requiring complete retraining

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Introduction** *Methods* *Results* -->
### 1.3 Key Idea

We propose a new plug-and-play Vision module to improve existing time series forecasting models. This module has the following advantages:

1. Compatible with multiple types of images transferred from time series.
2. Makes full use of shared information between different time series.
3. Can be combined with previous time series forecasting models to achieve improvements without requiring retraining of existing models.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Methods
---

<!-- _class: navbar cols-2-64-->
<!-- _header: \ ***ViTF*** *Introduction* **Methods** *Results* -->

### 2.1 Overall framework

<div class="ldiv">

- Convert multi-channel time series into images after normalization
- Use pre-trained ViT on images to extract features and generate corresponding embeddings
- Adjust the embeddings and fuse them with intermediate features from the original time series model
- Pass the fused features through prediction heads to obtain results


</div>

<div class="rimg">

![Overall](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503141341410.png)

</div>

---

<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***ViTF*** *Introduction* **Methods** *Results* -->

### 2.2 Image Constructer

<div class="ldiv">
Converting time series to RGB images involves:

- Transform time series using standard methods (FFT for spectrograms, stacking for heatmaps)
- Stack transformed images into multi-channel format
- Process and map to RGB space through either:
    - Method1: Convolution layers for channel sharing and RGB mapping
    - Method2: Attention mechanism for channel sharing with convolution mapping

</div>

<div class="rimg">

![Image Constructer](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503141348795.png)

</div>

---

<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***ViTF*** *Introduction* **Methods** *Results* -->

### 2.3 Multi-view Fusion
<div class="ldiv">

Two different branches will get different embeddings, and we use two different methods to fuse them.

- Method1: Use two convolutions to match sizes then concatenate
- Method2: Use attention mechanism for fusion

</div>

<div class="rimg">

![Multi-view Fusion](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503141421415.png)

</div>


---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 3. Current Results
---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Introduction* *Methods* **Results** -->

### 3.1 Results with PatchTST
![PatchTST](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202503141455272.png)