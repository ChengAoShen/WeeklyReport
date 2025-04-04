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

# Weighted Average is all you need


ChengAo Shen
Apr 2, 2025

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Vision as Feature Extractor
- Vision as Reconstructor
- Simple Model to simulate VisionTS
- Disscusion

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Vision as Feature Extractor

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Extractor** *Reconstructor* *Methods* *Discussion* -->
### 1.1 Motivation

1. Transforming one-dimensional time series into two-dimensional representations creates a new feature space, enabling visual models to extract supplementary features more effectively.
2. In the field of time series classification, certain features can be well-represented when converted into images.

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Extractor** *Reconstructor* *Methods* *Discussion* -->
### 1.2 Methods
Key: Result of Using Pretrain ViT+LinearLayer.

<div align="center">
    <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021042752.png" alt="Experiment Result" width="800">
</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Extractor** *Reconstructor* *Methods* *Discussion* -->
### 1.3 Experiment
![image-20250402104317793](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021043812.png)

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Extractor** *Reconstructor* *Methods* *Discussion* -->
### 1.4 Why Extractor didn't work?
1. Pretrained ViT-based Feature Extractors generate embeddings that represent **abstract semantic features** rather than numerical characteristics. Attempting to restore these embeddings to numerical values using a **Linear layer** is highly challenging.
2. ViT's pretraining focuses on image classification, aiming to understand the information of object. This makes it particularly difficult to perform **low-level numerical restoration**.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Vision as Reconstructor

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Extractor* **Reconstructor** *Methods* *Discussion* -->
### 2.1 Background
1. Using visual models for feature extraction makes Linear restoration extremely challenging.  
2. VisionTS appears to achieve impressive results.

> Idea: Should we retain the decoder to restore the numerical domain?

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Extractor* **Reconstructor* *Methods* *Discussion* -->
### 2.2 Experiment
Context Window 1152(Use checkpoint from FaceBook and MS)

<div align="center">
    <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021043885.png" alt="Experiment Result" width="600">
</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***vitf*** *extractor* **reconstructor** *methods* *discussion* -->
### 2.3 which part in vision real help the performance?
**Simple Study**: Using VisionTS to predict the Sin function

![image-20250402104221488](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021042508.png)


---

<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***vitf*** *extractor* **reconstructor** *methods* *discussion* -->
### 2.3 which part in vision real help the performance?
#### Interpertation


<div class="ldiv">
In reality, VisionTS does not effectively supplement information or achieve predictive capabilities through the image modality. In practical applications, it primarily relies on copying information from adjacent pixels on the left. Since the heatmap itself exhibits periodicity, its behavior can be interpreted as a direct replication of the previous period.
</div>

<div class='rimg'>

![image-20250402104240132](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021042152.png)

</div>

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 3. Simple Model to simulate VisionTS

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Extractor* *Reconstructor* **Methods** *Discussion* -->
### 3.1 Methods

<div align="center">
    <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021042714.png" alt="Experiment Result" width="600">
</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Extractor* *Reconstructor* **Methods** *Discussion* -->
### 3.2 Experiment Results

<div align="center">
    <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021044510.png" alt="Experiment Result" width="1000">
</div>

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 4. Discussion
---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Extractor* *Reconstructor* *Methods* **Discussion** -->
### 4.1 Pre-condition
1. Can periodicity be utilized effectively?  
2. Is the window length appropriate for use?  


---

<!-- _class: navbar cols-2-46-->
<!-- _header: \ ***ViTF*** *Extractor* *Reconstructor* *Methods* **Discussion** -->
### 4.2 Are Deep Networks Really Effective?

<div class="ldiv">
Deep networks like ViTs and LLMs focus on extracting high-level semantic features, which often leads to the loss of critical numerical details in time series forecasting. Restoring these details with a simple linear layer is challenging.

A better approach could involve using an encoder-decoder structure with hierarchical connections, like U-Net, to preserve and restore numerical information more effectively.
</div>

<div class="rimg">
    <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202504021121973.png" alt="U-Net Structure" width="600">
</div>

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Extractor* *Reconstructor* *Methods* **Discussion** -->

### 4.3 Future Work

1. Continue experimenting with the simplest models.  
2. Attempt to modify previous work into a U-Net structure to better restore numerical information.  
3. Explore the possibility of creating a new benchmark to replace ETTh1.  
