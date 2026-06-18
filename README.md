# Mall Customers Covariance Analysis

This project analyzes a mall customer dataset to explore relationships between customer attributes such as gender, age, annual income, and spending score.

The main focus is on covariance analysis, supported by visual exploration and correlation analysis for easier interpretation.

## Project Files

| File | Description |
| --- | --- |
| `Mall_Customers.csv` | Customer dataset used for the analysis |
| `mall_customers_covariance_assignment.ipynb` | Jupyter Notebook containing the full analysis workflow |

## Dataset Columns

| Column | Description |
| --- | --- |
| `CustomerID` | Unique customer identifier |
| `Gender` | Customer gender |
| `Age` | Customer age |
| `Annual Income (k$)` | Annual income in thousands of dollars |
| `Spending Score (1-100)` | Customer spending score from 1 to 100 |

## Analysis Goals

The notebook explores questions such as:

- How is customer age related to spending score?
- Does higher annual income always imply a higher spending score?
- Is there a visible relationship between gender and other customer attributes?
- Which feature pairs have positive or negative covariance?
- How does covariance compare with correlation for interpretation?

## Workflow Overview

### 1. Import Libraries

The notebook uses:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

Library roles:

- `pandas`: loading and manipulating tabular data
- `numpy`: numerical operations
- `matplotlib`: basic plotting
- `seaborn`: statistical visualizations such as scatter plots and heatmaps

### 2. Load the Dataset

The dataset is loaded from the local CSV file:

```python
df = pd.read_csv("Mall_Customers.csv")
df.head()
```

This confirms that the file is readable and shows the first rows of the dataset.

### 3. Inspect the Data

The notebook checks the dataset structure:

```python
df.shape
df.columns
df.info()
df.isna().sum()
df.duplicated().sum()
```

This step identifies:

- number of rows and columns
- column names
- data types
- missing values
- duplicate rows

### 4. Rename Columns

Long column names are renamed for easier use:

```python
df = df.rename(columns={
    "Annual Income (k$)": "Income",
    "Spending Score (1-100)": "SpendingScore"
})
```

After this step, `Income` and `SpendingScore` are used throughout the notebook.

### 5. Descriptive Statistics

The notebook uses `describe()` to summarize numerical columns:

```python
df.describe()
```

This gives count, mean, standard deviation, minimum, maximum, and quartile values.

### 6. Gender Distribution

The gender distribution is checked with:

```python
df["Gender"].value_counts()
```

A count plot is also used to visualize the number of customers in each gender group.

### 7. Numeric Feature Distributions

Histograms are created for:

- age
- annual income
- spending score

These charts help show how the values are distributed across the dataset.

### 8. Grouped Averages by Gender

The notebook compares average age, income, and spending score by gender:

```python
df.groupby("Gender")[["Age", "Income", "SpendingScore"]].mean()
```

This provides a simple group-level summary before moving into covariance analysis.

### 9. Encode Gender

Because covariance requires numeric values, the `Gender` column is encoded:

```python
df["Gender_ID"] = df["Gender"].map({"Female": 0, "Male": 1})
```

This encoding is only used to make the categorical feature usable in numerical analysis. It does not imply any ranking between gender values.

### 10. Relationship Visualizations

Scatter plots are used to visually inspect relationships between:

- income and spending score
- age and spending score
- age and income

Example:

```python
sns.scatterplot(data=df, x="Income", y="SpendingScore", hue="Gender")
```

These plots provide an initial visual understanding before calculating covariance.

### 11. Covariance Matrix

The central part of the project is the covariance matrix:

```python
analysis_df = df[["Gender_ID", "Age", "Income", "SpendingScore"]]
cov_matrix = analysis_df.cov()
cov_matrix
```

Covariance interpretation:

- Positive covariance means two variables tend to move in the same direction.
- Negative covariance means one variable tends to increase while the other decreases.
- A value near zero suggests no clear linear movement between the two variables.

Important note: covariance depends on the scale of the variables, so its magnitude is not always easy to compare across different feature pairs.

### 12. Covariance Heatmap

The covariance matrix is visualized with a heatmap:

```python
sns.heatmap(cov_matrix, annot=True, fmt=".2f", cmap="Blues")
```

The heatmap makes positive and negative relationships easier to scan.

### 13. Pairwise Covariance Table

The notebook also builds a pairwise covariance table to make feature-by-feature interpretation clearer.

This helps identify:

- covariance between age and spending score
- covariance between income and spending score
- whether each relationship is positive or negative

### 14. Correlation Matrix

Correlation is calculated as a standardized companion to covariance:

```python
corr_matrix = analysis_df.corr()
```

Unlike covariance, correlation always ranges from `-1` to `1`, making it easier to compare relationship strength across feature pairs.

## Key Takeaways

- Covariance helps identify the direction of movement between variables.
- Correlation is easier to interpret when comparing relationship strength.
- Visualizations such as scatter plots and heatmaps make numerical relationships easier to understand.
- Encoding categorical values is necessary before using them in numerical covariance calculations.

## How to Run

1. Open `mall_customers_covariance_assignment.ipynb` in Jupyter Notebook, JupyterLab, or VS Code.
2. Make sure `Mall_Customers.csv` is in the same directory as the notebook.
3. Run the notebook cells from top to bottom.
4. Review the printed outputs, tables, and charts.

## Requirements

Install the required libraries with:

```bash
pip install numpy pandas matplotlib seaborn
```

## Summary

This project provides a step-by-step covariance-based analysis of mall customer attributes. It combines data inspection, visualization, covariance, and correlation to understand how customer features relate to each other.
