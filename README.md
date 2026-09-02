**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.

## Program

Name : SANJAY R

## Reg No:212225040372
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler, StandardScaler

# Load dataset
df = pd.read_csv("C:/Users/acer/Downloads/del-sirifort-cpcb-2024-25.csv")

# Data Understanding
print("First 5 Records:")
print(df.head())

print("\nDataset Shape:")
print(df.shape)

print("\nMissing Values:")
print(df.isnull().sum())


# Selected Air Quality Variables
cols = [
    "PM2.5 (µg/m³)",
    "PM10 (µg/m³)",
    "NO2 (µg/m³)",
    "Ozone (µg/m³)"
]

# Select data and remove missing values
data = df[cols].dropna()


# 1. Central Tendency
print("\n--- CENTRAL TENDENCY ---")

print("\nMean:")
print(data.mean())

print("\nMedian:")
print(data.median())

print("\nMode:")
print(data.mode().iloc[0])


# 2. Measures of Spread
print("\n--- MEASURES OF SPREAD ---")

print("\nMinimum:")
print(data.min())

print("\nMaximum:")
print(data.max())

print("\nRange:")
print(data.max() - data.min())

print("\nVariance:")
print(data.var())

print("\nStandard Deviation:")
print(data.std())


# 3. Distribution using Histogram
for col in cols:
    plt.figure(figsize=(6, 4))
    sns.histplot(data[col], bins=30, kde=True)
    plt.title(f"Distribution of {col}")
    plt.show()


# 4. Outlier Detection using Boxplot
for col in cols:
    plt.figure(figsize=(6, 3))
    sns.boxplot(x=data[col])
    plt.title(f"Boxplot of {col}")
    plt.show()


# 5. Min-Max Scaling
scaler = MinMaxScaler()

scaled_data = pd.DataFrame(
    scaler.fit_transform(data),
    columns=cols
)

print("\n--- MIN-MAX SCALED DATA ---")
print(scaled_data.head())


# 6. Standardization
standard_scaler = StandardScaler()

standardized_data = pd.DataFrame(
    standard_scaler.fit_transform(data),
    columns=cols
)

print("\n--- STANDARDIZED DATA ---")
print(standardized_data.head())


# 7. Outlier Detection using IQR
print("\n--- OUTLIER COUNT ---")

for col in cols:
    Q1 = data[col].quantile(0.25)
    Q3 = data[col].quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = data[
        (data[col] < lower) |
        (data[col] > upper)
    ]

    print(col, "Outliers =", len(outliers))


# 8. Gini Coefficient for Inequality
def gini(x):
    x = np.array(x)
    x = np.sort(x)

    n = len(x)
    index = np.arange(1, n + 1)

    return np.sum((2 * index - n - 1) * x) / (n * np.sum(x))


print("\n--- GINI COEFFICIENT ---")

for col in cols:
    print(col, "=", gini(data[col]))
```


## Output
<img width="922" height="323" alt="Screenshot 2026-09-01 134104" src="https://github.com/user-attachments/assets/7e35c850-2ec0-4298-b7b7-18081bdb3d46" />
<img width="671" height="318" alt="Screenshot 2026-09-01 134115" src="https://github.com/user-attachments/assets/cfa023a9-5237-46af-840a-46ecd5befb3b" />
<img width="453" height="335" alt="Screenshot 2026-09-01 134122" src="https://github.com/user-attachments/assets/ad178e05-a67c-42a0-b50a-20817b1c47c6" />
<img width="625" height="311" alt="Screenshot 2026-09-01 134144" src="https://github.com/user-attachments/assets/7f0324c0-7005-4bb1-b86b-91f457bda42d" />
<img width="612" height="302" alt="Screenshot 2026-09-01 134154" src="https://github.com/user-attachments/assets/679a4803-c451-4a19-bd39-c5ae28016acb" />
<img width="5" height="1" alt="Screenshot 2026-09-01 134209" src="https://github.com/user-attachments/assets/10dc6d34-9a44-4efa-9257-83140ed09501" />
<img width="762" height="327" alt="Screenshot 2026-09-01 134227" src="https://github.com/user-attachments/assets/825e38ef-69d7-4734-a589-a9d500699bcf" />

<img width="622" height="303" alt="Screenshot 2026-09-01 134239" src="https://github.com/user-attachments/assets/c5c24c6d-c2a8-4462-abc6-c1c97dcfcd60" />

## Interpretation

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) write other insights

## Result

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.
