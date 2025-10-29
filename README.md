# predicting-insurance-charges



## Overview of Dataset

This dataset contains the medical insurance cost information for 1338 individuals.

- **age**: Age of primary beneficiary
- **sex**: Gender of beneficiary (male, female)
- **bmi**: Body Mass Index, a measure of body fat based on height and weight
- **children**: Number of children covered by health insurance
- **smoker**: Smoking status of the beneficiary (yes, no)
- **region**: Residential region in the US (northeast, northwest, southeast, southwest)
- **charges**: Medical insurance cost billed to the beneficiary

## Objective

1. To **predict health insurance charges** accurately and identify which **features** are the most important.
2. Assess **fairness** in insurance charges; ensure that males and females, as well as individuals from different states, are not systematically charged higher than others when their other characteristics are similar.

## Exploratory Data Analysis

### Overview of Numeric Columns

From the boxplot and histogram of `charges`, we observe that `charges` is skewed to the right:

<Figure size 1600x800 with 16 Axes><img width="1590" height="789" alt="image" src="https://github.com/user-attachments/assets/64f50b37-d12c-4bcc-a0a1-5ab857a6b27b" />

<Figure size 640x480 with 1 Axes><img width="569" height="435" alt="image" src="https://github.com/user-attachments/assets/9d27843b-8804-4590-881f-3997f2c7b006" />

We thus decided to perform a **log-transformation** on `charges` which reduced skewness and mitigated the impact of outliers:

<p float="left">
  <Figure size 640x480 with 1 Axes><img width="45%" height="435" alt="image" src="https://github.com/user-attachments/assets/2c939cc4-99bc-460e-a278-dd2fc7a778e6" />
  <Figure size 640x480 with 1 Axes><img width="45%" height="455" alt="image" src="https://github.com/user-attachments/assets/2611388d-78a2-42dd-a42e-4c7bba0d87e5" />
</p>

### Overview of Categorical Columns

<Figure size 1600x600 with 3 Axes><img width="1573" height="569" alt="image" src="https://github.com/user-attachments/assets/653846cc-2f24-4ba0-b20e-18ccc2545a59" />

### Correlation Analysis

Based on the correlation matrix, these features seem to be highly/moderately positively correlated with `log_charges`
- `age` (0.5269)
- `bmi` (0.1382)
- `children` (0.1603)
- `smoker_yes` (0.6657)

It may also be worthwhile to include `sex_male` as a variable to explore whether gender has any influence on insurance charges as it raises potential concerns related to fairness in insurance charges.

<Figure size 1000x600 with 2 Axes><img width="876" height="636" alt="image" src="https://github.com/user-attachments/assets/4e94cdee-c1cb-41df-8748-34eb1c7800e9" />

## Results of Models

We decided to use Linear Regression, Random Forest, and XGBoost to model insurance charges, allowing us to compare performance between a simple linear approach and more complex methods.

In the end, **XGBoost** performed the best with the following scores (after hyperparameter tuning):
- R-squared: 0.8822
- RMSE: 0.3095
- MAE: 0.1778

<Figure size 800x500 with 1 Axes><img width="790" height="490" alt="image" src="https://github.com/user-attachments/assets/39112632-fb96-4fb2-8c23-0ec5fed1d717" />

<Figure size 640x480 with 1 Axes><img width="660" height="470" alt="image" src="https://github.com/user-attachments/assets/db5ef320-cebb-4320-9819-a59695b5c7a8" />

### Feature Importance

Both the Random Forest and XGBoost models indicate that `smoker_yes` and `age` are clearly important features for predicting insurance charges. Additionally, `children` and to a lesser extent, `bmi` appear to contribute meaningfully to the predictions as well.

<Figure size 1000x600 with 1 Axes><img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/fcc175a1-2715-4859-8d19-71255baf642a" />

## Fairness Analysis

### Visualisation Check

Based on the scatterplots, the distribution of insurance charges across genders and regions appears fairly random, with no clear pattern indicating that a particular gender or region consistently incurs higher or lower charges. Therefore, we cannot conclude that insurance costs differ systematically by gender or region from these plots alone.

<Figure size 1600x800 with 2 Axes><img width="1589" height="790" alt="image" src="https://github.com/user-attachments/assets/a4e2a1b9-d253-44b3-adfc-24b6eec38290" />

### Analysis of Variance (ANOVA) Check

Using ANOVA check on `sex` and `region`, we obtained evidence at the 5% level of significance that:
- mean insurance charges are different between males and females
- mean insurance charges are different for at least region

However, these differences may be influenced by other factors. For example, it is possible that **certain genders have higher smoking rates** or **some regions have an older population**, which could contribute to the observed disparities. Further investigation is needed to determine whether these factors, rather than sex or region alone, are driving the differences in charges.
