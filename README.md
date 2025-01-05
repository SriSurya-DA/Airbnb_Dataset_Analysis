# Airbnb_Dataset_Analysis

![airbnb](https://github.com/SriSurya-DA/Airbnb_Dataset_Analysis/blob/main/Airbnb_logo.avif)

## Overview
This project involves analyzing an Airbnb dataset to explore various data patterns, perform data cleaning, and extract meaningful insights. The following steps were undertaken:

1. **Data Exploration**
2. **Data Cleaning**
3. **Exploratory Data Analysis (EDA)**
4. **Feature Engineering**
5. **Visualizations**

---

## Steps and Insights

### 1. Data Exploration

#### Loading the Dataset
```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.read_csv('datasets.csv')
```

- **Initial Inspection**:
  - `df.head()`: Displays the first few rows of the dataset.
  - `df.shape`: Provides the dimensions of the dataset.
  - `df.info()`: Summarizes the dataset, including data types and non-null counts.

#### Key Observations:
- The dataset contains missing values and duplicate entries.
- The `id` column is stored as a float but should be categorical.

---

### 2. Data Cleaning

#### Handling Missing Values
```python
df.isnull().sum()
df.dropna(inplace=True)
df.isnull().sum()
```
- Missing values were identified and dropped to ensure clean data.

#### Handling Duplicate Values
```python
df.duplicated().sum()
df.drop_duplicates(inplace=True)
df.duplicated().sum()
```
- Duplicate entries were removed.

#### Adjusting Data Types
```python
df['id'] = df['id'].astype('object')
```
- The `id` column was converted to an object data type.

#### Updated Dataset:
- Shape: `df.shape`

---

### 3. Exploratory Data Analysis (EDA)

#### Univariate Analysis

1. **Price Distribution**
```python
sns.histplot(data=df, x='price')
```
- Identified extreme outliers in the `price` column using a boxplot.
```python
sns.boxplot(data=df, x='price')
```
- Excluded prices > 16,000 for focused analysis.
```python
data = df[df['price'] < 16000]
sns.histplot(data=data, x='price', bins=120)
```

2. **Availability (365 Days)**
```python
sns.histplot(data=df, x='availability_365', bins=30)
```

![Availability](https://github.com/SriSurya-DA/Airbnb_Dataset_Analysis/blob/main/Availability_365.png)

#### Group Analysis
1. **Price by Neighborhood Group**
```python
data.groupby('neighbourhood_group')['price'].mean()
```
- Average price varies significantly across neighborhood groups.

2. **Price Per Bed**
```python
data['price_per_bed'] = data['price'] / data['beds']
data.groupby('neighbourhood_group')['price_per_bed'].mean()
```
- Introduced a new feature to normalize pricing.

#### Bivariate Analysis

1. **Neighborhood Group vs. Price by Room Type**
```python
sns.barplot(data=data, x='neighbourhood_group', y='price', hue='room_type')
```

2. **Reviews vs. Price**
```python
sns.scatterplot(data=data, x='number_of_reviews', y='price', hue='neighbourhood_group')
```

![locality Vs Price](https://github.com/SriSurya-DA/Airbnb_Dataset_Analysis/blob/main/locality%20Vs%20Review.png)
- Explored the relationship between the number of reviews and price.

#### Multivariate Analysis

```python
sns.pairplot(data=data, vars=['price', 'number_of_reviews', 'availability_365', 'beds'], hue='room_type')
```
![pairplot](https://github.com/SriSurya-DA/Airbnb_Dataset_Analysis/blob/main/pairplot.png)

- Examined relationships among numerical columns.

#### Geographical Distribution
```python
sns.scatterplot(data=data, x='longitude', y='latitude', hue='room_type')
```
- Visualized the spatial distribution of Airbnb listings.

---

## Visualizations
- **Histograms**: Displayed distributions of `price` and `availability_365`.
- **Boxplots**: Highlighted outliers in pricing.
- **Scatterplots**: Explored geographic and review-based trends.
- **Pairplot**: Showed interrelationships between numeric variables.

---

## Conclusions
- **Outliers**: High-priced listings (>16,000) were removed for a more focused analysis.
- **Neighborhood Insights**:
  - Pricing and price per bed vary significantly across neighborhoods.
  - Room type is a strong determinant of price within neighborhoods.
- **Geographical Trends**:
  - Listings are clustered in specific geographic locations.

This analysis highlights key trends and relationships within the Airbnb dataset and sets the foundation for further modeling or operational insights.

