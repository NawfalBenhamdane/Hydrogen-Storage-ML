
# Hydrogen Storage Prediction Using Machine Learning

## Overview  
This project focuses on predicting the hydrogen storage capacity of metal and complex hydrides using supervised machine learning techniques.  
By combining experimental thermodynamic data with atomic and elemental descriptors, this work aims to accelerate the discovery of efficient materials for solid-state hydrogen storage, a crucial component of the energy transition.

---

## Methodology

The project follows a structured and data-driven workflow composed of five main steps.

### 1. Data Collection  
The dataset was obtained from **ML_HYDPARK (2024)**, which includes **772 samples** of metal and complex hydrides.  
Each record contains:
- Chemical composition  
- Thermodynamic parameters (enthalpy ΔH, entropy ΔS, equilibrium pressure)  
- Experimental hydrogen storage capacity (wt%)  

The target variable used for prediction is **Hydrogen_Weight_Percent**, representing the gravimetric hydrogen capacity of a material.

---

### 2. Data Cleaning and Preparation  
Data preprocessing included several refinement steps:
- **Missing value imputation** using linear regression between correlated properties.  
- **Outlier detection** based on interquartile range (IQR) filtering to ensure model robustness.  
- **Feature normalization** to align feature scales and improve model performance.  

Outliers and inconsistencies were removed, ensuring reliable input for the learning algorithms.

---

### 3. Exploratory Data Analysis  
A detailed EDA was conducted to study the physical and thermodynamic characteristics of the materials.  
Main observations included:
- Strong negative correlation (**R = -0.94**) between `Heat_of_Formation_kJperMolH2` and `LnEquilibrium_Pressure_25C`, consistent with Van’t Hoff’s thermodynamic relation.  
- Pressure data were transformed into logarithmic scale to linearize relationships.  
- Positive correlation observed between `ΔH` and `ΔS`, confirming expected energetic trends.  

This analysis guided the selection of relevant features before feature engineering.

---

## Feature Engineering

### 1. Motivation  
The original dataset, while detailed, contained limited information for robust learning.  
To improve model performance, the dataset was enriched with **elemental descriptors** representing the physical and chemical properties of each constituent atom in the hydrides.

---

### 2. Feature Generation  
Using the **Mendeleev Python library**, additional features were generated for each compound.  
The elemental properties extracted include:
- Atomic number  
- Period and group  
- Atomic weight and volume  
- Electronegativity  
- Ionization energy  
- Electron affinity  
- Covalent radius  
- Melting point  

From these properties, statistical descriptors were created:
- **Weighted sums** (e.g., `atomic_weight_weighted_sum`)  
- **Minimum and maximum values** (e.g., `atomic_number_min`, `period_max`)  
- **Standard deviations** and **ratios** to describe compositional variability  

This enrichment produced **over 50 new descriptors**, each providing structural and chemical context beyond raw thermodynamic data.

---

### 3. Feature Selection  
To identify the most predictive features, **Pearson correlation** and **SHAP (Shapley Additive Explanations)** analyses were applied.  
The following features showed the strongest influence on hydrogen capacity:

| Feature | Correlation with H₂ Capacity |
|----------|-----------------------------|
| period_weighted_sum | 0.665 |
| atomic_number_weighted_sum | 0.643 |
| atomic_weight_weighted_sum | 0.625 |
| electronegativity_weighted_sum | 0.516 |
| atomic_number_max | 0.448 |
| atomic_weight_max | 0.442 |
| period_max | 0.435 |

**Interpretation:**  
Materials composed of **light elements** (e.g., Li, Mg, Na) exhibit the highest hydrogen gravimetric capacity.  
This aligns with known chemical behavior and validates the relevance of atomic-level descriptors in predicting hydrogen storage performance.

---

## Modeling

Four supervised learning algorithms were tested and compared:

| Model | R² (Train) | R² (Test) | RMSE | MAE |
|-------|-------------|-----------|------|-----|
| **XGBoost** | 0.926 | 0.926 | 0.201 | 0.145 |
| Random Forest | 0.928 | 0.779 | 0.306 | 0.198 |
| CatBoost | 0.978 | 0.793 | 0.295 | 0.190 |
| Neural Network | 0.587 | 0.500 | 0.459 | 0.330 |

Among all models, **XGBoost** achieved the best balance between accuracy and generalization.  
Its gradient boosting architecture effectively captured nonlinear relationships between atomic, thermodynamic, and structural parameters.

---

## Results and Discussion  

The model revealed several key insights:
- **Feature enrichment** significantly improved model accuracy (R² +20% compared to baseline).  
- **Light elements** correlate with increased hydrogen storage capacity due to their low atomic weight and high reactivity.  
- The model enables identification of new candidate materials before experimental validation, reducing cost and time in material discovery.

Compared to earlier linear models from literature, the present approach demonstrated substantial improvements in prediction precision and interpretability.

---

## Perspectives

The work opens several promising research directions:
- Expand the dataset with additional hydride families and DFT-calculated descriptors.  
- Incorporate new target variables such as **thermal stability**, **absorption/desorption kinetics**, and **reversibility**.  
- Use **supercomputing infrastructure** for large-scale feature optimization.  
- Integrate multi-objective optimization techniques to balance energy density and material stability.

---

## Tech Stack

- **Language:** Python 3.10  
- **Libraries:** Pandas, NumPy, Scikit-learn, XGBoost, CatBoost, Mendeleev, Matplotlib, Seaborn  
- **Environment:** Google Colab / Jupyter Notebook  

---

## Authors

- Mourad Hammale  
- Ikram Firdaous  
- Nouhaila Gargouz  
- Mohamed Benkirane  
- **Nawfal Benhamdane**  
- Supervisor: Hamza Bouhani  
- École Centrale Casablanca, Morocco  

---

## Reference

N. Benhamdane et al., *Prediction of Hydrogen Storage in Metal and Complex Hydrides: A Supervised Machine Learning Approach*, École Centrale Casablanca, 2025.  
Based on [ML_HYDPARK Dataset, Zenodo (2024)](https://zenodo.org/records/10680097).
