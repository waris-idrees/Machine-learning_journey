# Train-Test Split

## What is Train-Test Split?

Train-Test Split is the process of **dividing a dataset into separate parts for training and testing a Machine Learning model**.

## Why Do We Split the Data?

We split the data because we need to:

- Train the model using training data.
- Test the model using unseen data.
- Measure how well the model performs on new data.
- Detect overfitting.

## Main Parts

### 1. Training Data

Training data is used to **teach the Machine Learning model** and learn patterns from the data.

### 2. Testing Data

Testing data is used to **evaluate the trained model** on data that it has not seen during training.

## Example

Suppose we have 1,000 records:

| Data | Percentage | Records | Purpose |
|---|---:|---:|---|
| Training Data | 80% | 800 | Train the model |
| Testing Data | 20% | 200 | Test the model |

## Common Splits

- 80% Training / 20% Testing
- 70% Training / 30% Testing
- 90% Training / 10% Testing

The choice depends on the dataset size and the Machine Learning problem.

## Important Point

The testing data should **not be used during model training**.

The model should see the testing data only when we evaluate its performance.

## Python Example

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)