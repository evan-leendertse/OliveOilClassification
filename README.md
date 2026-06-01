# Olive Oil Origin Classification

Classifying the geographic origin of extra virgin olive oils from spectral signals, using **Cubic B-Splines** and **Functional PCA** for feature extraction, followed by an **SVM** classifier.

---

## Overview

Each of the 60 samples in the dataset is represented as a spectral signal. The task is to predict which of four European countries the oil originates from, using only the signal.

| Class | Country |
|---|---|
| 1 | Greece |
| 2 | Italy |
| 3 | Portugal |
| 4 | Spain |

Two feature extraction pipelines are compared:

| Method | Feature Extraction | Classifier |
|---|---|---|
| **B-Spline + SVM** | Cubic B-spline basis coefficients | SVM |
| **FPCA + SVM** | Functional principal component scores | SVM |

---

## Results

| Method | Features | Test Accuracy |
|---|---|---|
| B-Spline + SVM | 74 spline coefficients | **86.7%** |
| FPCA + SVM | 2 components | 83.3% |
| FPCA + SVM | 5 components | **86.7%** |
| FPCA + SVM | 8 components | 83.3% |
| FPCA + SVM | 10 components | 83.3% |

> FPCA with 5 components matches the B-Spline accuracy using far fewer features, suggesting the signals contain most of their class-discriminating information in a low-dimensional functional subspace.


## Method

### Cubic B-Splines

Each signal $x(t)$ is approximated as a weighted sum of cubic B-spline basis functions:

$$f(x) = \sum_{j=1}^{K} c_j B_j(x)$$

The least-squares coefficients $c_j$ are computed for each sample and used as features for the SVM.

**Basis configuration:** order $M = 4$, 70 uniformly spaced interior knots, giving $K = 74$ degrees of freedom.

### Functional PCA

FPCA generalises classical PCA to function-valued data. Each signal is decomposed as:

$$x_i(t) \approx \mu(t) + \sum_{k=1}^{K} \xi_{ik} \, \phi_k(t)$$

where $\phi_k$ are the functional eigenfunctions and $\xi_{ik}$ are the PC scores used as SVM features. The number of components $K$ is treated as a hyperparameter, swept over $\{2, 5, 8, 10\}$.

---
