# Feb 12

## Problem Correction

* Multi-view is different from our methods.

  * Multi-view is the previous method for Multi-modality.
  * This method uses alignment because two modalities represent the same semantic information.
  * This method will use only one branch in the final.

  > We may not claim that our method is multi-view.

## New Discussion topic

### Methods Structure

The paper “Are Language Models Useful for Time Series Forecasting?” proposes that removing the pre-trained LLMs in Time-LLM and only using one multi-head attention(encoder) can have better performance.

* A per-trained Large language model is useless in the forecasting field.	
* After removing the LLMs, the single transformer-based methods with patches can achieve great methods.

But if we use PatchTST as one branch, it will cause our methods to have totally the same architecture.

Additionally, our “Vision branch” actually is a new method to split patches (Due to the ViT reason).

> Vit is a Transformer used in Vision by splitting the Image to patch.

## New Question

* The single variable methods will be argued, such as VisionTS
* ViT for forecasting also be rejected: https://openreview.net/forum?id=dszD2gZIif
* We can’t use the multi-variable heat map



















1. The period will be 1-> cause 1 * T image.-> test dataset.
2. Add Multi-variate branch to Patch TST-> improve Patch TST performance
3. Use conv instead of multi-variable heat map.
4. Key Idea: Vision Model accepted multi-variable information to improve  old branch -> as the framework
5. seasonal trend-> need to thinks



TODO:

1. finish original framework
2. End2End train + seprate training
3. forecasting
4. test which strategy to test vit
5. Zero-Shot









