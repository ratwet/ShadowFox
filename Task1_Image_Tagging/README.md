# Task 1 — Image Tagging with CNN

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ratwet/ShadowFox/blob/Preview/Task1_Image_Tagging/Image_Tagging_CNN_FINAL.ipynb)
[![Level](https://img.shields.io/badge/Level-Beginner-brightgreen)]()
[![Framework](https://img.shields.io/badge/TensorFlow-2.20.0-FF6F00?logo=tensorflow&logoColor=white)]()

[← Back to main README](../README.md)

**Objective:** Build and train a Convolutional Neural Network **from scratch** — no pretrained weights — to classify images into 10 categories using the CIFAR-10 dataset.

> **Result: 74.07% test accuracy** (0.7495 test loss) after 25 epochs on a Tesla T4 GPU.

---

## Table of Contents
- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Model Architecture](#model-architecture)
- [Training Configuration](#training-configuration)
- [Results](#results)
- [Classification Report](#classification-report-test-set)
- [Key Techniques Used](#key-techniques-used)
- [How to Run](#how-to-run)
- [Tech Stack](#tech-stack)

---

## Problem Statement

Develop a practical image tagging solution by training a CNN capable of categorizing images into elementary classes such as cat, dog, automobile, and airplane. The model is built using TensorFlow and Keras **without using any pretrained weights**, so all feature representations are learned end-to-end from raw pixels.

---

## Dataset

**CIFAR-10** — a standard benchmark image classification dataset, loaded directly from the [official University of Toronto CS host](https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz) (~170 MB archive).

| Property | Value |
|---|---|
| Total images | 60,000 |
| Training samples | 50,000 |
| Test samples | 10,000 |
| Image size | 32 × 32 pixels |
| Color channels | 3 (RGB) |
| Number of classes | 10 |

**Classes:** airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck

---

## Model Architecture

A CNN designed from scratch with 3 convolutional blocks of increasing depth.

```
Input (32 x 32 x 3)
|
Block 1: Conv2D(32) -> BatchNorm -> Conv2D(32) -> MaxPool -> Dropout(0.25)
Block 2: Conv2D(64) -> BatchNorm -> Conv2D(64) -> MaxPool -> Dropout(0.30)
Block 3: Conv2D(128) -> BatchNorm -> MaxPool -> Dropout(0.40)
|
Flatten -> Dense(128) -> Dropout(0.5) -> Dense(10, softmax)
```

---

## Training Configuration

| Parameter | Value |
|---|---|
| Optimizer | Adam (lr=0.001) |
| Loss function | Sparse Categorical Crossentropy |
| Epochs | 25 |
| Batch size | 64 |
| Validation split | 10% (45,000 train / 5,000 val) |
| Early stopping | patience=5 |
| LR scheduler | ReduceLROnPlateau (factor=0.5) |
| Hardware | Google Colab — Tesla T4 GPU |

**Data Augmentation applied:** Horizontal flip, rotation (10%), zoom (10%)

---

## Results

| Metric | Value |
|---|---|
| Test Accuracy | **74.07%** |
| Test Loss | 0.7495 |
| Best Validation Accuracy | 75.26% (Epoch 22) |

### Training Progress
*(verified against the executed notebook log)*

| Epoch | Train Accuracy | Val Accuracy | Learning Rate |
|---|---|---|---|
| 1 | 25.50% | 43.26% | 0.0010 |
| 6 | 53.35% | 63.56% | 0.0010 |
| 10 | 61.30% | 68.26% | 0.0010 |
| 14 | 66.13% | 71.50% | 0.0005 |
| 22 | 70.60% | 75.26% | 0.00025 |
| 25 | 71.32% | 74.36% | 0.00025 |

The learning-rate scheduler stepped down twice (at epochs 14 and 21) as validation loss plateaued, which is visible in the accuracy gains that follow each drop.

---

## Classification Report (Test Set)

Per-class precision, recall, and F1-score computed on the full 10,000-image test set (1,000 images/class):

| Class | Precision | Recall | F1-score |
|---|---|---|---|
| airplane | 0.75 | 0.79 | 0.77 |
| **automobile** | 0.87 | 0.92 | **0.89** |
| bird | 0.73 | 0.54 | 0.62 |
| **cat** | 0.65 | 0.44 | **0.53** |
| deer | 0.68 | 0.70 | 0.69 |
| dog | 0.80 | 0.52 | 0.63 |
| frog | 0.58 | 0.90 | 0.71 |
| horse | 0.76 | 0.83 | 0.80 |
| ship | 0.85 | 0.89 | 0.87 |
| truck | 0.79 | 0.88 | 0.83 |
| **accuracy** | | | **0.74** |
| macro avg | 0.75 | 0.74 | 0.73 |
| weighted avg | 0.75 | 0.74 | 0.73 |

**Key observations:**
- **Strongest classes** are rigid, man-made objects with consistent silhouettes — `automobile` (F1 0.89), `ship` (0.87), and `truck` (0.83).
- **Weakest class is `cat`** (F1 0.53, recall only 0.44) — the model misses more than half of all cat images, most likely confusing them with `dog` (also comparatively weak at F1 0.63) or other small quadrupeds.
- **`frog` shows a precision/recall imbalance** (0.58 precision vs. 0.90 recall): the model over-predicts "frog," catching almost every true frog but at the cost of frequent false positives from other classes.
- Overall accuracy (0.74) and macro/weighted F1 (0.73) are closely aligned, indicating no class-imbalance distortion — expected, since the CIFAR-10 test set is perfectly balanced at 1,000 images per class.

---

## Key Techniques Used

- **Normalization:** Pixel values scaled from [0, 255] to [0, 1]
- **Data Augmentation:** Random flip, rotation, and zoom to improve generalization
- **Batch Normalization:** Stabilizes and speeds up training
- **Dropout:** Increasing dropout rates (0.25 → 0.5) to prevent overfitting
- **Callbacks:** EarlyStopping restores best weights; ReduceLROnPlateau adapts learning rate

---

## How to Run

1. Open `Image_Tagging_CNN_FINAL.ipynb` in Google Colab
2. Set runtime to T4 GPU: Runtime → Change runtime type → T4 GPU
3. Run all cells: Runtime → Run all

---

## Tech Stack

- Python 3
- TensorFlow 2.20.0 / Keras
- NumPy
- Matplotlib
- seaborn
- scikit-learn