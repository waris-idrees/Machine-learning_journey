# Feature Selection

## What is Feature Selection?

Feature Selection is the process of **selecting the most useful features from a dataset and removing unnecessary or irrelevant features**.

## Why is Feature Selection Important?

- Reduces unnecessary features.
- Reduces model complexity.
- Can improve model performance.
- Reduces training time.
- Can help reduce overfitting.
- Makes the model easier to understand.

## Main Methods

### 1. Correlation
Checks the relationship between numerical features and the target.

### 2. Filter Methods
Select features using statistical measures or scores.

### 3. Wrapper Methods
Tests different combinations of features using a Machine Learning model.

### 4. Embedded Methods
Feature selection happens during model training.

Examples:
- Lasso (L1 Regularization)
- Decision Tree
- Random Forest

### 5. RFE
Recursive Feature Elimination repeatedly removes less important features until the desired number of features remains.

## Example

Suppose we want to predict house prices:

| Feature | Importance |
|---|---|
| House Size | Useful |
| Location | Useful |
| Number of Rooms | Useful |
| Wall Color | Not useful |
| House ID | Not useful |

Selected features:

- House Size
- Location
- Number of Rooms

Removed features:

- Wall Color
- House ID

## Feature Selection vs Feature Engineering

**Feature Selection:**  
Selects useful features from existing features.

**Feature Engineering:**  
Creates or transforms features to make them more useful.

## Conclusion

Feature Selection helps us **keep the most relevant features and remove unnecessary ones before training the Machine Learning model.**