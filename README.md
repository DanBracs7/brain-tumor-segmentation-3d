# Brain Tumor Segmentation using 3D U-Net (BraTS 2020)
(The full thesis document (in Italian) is available in this repository as a PDF)
This repository contains the code and experimental results for a brain tumor segmentation project based on the BraTS 2020 dataset. The project implements a 3D U-Net architecture to identify and segment distinct tumor sub-regions from MRI scans.

## Overview

The primary objective of this project is to accurately segment brain tumors into three main sub-regions:
*   **NCR/NET**: Necrotic and non-enhancing tumor core.
*   **ED**: Peritumoral edema.
*   **ET**: GD-enhancing tumor.

Initial experiments were conducted using a 2D patch-based model, which achieved high patch-level Dice scores but struggled with spatial consistency along the z-axis and class separation (particularly between ET and NCR). To address these limitations, the final architecture relies on **volumetric 3D patches** with sliding window inference. This 3D approach successfully leverages volumetric context across adjacent slices, significantly improving the model's ability to separate complex boundaries and reducing the "jagged" artifacts typical of 2D slice-by-slice predictions.

## Quantitative Results

The 3D U-Net model demonstrates competitive performance, especially considering the hardware constraints and the reduced patch size utilized during training. It employs a combined loss function (Focal Loss + Dice Loss) to handle the severe class imbalance inherent to the BraTS dataset.

### Test Set Performance Metrics

| Metric | Value |
| :--- | :--- |
| **Accuracy** | 0.9319 |
| **Global Dice** | 0.7298 |
| **Precision** | 0.9325 |
| **Sensitivity (Recall)** | 0.8496 |
| **Specificity** | 0.9845 |
| **Mean IoU** | 0.3750 |

### Dice Scores by Sub-region

| Tumor Region | Dice Score |
| :--- | :--- |
| **ED (Edema)** | 0.7351 |
| **ET (Enhancing Tumor)** | 0.7907 |
| **NCR/NET (Necrotic/Non-enhancing)** | 0.6637 |

## Discussion

The model achieves a **Global Dice of 0.73**, indicating good overlap between the predicted and ground truth segmentations. 
*   **High Precision (0.93) and Specificity (0.98):** The model produces clean, well-defined segmentations with very few false positives, reliably distinguishing pathological regions from healthy background tissue.
*   **Sensitivity (0.85):** The slightly lower recall indicates that the model occasionally misses small portions of the tumor in low-contrast or highly complex cases.
*   **Class-Specific Challenges:** As expected, the Edema (ED) and Enhancing Tumor (ET) classes yielded the highest Dice scores. The NCR/NET class proved more difficult to segment (Dice 0.66) due to its smaller volume and poorly defined boundaries.

Compared to standard 3D U-Net baselines on the BraTS 2020 dataset (which typically report average Dice scores between 0.70 and 0.80), this pipeline offers a lightweight, reproducible alternative that avoids over-segmentation while maintaining structural consistency.