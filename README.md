# MJKAN: Bridging KAN and MLP

---

## 📖 Overview

**MJKAN (Modulation Joint KAN)** is a novel neural network layer designed to bridge the gap between **Kolmogorov-Arnold Networks (KANs)** and **Multilayer Perceptrons (MLPs)**. 

While KANs offer superior theoretical expressiveness via learnable activation functions, they often suffer from high computational costs and optimization difficulties. MJKAN overcomes these challenges by integrating **FiLM (Feature-wise Linear Modulation)** with **Radial Basis Function (RBF)** activations.

**Key Features:**
* **Hybrid Architecture:** Combines the non-linear expressive power of KANs with the efficiency of MLPs.
* **Tunable Complexity:** The number of basis functions ($k$) acts as a direct dial for model complexity.
* **Efficiency:** Significantly faster inference and lower resource usage compared to standard B-spline KANs.
* **Versatility:** Demonstrated effectiveness in Function Regression, PDE solving, Image Classification, and NLP.

---

## ⚙️ Methodology

### The MJKAN Layer
The MJKAN layer is inspired by the Kolmogorov–Arnold representation theorem. Unlike standard KANs that use B-splines, MJKAN uses a **FiLM-modulated RBF decomposition**.

Given an input vector $\mathbf{x}$, the layer output $y$ is calculated as:

$$
y = \sum_{i=1}^{d_{\text{in}}} \text{FiLM}_i(x_i) + \text{Base}(x)
$$

Where:
1.  **RBF Expansion:** Each input $x_i$ is expanded into $K$ Gaussian basis functions:
    $$\phi_{ij}(x_i) = \exp\left( -\frac{(x_i - c_j)^2}{2\sigma^2} \right)$$
2.  **FiLM Modulation:** The expansion allows for learnable scaling ($\gamma$) and shifting ($\beta$):
    $$\text{FiLM}_i(x_i) = \gamma_i x_i + \beta_i$$

> **Note:** With trivial modulation, MJKAN behaves like a KAN. With identity RBFs, it reduces to a linear MLP.

---

## 📊 Experimental Results

### 1. Function Regression (Strongest Performance)
MJKAN demonstrates superior approximation capabilities compared to MLPs. As the number of basis functions ($K$) increases, the Root Mean Square Error (RMSE) decreases significantly, especially for complex compositional functions.

| Task | MLP (128) | MJKAN ($k=5$) | MJKAN ($k=10$) | MJKAN ($k=25$) | MJKAN ($k=50$) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Local Bumps** | 0.1955 | 0.2903 | 0.2682 | 0.1930 | **0.1489** |
| **Global Pattern** | 0.0840 | 1.0631 | 0.7286 | 0.2329 | **0.0734** |
| **Step Function** | 0.1179 | 0.1082 | 0.4653 | 0.0739 | **0.0638** |
| **High-Freq Sine** | 0.7034 | 0.7094 | 0.7087 | 0.7098 | **0.6918** |
| **Compositional** | 0.4712 | 0.5111 | 0.4692 | 0.4489 | **0.2628** |

### 2. Solving PDEs (Burgers' Equation)
Consistent with literature suggesting KANs excel at scientific computing, MJKAN outperforms MLP in solving the 1D viscous Burgers' equation.

| Model | MAE (Mean Absolute Error) | MSE (Mean Squared Error) |
| :--- | :---: | :---: |
| **MJKAN ($k=5$)** | **0.0044** | **0.00003** |
| MJKAN ($k=10$) | 0.0226 | 0.00059 |
| MJKAN ($k=25$) | 0.0154 | 0.00031 |
| MJKAN ($k=50$) | 0.0091 | 0.00011 |
| **MLP** | 0.0263 | 0.00094 |

### 3. Image Classification (Accuracy vs. Efficiency)
On standard vision datasets, MJKAN is competitive with MLP.
* **Observation:** Smaller basis sizes ($k=5$) generally generalize better for classification.
* **Trade-off:** Larger basis sizes increase expressiveness but can lead to overfitting on sparse data (e.g., CIFAR-100).

| Dataset | Model | Accuracy (%) | Training Time (s) |
| :--- | :--- | :---: | :---: |
| **MNIST** | MJKAN | 96.6 | 124.57 |
| | MLP | **97.9** | 120.84 |
| **CIFAR-10** | MJKAN ($k=5$) | 50.2 | 127.7 |
| | MLP | **50.3** | 115.2 |
| **CIFAR-100** | MJKAN ($k=5$) | 19.2 | 125.3 |
| | MLP | **22.7** | 115.5 |

### 4. Computational Cost
The cost of MJKAN scales linearly with the number of basis functions.

| Basis Size ($k$) | Total Parameters | GFLOPs |
| :---: | :---: | :---: |
| 5 | 6,043 | 0.0008 |
| 10 | 11,973 | 0.0015 |
| 25 | 29,763 | 0.0037 |
| 50 | 59,413 | 0.0074 |

## 📝 Citation

**Paper:** [Bridging KAN and MLP: MJKAN, a Hybrid Architecture with Both Efficiency and Expressiveness](https://doi.org/10.1016/j.icte.2025.11.010)  
