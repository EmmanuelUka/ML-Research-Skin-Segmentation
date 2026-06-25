# DSAT — Skin Lesion Segmentation on IMA++

## Dual Attention Self-Attention Transformer for 
## Uncertainty-Aware Skin Lesion Segmentation

---

## Overview

This project trains and evaluates a novel segmentation 
architecture called DSAT (Dual Attention Self-Attention 
Transformer) on the IMA++ dataset — the largest publicly 
available multi-annotator skin lesion segmentation dataset.

Unlike standard segmentation models that train on a single 
expert's annotation, DSAT is trained on soft labels derived 
from averaging the masks of multiple annotators per image. 
This allows the model to output a continuous confidence map 
rather than a hard binary mask — showing not just where the 
lesion is, but how certain the model is at each pixel, 
directly reflecting areas where human experts disagreed.

---

## Dataset — IMA++

The IMA++ dataset contains:
- 14,967 dermoscopic images from the ISIC Archive
- 17,684 segmentation masks drawn by 16 distinct annotators
- 2,394 images with 2–5 masks each (multi-annotator subset)
- 12,573 images with a single annotator mask

The multi-annotator subset enables a clinically meaningful 
evaluation: comparing the model's predictions directly 
against each individual human annotator's mask, and 
measuring whether the model falls within the range of 
human inter-annotator variability.

**Dataset links:**
- Raw dataset: https://zenodo.org/records/14201693
- ISIC Archive: https://api.isic-archive.com/collections/482/
- Kaggle compiled: https://www.kaggle.com/datasets/emmanueluc322/ima-segmentation-images

---

## Architecture — DSAT

The DSAT encoder processes each image through three 
sequential attention stages before passing features 
to a U-Net decoder:

1. **Channel Attention (CBAM)** — recalibrates which of 
   the 2,048 feature channels carry the most diagnostic 
   information for a given image
2. **Spatial Attention (CBAM)** — highlights which spatial 
   regions of the image contain the lesion and suppresses 
   irrelevant background areas
3. **Transformer Self-Attention** — captures global 
   relationships between all 25 spatial regions 
   simultaneously, providing context the CNN backbone 
   alone cannot

Backbone: InceptionV3 (ImageNet pretrained)  
Parameters: 80,808,419  
Input: 224 × 224 dermoscopic image  
Output: 224 × 224 confidence map (values 0–1 per pixel)

---

## Dataset Split

| Split | Images | Source | Purpose |
|---|---|---|---|
| Train | 10,687 | Single annotator | Learn general lesion boundaries |
| Val | 2,604 | Mixed | Monitor training progress |
| Test | 1,676 | Multi-annotator only | IAA evaluation |

The test set is reserved exclusively for multi-annotator 
evaluation and was never seen during training.

---

## Results

| Metric | Value |
|---|---|
| Best Val Dice | 0.8824 |
| Model Dice (vs annotators) | 0.8435 ± 0.1489 |
| Human Inter-Annotator Dice | 0.8503 ± 0.2000 |
| Model IoU | 0.7570 |
| Model within human range | ✓ Yes |

The gap between the model (0.8435) and human annotators 
(0.8503) is only 0.0068. The model's standard deviation 
(0.1489) is lower than the human standard deviation 
(0.2000), indicating the model produces more consistent 
boundary predictions than individual human experts.

To the best of our knowledge, DSAT is the first external 
segmentation model benchmarked on IMA++.

---

## Kaggle Notebook

https://www.kaggle.com/code/emmanueluc322/skin-disease-segmentation-annotation/notebook

---

## Citation

If you use this work or the IMA++ dataset, please cite:

```
@Article{abhishek2025imaplusplus,
  author  = {Abhishek, Kumar and Kawahara, Jeremy 
             and Hamarneh, Ghassan},
  title   = {{IMA++}: {ISIC Archive} Multi-Annotator 
             Dermoscopic Skin Lesion Segmentation Dataset},
  journal = {arXiv preprint arXiv:2512.21472},
  year    = {2025}
}
```