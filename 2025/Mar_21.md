---
marp: true
size: 16:9
theme: am_uh
paginate: true
footer: ChengAo Shen, Mar 21, 2025
---

<!-- _class: cover_e -->
<!-- _header: ![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png) -->
<!-- _footer: ![UH_brand](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_brand.png) -->
<!-- _paginate: "" -->

# ViTF: Vision to improve Time-series Forecasting


ChengAo Shen
Mar 21, 2025

---

<!-- _class: toc_b -->
<!-- _header: <br>CONTENTS<br>![UH_logo](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/UH_logo.png)-->
<!-- _footer: "" -->
<!-- _paginate: "" -->

- Questions to be Answered
- Methods
- Experiments Results
- Discussion

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 1. Questions to be Answered

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Questions** *Methods* *Experiments* *Discussion* -->
### 1.1 Main structure of PatchTST

- The main structure of PatchTS can be divided into three parts, assuming the input sequence is $B\times N\times T$
    1. Patch Splitting: Split the sequence into multiple patches, each patch has size $B\times N\times P\times T'$
    2. Use linear layer to reduce dimension from $T'$ to $D$, obtaining $B\times N\times P\times D$
    3. Flatten $B\times N\times P\times D$ into **$(B* N)\times P\times D$**
    4. Feed $(B* N)\times P\times D$ into Transformer Encoder, getting $(B*N)\times P\times D$
    5. Resize to $B\times N\times P\times D$, flatten to get $B\times N\times (P*D)$
    6. Use N linear layers to map it to $B\times N\times H$

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Questions** *Methods* *Experiments* *Discussion* -->

### 1.2 Previous questions about PatchTST
- How independent works in PatchTST?
    - It will learn different variables information during the same time.
- How the PatchTST calculate the loss?
    - It will use **all variables** to calculate the loss and metrics.
- How PatchTST run so fast?
    - It uses the parallel computing to speed up the training process.
    - It use really small patch size, which is $16/32$.


---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** **Questions** *Methods* *Experiments* *Discussion* -->
### 1.3 Previous questions about VisionTS

- VisionTS will also use $(B*N)\times H\times W$ to calculate the loss and metrics.
- Why VisionTS is so fast?
    - It uses the parallel computing to speed up the training process.
    - It's zero-shot model at most time. 
    - It didn't add many trainable parameters, all of it's parameters start from great checkpoint.

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 2. Methods(Single ViT to process image)

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Questions* **Methods** *Experiments* *Discussion* -->
### 2.1 three methods our test

![image-20250321133253994](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/ViTsignle.png)

---
<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 3. Experiments Results

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Questions* *Methods* **Experiments** *Discussion* -->

### 3.1 Results of ViT_single

![image-20250321135743505](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Results.png)

---

<!-- _class: trans -->
<!-- _footer: "" -->
<!-- _paginate: "" -->

## 4. Discussion

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Questions* *Methods* *Experiments* **Discussion** -->


### How to build the image from timeseries?

* We cannot use too many types of images, as many of them would lose their inherent time series values, which would not be helpful for prediction.

**Bilinear Interpolation**
In image enlargement, bilinear interpolation estimates the pixel value at position (x, y) in the scaled image by using the four surrounding pixels from the original image.

Let the four neighboring pixels be:

$(x_1, y_1),\ (x_2, y_1),\ (x_1, y_2),\ (x_2, y_2)$ with values: $f_{11},\ f_{21},\ f_{12},\ f_{22}$

Let: $t = x - x_1,\quad u = y - y_1$. Then: $f(x, y) = (1 - t)(1 - u)f_{11} + t(1 - u)f_{21} + (1 - t)u f_{12} + tu f_{22}$


Currently, we are treating the image as a heatmap, interpolating and then splitting it. This essentially means we are applying a blurring operation on the heatmap before splitting it into patches that serve as input to the ViT.
On one hand, this significantly increases our computational cost, and on the other hand, the numerical features themselves may be smoothed out by the bilinear interpolation.

---

<!-- _class: navbar-->
<!-- _header: \ ***ViTF*** *Questions* *Methods* *Experiments* **Discussion** -->

### Questions in ViT_single
1. Whether modifying the pre-positioned Attention is effective, as the large difference in model parameters between before and after makes gradients unstable during training, easily leading to gradient explosion and vanishing
2. How to handle cases with many variables (300+)
3. Differences between using for loops and batch processing in parallel scenarios