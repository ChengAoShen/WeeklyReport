# Feb 5

## LLMs for Time series

![LLMs for TS](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/LLMs_for_TS.png)

### Various Methodologies

![image-20250204135526656](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Direct_Prompting_of_LLMs.png)

**Direct Prompting of LLMs**: treat time series data as raw text and directly prompt LLMs with time series.

* Nuber-Agnostic Tokenization: The method treats numerical time series as raw textual data and directly prompts existing LLMs using predefined templates.
* Number-Specific Tokenization: add space between each number and use fixed precision.

**Quantization**: converts numerical data into discrete representations as input to LLMs.

![Quantization methods](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/quantization_function.png)

* VQ-VAE: Use Vector Quantized-Variational AutoEncoder to learn a codebook in latent space. Then it can quantize the continuous series to token ID.
* K-Means: Use the center of the cluster indicated as a token.
* Using pre-defined text to quantize.
* Others: Using frequency spectrum as a common dictionary; quantizes real-valued time series into discrete bins.

**Aligning:  **(embedding stage) designs time series encoder to align time series embeddings with language space

![Aligning based methods](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/202502041443475.png)

* Losses function: Using the contrastive learning idea to use loss function to train a time series encoder that can convert time series to text embedding space.
* LLMs as Backbone: directly adding LLMs to the model, can finetune the LLMs

**Vision modality as a bridging mechanism**

* Using the image as a bridge to convert from a time series to a different modality.

  > This method mainly used the Inertial Measurement Unit(IMU) dataset with paired datasets between image and IMU. 

* Time Series Plots as Images

**Combination of LLMs with Tools**: Use LLMs to generate indirect tools $z(\cdot)$ To benefit time series tasks. Such as Code, API Calls, and Text Domain Knowledge.

### Tricks

**Tokenization Design**

- Time series will encounter the challenge of distribution shift -> adopt channel independence and reversible instance normalization before tokenization.
- Mainly use patch representation from PatchTST.
- Harmonize the different modalities -> loss function to align.
- Some papers use STL to decompose to extract trend, seasonal, and residual components.

**Prompt Design**

- Enrich the prompt design by incorporating LLM-generated or gathered background information.
- Add statistical information of the time series.

**Fine-tuning Strategy**

- Use Low-Rank Adaption to fine-tune the self-attention modules.
- Two-stage fine-tuning
  1. The first stage is autoregressive fine-tuning.
  2. The second stage lets half epoch to train the final linear layer, and half epochs to train all parameters.

### Application

The time series task included **forecasting, classification, imputation, and anomaly detection** across different fields. Some typical domains are shown as follows:

* **Internet of Things**

  with text: DeepSQA

* **Finance**

  Series dataset: exchange_rate, FRED-MD,NN5

  with text: MoAT, PIXIU

* **Healthcare**

  Series dataset: Illness, Covid Deaths

  with text: Zuco, PTB-XL, ECG-QA

* **Audio**

  with text: OpenAQA-5M, Music Caps, Common Voice

* **Traffic**

  Series dataset: Traffic, Taxi

* **Energy**

  Series dataset: Electricity, ETT-small,

* **Weather**

  Series dataset: weather

##  Cutting-edge Vision-Language Model

* **CoDi (Composable Diffusion)**: is a multimodal AI model capable of generating and understanding multiple modalities, including text, images, audio, and video. It allows parallel and flexible multimodal generation, meaning it can create different types of content simultaneously while maintaining consistency. This makes it useful for applications such as creative content generation and cross-modal translation.
* **CoDi2**: CoDi2 is an improved version of CoDi, further enhancing its cross-modal reasoning and generation capabilities. It improves the quality of generated outputs, strengthens multimodal alignment, and ensures better consistency across different modalities. CoDi2 is designed for advanced applications like video editing, multimodal storytelling, and AI-driven media creation.
* **ImageBind** is a multi-modal alignment model developed by Meta AI, capable of mapping images, text, audio, depth, thermal imaging, and inertial measurement unit (IMU) data into a shared representation space without requiring explicit cross-modal paired training data. The core idea is to use images as a binding modality, leveraging large-scale unimodal datasets to learn cross-modal relationships. This enables zero-shot modality transfer, allowing tasks like audio-to-image or text-to-audio without direct supervision. ImageBind significantly enhances AI’s ability to perceive, integrate, and reason across diverse sensory modalities.
* **Gemini**: Gemini, developed by Google DeepMind, is a family of multimodal AI models designed to compete with OpenAI’s GPT series. It has strong capabilities in text, image, audio, video, and code understanding and generation. Gemini excels in complex reasoning, and problem-solving, and integrates seamlessly with Google’s ecosystem, making it suitable for productivity and research applications. The latest version, Gemini 1.5, improves context length and multimodal comprehension.
* **NExT-GPT**: NExT-GPT is a large-scale multimodal AI model developed by the National University of Singapore (NUS). It focuses on deep cross-modal alignment, allowing advanced reasoning and generation across text, images, audio, and video. Designed for research purposes, NExT-GPT enhances multimodal understanding and supports tasks like AI-powered storytelling and interactive media.

## Q & A

1. Choose which field to focus on? or test all datasets.
2. Use Codi to generate next time series step.
3. Image inpainting for the right piece(under mult-variable heatmap)
4. Align with text and images(heatmap)





