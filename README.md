# CSC3831 — Predictive Analytics, Computer Vision & AI

**Newcastle University · Fall 2024 (study abroad) · Python**
Course materials (lecture slides, Colab notebooks, coursework brief):
[**CSC3831 Google Drive folder**](https://drive.google.com/drive/folders/1ZFzIz2oyMbxYrSIvm4Wo_9jR9OrkdU-n)

## What this course actually taught

Three connected strands, taught entirely in Jupyter notebooks against real libraries — `scikit-learn`
for the classical work and `PyTorch` for the deep learning. Unlike
[CS362](https://github.com/ryanfahimi/CS362), which builds the algorithms from scratch, this course
is about **applying** them well: choosing methods, tuning them, and — more than anything — judging
whether a result is real.

**Data Engineering** comes first, and the ordering is the argument. Before any model, you deal with
what the data is actually like: exploration, missing values, duplicate records that refer to the same
entity, and outliers. Most of the effort in real predictive work is here, and putting it first says
so.

**Machine Learning** is the classical toolkit — regression, classification, validation and
hyper-parameter tuning, then unsupervised methods (PCA, k-means, hierarchical clustering, DBSCAN).
Validation is treated as a first-class topic rather than a step, which is the right emphasis.

**Computer Vision** is deep learning on images: fully connected networks first, then convolutional
ones, then the practical machinery — early stopping, batch normalization, and looking at what the
filters actually learned.

## Repo layout

Each strand has a `Practicals/` folder of guided lab notebooks and a coursework notebook that was the
assessed work.

| Strand | Practicals | Coursework |
|---|---|---|
| [Data Engineering](Data%20Engineering/) | EDA · Imputation · Record Linkage · Outlier Detection | [Part 1](Data%20Engineering/Coursework_Part_1.ipynb) |
| [Machine Learning](Machine%20Learning/) | Regression · Classification · Unsupervised Learning | [Part 2](Machine%20Learning/Coursework_Part_2.ipynb) |
| [Computer Vision](Computer%20Vision/) | Image Classification (DNN) · Image Classification (ConvNet) | [Part 3](Computer%20Vision/Coursework_Part_3.ipynb) |

Each strand folder has its own README with the study-guide layer. Drive additionally holds
`Lectures/`, `Colab Notebooks/`, and `CV Coursework Part 3/`.

## The assessed coursework, in one place

The three coursework notebooks are the best summary of the course, and each has a clear spine:

**[Part 1 — Data Engineering](Data%20Engineering/Coursework_Part_1.ipynb)** (36 cells): data
understanding [7 marks] → outlier identification [10] → imputation [10] → conclusions [3]. Uses
`LocalOutlierFactor`, `IsolationForest`, and `OneClassSVM` for outliers, and `KNNImputer` and
`IterativeImputer` for missing values, with `missingno` for visualising missingness patterns. The
mark distribution is worth noting — the two *methodological* sections carry twenty of the thirty
marks.

**[Part 2 — Machine Learning](Machine%20Learning/Coursework_Part_2.ipynb)** (28 cells): load MNIST
and visualise → logistic regression baseline → PCA for dimensionality reduction → add random normal
noise and re-evaluate. That last step is the interesting one: deliberately degrading your own data to
see how gracefully a model fails is exactly the robustness question that a clean-data accuracy score
never asks.

**[Part 3 — Computer Vision](Computer%20Vision/Coursework_Part_3.ipynb)** (40 cells): a CNN on
CIFAR-10 with early stopping → the same network with and without batch normalization → visualising
the learned convolutional features. Built in PyTorch with `random_split`, `Subset`, `optim`, and
`transforms`.

## The habit the course is really teaching

Every coursework part is structured as a **comparison**, not a demonstration: outlier methods against
each other, a model before and after PCA, clean data against noisy, a network with and against
without batch norm. None of them asks "does this work?" — they ask "compared to what?" That framing,
plus the baseline discipline, is the transferable part.

## Running

The notebooks were written for Colab and need no local setup there. Locally:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn missingno torch torchvision
jupyter lab
```

Datasets are fetched by the notebooks themselves (MNIST and CIFAR-10 via `torchvision.datasets`), so
nothing large is committed here.
