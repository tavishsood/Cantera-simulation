# Data Generation using Chemical Modelling and Simulation for ML

## Overview

This project fulfills the requirements for generating data using modelling and simulation for Machine Learning. We utilize **Cantera**, an open-source suite of tools for chemical kinetics and thermodynamics, to simulate the combustion of a Methane-Air mixture.

The goal is to build a "Virtual Lab" that predicts final combustion states (Temperature and Pollutants) based on initial conditions, and then evaluate which Machine Learning model best learns these underlying physical laws.

---

## Methodology

### Step 1 & 2: Simulator Selection and Installation

We have selected **Cantera** as our simulation engine due to its industry-standard accuracy in chemical equilibrium and kinetics. The environment is set up in Google Colab using `pip install cantera`.

### Step 3: Important Parameters & Bounds

The simulation focuses on a constant pressure, constant enthalpy reaction (**HP equilibrium**). We identified three critical parameters that influence combustion outcomes:

| Parameter | Lower Bound | Upper Bound | Unit |
| --- | --- | --- | --- |
| **Inlet Temperature (T_in)** | 300 | 1000 | Kelvin (K) |
| **Pressure (P)** | 1 | 50 | Atmosphere (atm) |
| **Equivalence Ratio (ϕ)** | 0.5 (Lean) | 1.5 (Rich) | Dimensionless |

### Step 4 & 5: Data Generation (1,000 Simulations)

We generated **1,000 random sets of parameters** using a uniform distribution within the bounds above. For each set, the simulator solves for the final equilibrium state using the **GRI-Mech 3.0** mechanism. We recorded three target variables:

1. **Final Flame Temperature (T_final)**
2. **CO (Carbon Monoxide)** mole fraction
3. **NO (Nitrogen Oxide)** mole fraction

---

## Step 6: Machine Learning Comparison & Ranking (TOPSIS)

To identify the overall "Best" model, we evaluated 6 algorithms across 5 distinct metrics: **R² Score**, **MAE**, **MSE**, **RMSE**, and **Training Time**. We then applied the **TOPSIS** (Technique for Order of Preference by Similarity to Ideal Solution) algorithm with equal weights (w=1) to rank the models based on accuracy and efficiency.

### Multi-Metric Evaluation Results

| Model | R² | MAE | MSE | RMSE | Time (s) | TOPSIS Score | Rank |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Decision Tree** | 0.9364 | 9.25 | 433.13 | 20.81 | 0.0061 | **0.9462** | **1** |
| **Random Forest** | 0.9867 | 4.43 | 108.30 | 10.40 | 0.4497 | 0.7130 | 2 |
| **Gradient Boosting** | 0.9957 | 4.61 | 111.44 | 10.55 | 0.6657 | 0.6218 | 3 |
| **Linear Regression** | 0.7050 | 45.07 | 8178.75 | 90.43 | 0.0032 | 0.5806 | 4 |
| **Ridge Regression** | 0.7052 | 45.08 | 8188.40 | 90.48 | 0.0023 | 0.5806 | 5 |
| **SVR** | -0.2367 | 58.24 | 16361.36 | 127.91 | 0.0381 | 0.3648 | 6 |

### Final Model Selection: Decision Tree

While Gradient Boosting and Random Forest achieved higher R² scores, the **Decision Tree** was identified by TOPSIS as the optimal model. This is due to its exceptional balance between high accuracy and near-instantaneous training speed, making it the most efficient "surrogate model" for real-time combustion predictions.

---

## Visualizations

### 1. Regression Analysis (Predicted vs. Actual)

![Regression Analysis](images/regression_analysis.png)
This plot shows the performance of the TOPSIS winner. The tight clustering along the red dashed line (45°) demonstrates high predictive accuracy across all three chemical targets.

### 2. 3D Physical Surface Analysis

![3D Surface Plot](images/3d_surface_plot.png)
This graph visualizes how the model understands the relationship between Temperature, Equivalence Ratio, and NO formation. It accurately captures the non-linear "peak" in NO formation.

### 3. Feature Importance

![Feature Importance](images/feature_importance.png)
This chart highlights which input had the most impact on the final predictions. The **Equivalence Ratio (ϕ)** remains the dominant factor in determining chemical outcomes.

---

## Conclusion

The use of **TOPSIS** allowed for a multi-dimensional comparison beyond simple accuracy. The results indicate that ensemble methods (RF, GB) are slightly more accurate for chemical manifolds, but simple trees offer significantly better computational efficiency. The failure of the **SVR (R² < 0)** underscores the highly non-linear nature of combustion physics, which requires tree-based partitioning to model successfully.

---

## Submission Details

* **Environment:** Google Colab
* **Language:** Python 3.x
* **Libraries:** `cantera`, `scikit-learn`, `pandas`, `numpy`, `matplotlib`
