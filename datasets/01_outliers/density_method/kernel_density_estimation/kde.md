# Outlier Detection Study Notes: KDE → Isolation Forest

## 1. Overview

This document summarizes the core concepts learned in Chapter 2 of *Introduction to Data Cleaning (Xu Chu)*, focusing on:

- Kernel Density Estimation (KDE)
- Bias–Variance tradeoff
- Bandwidth selection
- KDE-based anomaly detection
- Limitations of density-based methods
- Transition to Isolation Forest

---

## 2. Kernel Density Estimation (KDE)

KDE is a **non-parametric method** for estimating the probability density function (PDF) of a dataset.

### Core Idea

Each data point contributes a smooth “kernel bump” to the overall density:

\[
\hat{f}_h(x)=\frac{1}{nh}\sum_{i=1}^{n}K\left(\frac{x-x_i}{h}\right)
\]

Where:
- \(K\): kernel function (e.g., Gaussian)
- \(h\): bandwidth (smoothing parameter)

---

## 3. Histogram vs KDE

| Method | Characteristics |
|--------|----------------|
| Histogram | Discrete, blocky, depends on bins |
| KDE | Smooth, continuous, probabilistic |

KDE provides a smoother and more flexible estimate of data distribution.

---

## 4. Bandwidth and Its Role

Bandwidth controls the smoothness of KDE:

### Small bandwidth (h ↓)
- Low bias
- High variance
- Overfits noise
- Very wiggly density curve

### Large bandwidth (h ↑)
- High bias
- Low variance
- Over-smooths structure
- Misses important patterns

---

## 5. Bias–Variance Tradeoff

- **Bias**: error from overly simple models  
- **Variance**: sensitivity to data fluctuations  

Tradeoff principle:

> Reducing bias increases variance and vice versa.

This principle appears across ML models (KNN, trees, neural networks, KDE, SVM, etc.).

---

## 6. Optimal Bandwidth Selection

### 1. Rule of Thumb (Silverman’s Rule)

\[
h = 1.06 \cdot \sigma \cdot n^{-1/5}
\]

- Fast approximation
- Assumes near-Gaussian data

---

### 2. Cross-Validation (Preferred Method)

Steps:
1. Split data into folds (KFold)
2. Train KDE on training set
3. Evaluate log-likelihood on test set
4. Choose bandwidth with highest average score

Objective:

\[
\sum \log p(x_i)
\]

This maximizes likelihood of unseen data.

---

## 7. KDE for Outlier Detection

Steps:
1. Fit KDE on dataset
2. Compute density for each point
3. Flag low-density points as anomalies

Rule:
> Low probability density → potential outlier

---

## 8. KDE Limitations

### 1. Curse of Dimensionality
- Density becomes unreliable in high dimensions
- Distances lose meaning

---

### 2. Bandwidth Sensitivity
- Small change in bandwidth drastically affects results

---

### 3. Computational Cost
- O(n²) complexity
- Expensive for large datasets

---

### 4. Boundary Problems
- Density spills outside valid range

---

### 5. Multimodal & Complex Structures
- Over-smoothing merges distinct clusters

---

### 6. Collective Anomalies
- KDE only detects point-wise anomalies
- Cannot detect sequential or group anomalies

---

### 7. Mixed Data Types
- Works only for continuous numerical data

---

## 9. Why KDE Fails in Real-World Data

KDE assumes:
> Smooth, continuous, well-behaved distributions

But real-world data often has:
- high dimensionality
- clusters of varying density
- categorical features
- temporal dependencies
- complex structures

---

## 10. Transition to Isolation Forest

Because KDE fails in:
- high dimensions
- scalability
- structural anomalies

We move to Isolation Forest.

---

## 11. Isolation Forest Intuition

Instead of density estimation, Isolation Forest asks:

> How easy is it to isolate a data point?

### Key idea:
- Outliers are easier to isolate
- Normal points require more splits

---

## 12. Comparison: KDE vs Isolation Forest

| Feature | KDE | Isolation Forest |
|--------|-----|------------------|
| Principle | Density estimation | Random partitioning |
| Core idea | Low probability = anomaly | Short path length = anomaly |
| Complexity | O(n²) | O(n log n) |
| Strength | Low-dimensional smooth data | High-dimensional data |
| Weakness | Scalability + dimensions | Less probabilistic interpretability |

---

## 13. Key Insight

Different anomaly detection methods represent different assumptions:

- KDE → probability space  
- Isolation Forest → geometric separability  
- LOF → local density  
- DBSCAN → density clustering  
- Autoencoders → reconstruction error  

---

## 14. Conclusion

KDE introduces the foundation of density-based anomaly detection but fails under real-world complexity. Isolation Forest improves scalability and robustness by shifting from probability estimation to structural isolation.

This marks the transition from statistical density methods to tree-based anomaly detection methods.
