# **BERN02 - Challenge Project**
## Comparative Evaluation of Regression Models for Predicting Parkinson’s Disease Severity
### **Author:** Yang Shann Wen

---
### Overview

**Parkinson's disease (PD)** is a progressive neurodegenerative disorder that causes a person to experience movement-related symptoms, such as tremor, slowness of movement and balance problems, as well as non-motor symptoms like hallucinations, depression and loss of sense of smell. This project focuses on **evaluating the predictive performance of different regression models for estimating Parkinson's disease severity using demographic and voice measurement features.** Linear regression and nonlinear models are compared using test-set performance metrics.

The target variable is `total_UPDRS`, a score used to assess the severity of Parkinson’s disease symptoms.

---
### Table of Contents

- [Objectives](#objectives)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [Limitations & Future Improvements](#limitations--future-improvements)
- [Reflection on FAIR Principles](#reflection-on-fair-principles)
- [Conclusion](#conclusion)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [References](#references)
- [Acknowledgements](#acknowledgements)
- [License](#license)

---
### Objectives

- Explore the relationships between demographic and acoustic features and Parkinson’s disease severity.
- Perform exploratory data analysis (EDA) to identify patterns, correlations, and potential multicollinearity among predictors.
- Develop and compare linear and nonlinear regression models.
- Evaluate model performance using RMSE and R².
- Investigate feature importance and model limitations.

---
### Dataset

The project uses the **Oxford Parkinson's Disease Telemonitoring Dataset**, obtained from the UCI Machine Learning Repository. The dataset contains 5,875 voice recordings collected from 42 individuals with early-stage Parkinson's disease during a six-month telemonitoring study. The recordings were collected in participants' homes and include demographic information, acoustic measurements derived from voice recordings, and clinical symptom severity scores.
> **Citation**
> 
> Tsanas, A. & Little, M. (2009). Parkinsons Telemonitoring [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5ZS3N.

#### Target Variable
`total_UPDRS`: Total Unified Parkinson’s Disease Rating Scale score.

#### Predictors
The following demographic and acoustic features were used as predictors of `total_UPDRS`.

| Variable                                                                                          | Description                                                                                           |
|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| `age`                                                                                             | Age of the participant, in years.                                                                     |
| `sex`                                                                                             | Sex of the participant (0=male, 1=female).                                                            |
| Jitter measurements: <br/>`Jitter(%)`, `Jitter(Abs)` , `Jitter:RAP`, `Jitter:PPQ5`, `Jitter:DDP`       | Several measures of variation in fundamental frequency.                                               |
| Shimmer measurements: <br/>`Shimmer`, `Shimmer(dB)`, `Shimmer:APQ3`, `Shimmer:APQ5`, `Shimmer:APQ11`, `Shimmer:DDA` | Several measures of variation in amplitude.                                                           |
| `NHR`                                                                                             | Noise-to-Harmonics Ratio, measuring the relative amount of noise in the voice signal.                 |
| `HNR`                                                                                             | Harmonics-to-Noise Ratio, measuring the relative strength of harmonic components compared with noise. |
| `RPDE`                                                                                            | Recurrence Period Density Entropy, a nonlinear dynamical complexity measure.                          |
| `DFA`                                                                                             | Detrended Fluctuation Analysis, measuring the signal's fractal scaling properties.                    |
| `PPE`                                                                                             | Pitch Period Entropy, measuring variation in the fundamental frequency of the voice signal.           |

`subject#`, `motor_UPDRS`, and `test_time` are excluded from the predictors. 
The subject identifier is excluded because it is not a meaningful measurement of symptom severity; 
`motor_UPDRS` is excluded because it represents a component of the total score and could introduce target leakage;
`test_time` is excluded because this analysis focuses on predictions based on demographic and acoustic features rather than elapsed time. 
Hence, we will have **18 predictors**, with the target `total_UPDRS`.

---
### Methodology

The project is organised into six main sections:

1. **Data Loading**: Load the Oxford Parkinson’s Disease Telemonitoring Dataset and inspect its structure and variables.

2. **Data Cleaning and Preprocessing**: Prepare the dataset for analysis by selecting the target variable (`total_UPDRS`) and predictor features, and performing the necessary data cleaning and preprocessing.

3. **Exploratory Data Analysis (EDA)**: Explore variable distributions, identify potential outliers, and examine correlations between predictors and the target variable.

4. **Model Development**: Develop and train Linear Regression, Lasso Regression, and Random Forest Regression models to predict `total_UPDRS`.

5. **Model Evaluation and Comparison**: Evaluate and compare model performance using Root Mean Squared Error (RMSE) and R², with cross-validation used to assess model performance. Examine residual patterns and feature importance to support model interpretation.

6. **Conclusion**: Summarise the findings, identify the best-performing model, discuss the limitations of the analysis, and suggest potential directions for future improvement.

---
### Results

The **Random Forest model** achieved substantially better predictive performance than the linear models on the test set.

| Model | Test RMSE | Test R² |
|---|----------:|---:|
| Linear Regression |     9.701 | 0.151 |
| Lasso Regression |     9.697 | 0.151 |
| Random Forest Regression |     2.969 | 0.920 |

Lower RMSE indicates smaller prediction errors, while higher R² indicates better predictive performance relative to a mean-only baseline.

The results suggest that Random Forest Regression captures relationships in the dataset that are not adequately represented by the linear models. Age was also the most important feature according to the fitted Random Forest model’s impurity-based feature importance.

---
### Limitations & Future Improvements
There are several **limitations** to this analysis:
- The dataset contains repeated recordings from only 42 individuals, limiting the diversity of independent subjects represented.
- A random row-level train-test split may place recordings from the same individual in both sets. Therefore, the reported test performance does not necessarily reflect performance on entirely unseen individuals.
- Correlated acoustic features may affect coefficient interpretation and feature-importance estimates.
- The findings are based on a specific dataset of individuals with early-stage Parkinson’s disease and may not generalise to other populations or recording conditions.

Several **improvements** could be explored to strengthen the predictive performance, generalisability, and interpretability of the analysis:

- Incorporate `subject#` and `test_time` to investigate how Parkinson’s disease severity changes over time at patient level, rather than treating each recording as an independent observation. Evaluate models using patient-wise data splitting to prevent recordings from the same individual appearing in both training and testing sets, providing a more reliable assessment of generalisation to unseen patients.

- Evaluate other nonlinear regression approaches, such as Gradient Boosting, XGBoost, or Support Vector Regression (SVR), to investigate whether they can improve predictive performance compared with Linear Regression, Lasso Regression, and Random Forest Regression.

- Apply systematic hyperparameter tuning using cross-validation to identify suitable model configurations and potentially improve predictive performance.

---
### Reflection on FAIR Principles

The FAIR principles — Findable, Accessible, Interoperable, and Reusable — provide a framework for improving the discoverability, accessibility, and reuse of research data and workflows. In this project, these principles apply to both the Oxford Parkinson’s Disease Telemonitoring Dataset and the Jupyter Notebook used for data analysis and regression modelling.

**1. Findable (F)**: This project is hosted on an open-source, publicly accessible GitHub repository with structured metadata, relevant topic keywords, descriptive file names, and a comprehensive `README.md` to make the project easy to find and navigate. The dataset used is publicly available through the UCI Machine Learning Repository, which provides a dataset description and citation.

**2. Accessible (A):** All code, notebooks, and datasets can be retrieved via standard, open protocols (https, Git). The reproduction instructions, dataset access information, and an overview of the workflow are provided in `README.md`.

**3. Interoperable (I):** The project uses a structured tabular dataset containing demographic and acoustic measurements, which can be processed using common data analysis tools such as Python, pandas, and scikit-learn. Standard file formats, including CSV and Jupyter Notebook (.ipynb), and documented variable names facilitate use across different tools and environments. The notebook follows a clearly organised workflow covering data loading, preprocessing, exploratory data analysis, model development, evaluation, and conclusion.

**4. Reusable (R):** The repository provides a README.md and explanatory Markdown cells in the notebook to help users understand and reuse the analytical workflow. The project's own materials are licensed under CC BY-NC-SA 4.0, subject to the author's rights to license them and the licence's conditions. Random seeds and a dependency file help improve reproducibility by documenting sources of randomness and required packages.

---
### Conclusion

This project investigated the prediction of Parkinson’s disease severity, measured by `total_UPDRS`, using demographic and acoustic features from the Oxford Parkinson’s Disease Telemonitoring Dataset.

Among the models evaluated, Random Forest Regression achieved the best predictive performance, substantially outperforming Linear Regression and Lasso Regression. These results suggest that Random Forest was better able to capture complex relationships between the predictors and the target variable.

Overall, this project demonstrates the potential of regression models for estimating Parkinson’s disease severity from demographic and acoustic measurements. The findings highlight the value of comparing different modelling approaches when analysing complex biomedical data.

**Note:** This project focuses on predicting a severity score and is intended for educational and statistical learning purposes. It is not a diagnostic tool and should not be used for clinical decision-making.

---
### Repository Structure

```text
Parkinsons_Regression_Model_Comparison/
│
├── data
│     └── parkinsons_updrs.data
├── LICENSE
├── Parkinsons_Regression_Model_Comparison.ipynb
├── README.md
└── requirements.txt
```

---
### Installation

1. Clone or download this repository.
```bash
git clone https://github.com/shannonyang37/Parkinsons_Regression_Model_Comparison
```

2. Install Python and Jupyter Notebook.
3. Install the required libraries:

   ```bash
   pip install -r requirements.txt
   ```

4. Launch Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open `Parkinsons_Regression_Model_Comparison.ipynb` and run the cells in order.

--- 
### References

1. Tsanas, A., Little, M., McSharry, P., & Ramig, L. (2009). Accurate telemonitoring of Parkinson’s disease progression by noninvasive speech tests. Nature Precedings, 57(4), 884–893. https://doi.org/10.1038/npre.2009.3920.1

2. Tsanas, A. & Little, M. (2009). Parkinsons Telemonitoring [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5ZS3N.

---
### Acknowledgements

The author acknowledges the UCI Machine Learning Repository for providing the Oxford Parkinson’s Disease Telemonitoring Dataset.

---
### License

This project is licensed under the **[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/).**

You are free to share and adapt the materials in this repository for non-commercial purposes, provided you give appropriate attribution and distribute adaptations under the same license.

See `LICENSE` or [full license terms](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode) for details.

**Note:** The dataset used in this project is subject to its own licensing terms. Please refer to the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/189/parkinson+telemonitoring) for dataset licensing information.


