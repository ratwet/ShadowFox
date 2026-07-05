# ShadowFox AIML Internship

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.20-FF6F00?logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.11-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen)]()

Three end-to-end Machine Learning / Deep Learning projects built for the **ShadowFox Artificial Intelligence and Machine Learning (AIML) Internship** — progressing from a from-scratch CNN image classifier, through a full retail analytics pipeline, to deploying and evaluating a 2-billion-parameter open-weight language model.

---

## Table of Contents
- [About](#about)
- [Results at a Glance](#results-at-a-glance)
- [Repository Structure](#repository-structure)
- [Tasks Overview](#tasks-overview)
- [Prerequisites](#prerequisites)
- [Proof of Work](#proof-of-work)
- [License](#license)

---

## About

| | |
|---|---|
| **Intern Role** | ShadowFox AIML Intern |
| **Organization** | ShadowFox |
| **Program** | AIML Internship |
| **Tasks Completed** | 3 / 3 — Beginner → Intermediate → Advanced |

Each task was scoped, built, trained or analyzed, and documented independently, then packaged as a fully executed, reproducible Google Colab notebook.

---

## Results at a Glance

| Task | Domain | Headline Result |
|---|---|---|
| [**1 — Image Tagging**](Task1_Image_Tagging/README.md) | Computer Vision (CNN) | **74.07% test accuracy** on CIFAR-10, trained from scratch (no pretrained weights) |
| [**2 — Store Sales & Profit**](Task2_Store_Sales_Analysis/README.md) | Data Analysis / BI | **8,399 orders** analyzed — ≈ **$14.9M** in sales and **$1.5M** in profit across 4 customer segments |
| [**3 — NLP / LLM Exploration**](Task3_NLP_LM_Project/README.md) | Natural Language Processing | **Gemma 2 (2B-it)** deployed, tested across 5 task types, and benchmarked against a GPT-2 baseline |

---

## Repository Structure

```
ShadowFox/
│
├── Task1_Image_Tagging/
│   ├── Image_Tagging_CNN_FINAL.ipynb
│   └── README.md
│
├── Task2_Store_Sales_Analysis/
│   ├── Store_Sales_Profit_Analysis_FINAL.ipynb
│   └── README.md
│
├── Task3_NLP_LM_Project/
│   ├── AI_NLP_Language_Model_FINAL.ipynb
│   └── README.md
│
├── LICENSE
└── README.md
```

---

## Tasks Overview

### Task 1 — Image Tagging with CNN (Beginner)
Built a Convolutional Neural Network **from scratch** with TensorFlow/Keras — no pretrained weights — to classify CIFAR-10 images into 10 categories. Trained for 25 epochs on a Tesla T4 GPU, reaching **74.07% test accuracy** (0.7495 test loss). Per-class evaluation shows strongest performance on `automobile` (F1 0.89) and `ship` (F1 0.87), with `cat` vs `dog` remaining the hardest confusion pair — consistent with known CIFAR-10 difficulty patterns.

**Tech Stack:** Python, TensorFlow, Keras, NumPy, Matplotlib, scikit-learn
**Notebook:** [Open in Colab](https://colab.research.google.com/github/ratwet/ShadowFox/blob/Preview/Task1_Image_Tagging/Image_Tagging_CNN_FINAL.ipynb) · [Task README](Task1_Image_Tagging/README.md)

---

### Task 2 — Store Sales and Profit Analysis (Intermediate)
Cleaned and analyzed **8,399 retail orders** (2010–2013) with Pandas and Plotly — IQR-based outlier detection, temporal trend analysis, and customer-segment profitability breakdowns. Key finding: **Small Business customers post the highest profit margin (11.32%)** despite having the lowest total sales of any segment, making them the most capital-efficient segment per revenue dollar.

**Tech Stack:** Python, Pandas, NumPy, Plotly
**Notebook:** [Open in Colab](https://colab.research.google.com/github/ratwet/ShadowFox/blob/Preview/Task2_Store_Sales_Analysis/Store_Sales_Profit_Analysis_FINAL.ipynb) · [Task README](Task2_Store_Sales_Analysis/README.md)

---

### Task 3 — AI-Driven NLP Project (Advanced)
Deployed **Google Gemma 2 (2B-it)**, an open-weight instruction-tuned language model, on a Colab T4 GPU. Evaluated it across 5 task types (factual recall, multi-step reasoning, creative writing, multi-turn memory, and summarization), visualized layer-13 attention weights to confirm correct pronoun resolution, and benchmarked generation quality/speed against a GPT-2 (124M) baseline on an identical prompt.

**Tech Stack:** Python, PyTorch, Hugging Face Transformers, Accelerate, Matplotlib
**Notebook:** [Open in Colab](https://colab.research.google.com/github/ratwet/ShadowFox/blob/Preview/Task3_NLP_LM_Project/AI_NLP_Language_Model_FINAL.ipynb) · [Task README](Task3_NLP_LM_Project/README.md)

---

## Prerequisites

All notebooks were developed and executed on Google Colab. Confirmed runtime versions are noted where the notebooks report them.

```
tensorflow==2.20.0        # confirmed at runtime (Task 1)
torch==2.11.0+cu128       # confirmed at runtime (Task 3)
transformers==5.13.0      # confirmed at runtime (Task 3)
accelerate
huggingface_hub
pandas
numpy
plotly
matplotlib
seaborn
scikit-learn
```

Tasks 1 and 3 require a GPU runtime (Runtime → Change runtime type → T4 GPU). Task 2 runs on CPU.

---

## Proof of Work

Each task is accompanied by:
- [x] A fully executed Jupyter/Colab notebook with all outputs preserved

---

## License

This project is licensed under the [MIT License](LICENSE).