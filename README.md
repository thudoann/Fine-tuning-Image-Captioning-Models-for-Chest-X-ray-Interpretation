# Fine-tuning Image-Captioning Models for Chest X-ray Interpretation

## Table of Contents:
1. [Introduction](#introduction)
2. [Methods](#methods)
   1. [Dataset](#dataset)
   2. [Models](#models)
      - [Microsoft git-base](#microsoft-git-base)
      - [Salesforce Blip-base](#salesforce-blip-base)
   3. [Fine-tuning Plan](#fine-tuning-plan)
   4. [Model Evaluation](#model-evaluation)
3. [Results](#results)
4. [Discussion](#discussion)
5. [Limitations and Future Work](#limitations-and-future-work)
6. [References](#references)

## Introduction <a name="introduction"></a>
Recent advancements in image captioning have not been extensively applied to medical imaging. Leveraging advanced image captioning techniques for chest X-ray interpretation can enhance healthcare practitioners' ability to diagnose and interpret medical images swiftly. This project aims to develop robust models capable of generating precise and informative captions for radiological images, contributing to improved diagnostic processes.

## Methods <a name="methods"></a>

### Dataset <a name="dataset"></a>
#### ROCO Dataset: Overview
- Purpose: Designed for image captioning in medical imaging.
- Content: ~65k radiological images (X-rays, MRI, CT scans) for training, ~8.2k for testing, ~8.2k for validation.
- Annotations: Detailed captions for each image provided by specialists.

### Models <a name="models"></a>

#### Microsoft git-base <a name="microsoft-git-base"></a>
- GIT (Generative Image2Text), base-sized Transformer.
- Image Encoder: Pre-trained with contrastive tasks.
- Text Decoder: Transformer module with self-attention layers.
- Initialization and Training Approach: Random initialization of the text decoder.

#### Salesforce Blip-base <a name="salesforce-blip-base"></a>
- BLIP (Bootstrapping Language-Image Pre-training), utilizing Vision Transformer (ViT).
- Image Encoder: ViT divides an input image into patches and encodes them.
- Text Encoder: BERT-based architecture with Image-Text Contrastive Loss.
- Image-grounded Text Encoder: Additional cross-attention layer for image-text alignment.
- Image-grounded Text Decoder: Causal self-attention layers for autoregressive caption generation.

### Fine-tuning Plan <a name="fine-tuning-plan"></a>
- Learning Rate: 5e-5
- Optimizer: AdamW
- Weight Decay: 1e-08
- Training Epochs: 10
- Loss Function: Cross Entropy
- GPU Efficient Parameters: Mixed Precision Training (fp16), Batch Size, Gradient Accumulation Steps.

### Model Evaluation <a name="model-evaluation"></a>
- Evaluation Metrics:
  - BLEU (Bilingual Evaluation Understudy)
  - ROUGE (Recall-Oriented Understudy for Gisting Evaluation)
  - Meteor (Metric for Evaluation of Translation with Explicit Ordering)

## Results <a name="results"></a>
### Evaluation Metrics Based on ROOC Dataset
| Model         | ROUGE-1 | ROUGE-2 | ROUGE-L | ROUGE-Lsum | BLEU | Meteor |
|---------------|---------|---------|---------|------------|------|--------|
| Git-base      | 0.1     | 0.01    | 0.8     | 0.8        | 0    | 0.05   |
| Git-base-ft   | 0.35    | 0.22    | 0.33    | 0.34       | 0    | 0.14   |
| Blip-base     | 0.2     | 0.06    | 0.19    | 0.19       | 0    | 0.08   |
| Blip-base-ft  | 0.34    | 0.22    | 0.33    | 0.35       | 0.08 | 0.12   |

### Visualizing X-rays along with Generated Captions
- Example 1:
  - Original Caption: *coronary chest x-ray computed tomography (mediastinal window) showing massive pericardial effusion with an increased pericardial thickness (arrowheads)*
  - Git base: *what is the name of a pregnant woman?*
  - Git fine-tuned: *chest x-ray showing a large left-sided pneumothorax.*

- Example 2:
  - Original Caption: *chest x-ray: multiple bilateral opacities and reticular pattern in both thoracic fields*
  - Blip base: *a chest with a chest with a chest*
  - Blip fine-tuned: *Chest x-ray showing a large right-sided mass with a mass-like opacity at the right mid and lower lung zones.*

## Discussion <a name="discussion"></a>
- Blip outperformed others in semantic-based metrics.
- The model is suggested for providing suggestions rather than making final decisions.
- Domain specialists' engagement is crucial for accurate model predictions.

## Limitations and Future Work <a name="limitations-and-future-work"></a>
- Limitations include a lack of domain knowledge, access to high-powered GPUs, and limited data diversity.
- Future work focuses on developing advanced fine-tuning strategies to enhance diagnostic capabilities.

## References <a name="references"></a>
- [GIT: A Generative Image-to-text Transformer for Vision and Language](#)
- [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](#)
- [BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation](#)
- [Learning to Evaluate Image Captioning](#)
