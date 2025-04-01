---
title: "Customer Category Predictor"
excerpt: "Customer classification and predictive analytics. <br/><img src='/images/500x300.png'>"
collection: portfolio
---

Link to the [GitHub repository](https://github.com/HenamSingla/CustomerCategoryPredictor)

# Customer Category Predictor - Process Overview

Below is an overview of the process used in this project:

- **Data Preparation**  
  - Imported the dataset (`train.csv`) and examined its structure.  
  - Handled missing values by creating dedicated dummy variables to indicate whether a cell was NaN or not.  
  - Converted categorical variables into multiple dummy features to capture all distinct categories.

- **Feature Engineering**  
  - Retained only relevant numerical and dummy variables for modeling.  
  - Removed unnecessary identifiers (like `customer_id`) and original categorical columns.

- **Train/Test Split**  
  - Separated the dataset into training (80%) and testing (20%) subsets.  
  - Ensured a fixed random state for reproducibility.

- **Modeling**  
  - **Logistic Regression**  
    - Served as a baseline model to quickly assess predictive performance.  
    - Provided straightforward interpretability of coefficients.
  - **Support Vector Machine (SVM)**  
    - Conducted hyperparameter tuning with GridSearchCV to identify optimal `C` and `gamma` values.  
    - Evaluated multiple parameter combinations for improved performance.

- **Performance Evaluation**  
  - Computed confusion matrices to examine true positives, true negatives, false positives, and false negatives.  
  - Calculated accuracy, recall, precision, and F1 scores to provide a comprehensive view of each model’s effectiveness.

- **ROC Curve Analysis**  
  - Plotted ROC curves for both Logistic Regression and SVM on the same graph.  
  - Measured the Area Under the Curve (AUC) to summarize model performance at various classification thresholds.  
  - A higher AUC indicates that the model is generally better at distinguishing between the classes across different thresholds.  
  - The diagonal line (from (0,0) to (1,1)) in the ROC plot represents a random guess baseline. The closer a model’s ROC curve is to the top-left corner, the more effective it is at discriminating between positive and negative classes.
