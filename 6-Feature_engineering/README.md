# Feature Engineering

## What is Feature Engineering?

Feature Engineering is the process of **creating, transforming, or extracting useful features from existing data** to make the data more useful for a Machine Learning model.

## Why is Feature Engineering Important?

- Improves the quality of features.
- Helps the model find useful patterns.
- Can improve model performance.
- Converts raw data into a useful format.
- Can reduce the complexity of the data.

## Main Techniques

### 1. Creating New Features

Create a new feature from existing features.

Example:

Age = Current Year - Birth Year

### 2. Combining Features

Combine multiple features to create a new feature.

Example:

Height + Weight → BMI

### 3. Transforming Features

Change the representation of an existing feature.

Examples:

- Log transformation
- Square root transformation
- Polynomial features

### 4. Extracting Features

Extract useful information from existing data.

Example:

Date → Day, Month, Year

### 5. Binning

Convert continuous numerical values into groups or categories.

Example:

Age → Young, Adult, Senior

## Example

Suppose we have:

| Birth Year | Current Year | Height | Weight |
|---:|---:|---:|---:|
| 2000 | 2026 | 1.75 | 70 |
| 2005 | 2026 | 1.65 | 60 |

We can create:

- Age
- BMI

The new features can provide more useful information to the Machine Learning model.

## Feature Engineering vs Feature Selection

**Feature Engineering:**  
Creates or transforms features.

**Feature Selection:**  
Selects the most useful features and removes unnecessary ones.

## Conclusion

Feature Engineering converts existing or raw data into **more useful features** that can help a Machine Learning model learn better.