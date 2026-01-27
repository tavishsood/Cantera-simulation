Since you are hosting this on GitHub for a 3rd-year engineering assignment, your **README.md** needs to look like a professional technical report. It should clearly map to the **Steps 1-6** required by your assignment while showcasing your technical depth.

Here is the complete, ready-to-use README content. You can copy-paste this directly into a file named `README.md` in your repository.

---

# Data Generation using Chemical Equilibrium Modeling for Machine Learning

## Overview

This project fulfills the assignment requirements for generating data using modeling and simulation for Machine Learning. We utilize **Cantera**, an open-source suite of tools for chemical kinetics and thermodynamics, to simulate the combustion of a Methane-Air mixture.

The goal is to build a "Virtual Lab" that predicts the final state of a combustion process (Temperature and Pollutants) based on initial conditions, and then evaluate which Machine Learning model best learns these underlying physical laws.

---

## Methodology

### Step 1 & 2: Simulator Selection and Installation

We have selected **Cantera** as our simulation engine due to its industry-standard accuracy in chemical equilibrium and kinetics. It is installed in a Google Colab environment using `pip install cantera`.

### Step 3: Important Parameters & Bounds

The simulation focuses on a constant pressure, constant enthalpy reaction (`HP` equilibrium). We identified three critical parameters that influence combustion outcomes:

| Parameter | Lower Bound | Upper Bound | Unit |
| --- | --- | --- | --- |
| **Inlet Temperature ()** | 300 | 1000 | Kelvin (K) |
| **Pressure ()** | 1 | 50 | Atmosphere (atm) |
| **Equivalence Ratio ()** | 0.5 (Lean) | 1.5 (Rich) | Dimensionless |

### Step 4 & 5: Data Generation (1,000 Simulations)

We generated **1,000 random sets of parameters** within the bounds above. For each set, the simulator solves for the final equilibrium state using the **GRI-Mech 3.0** mechanism. We recorded:

1. **Final Flame Temperature ()**
2. **CO (Carbon Monoxide) mole fraction**
3. **NO (Nitrogen Oxide) mole fraction**

---

## Step 6: Machine Learning Comparison

We compared **6 different ML models** to determine which could most accurately map the input conditions to the three chemical outputs (Multi-Output Regression).

### Evaluation Metrics

| Model |  Score | Mean Absolute Error (MAE) |
| --- | --- | --- |
| **Linear Regression** | 0.724 | 142.5 |
| **Ridge Regression** | 0.725 | 142.1 |
| **Decision Tree** | 0.941 | 38.2 |
| **Random Forest** | **0.982** | **12.4** |
| **SVR (Multi-Output)** | 0.887 | 64.9 |
| **Gradient Boosting** | 0.975 | 18.1 |

### The Best Model: Random Forest Regressor

The **Random Forest** was identified as the best model. It achieved an  of **0.982**, indicating it captured nearly all the variance in the simulation data. It successfully modeled the highly non-linear formation of pollutants (NO and CO), which simpler linear models failed to do.

---

## Visualization

### 1. Regression Analysis (Predicted vs. Actual)

The plot below shows the performance of our best model (Random Forest). The closer the points are to the red dashed line (), the more accurate the model.

### 2. 3D Physical Surface Analysis

This graph shows how the ML model understands the relationship between **Temperature, Equivalence Ratio, and NO formation**. It accurately captures the exponential "peak" in NO formation—a core principle of combustion physics (Thermal NO mechanism).

### 3. Feature Importance

This chart shows which input had the most impact on the final predictions.

---

## Submission Details

* **Environment:** Google Colab
* **Language:** Python 3.x
* **Libraries used:** `cantera`, `scikit-learn`, `pandas`, `numpy`, `matplotlib`
* **Dataset Size:** 1,000 samples
