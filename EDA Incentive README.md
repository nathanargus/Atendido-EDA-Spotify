# 🎸 Exploratory Data Analysis: Most Streamed Spotify Songs 2023 🎸

This project presents an exploratory data analysis (EDA) of __Spotify's Most Streamed Songs of 2023__ dataset. It covers _data cleaning, descriptive statistics, visualizations,_ and _correlation analyses_ to identify patterns and insights on track popularity. The findings highlight trends in _musical attributes_ (e.g., BPM, danceability, and energy), _artist frequency,_ and _platform popularity._

Through this analysis, the project aims to provide a deeper understanding of the factors contributing to a song's popularity and key trends in popular music for 2023.

---

### Loading the Data

The [dataset](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023?resource=download), __Most Streamed Spotify Songs 2023__, comprises __953 tracks__ with detailed musical attributes like BPM, danceability, and popularity across platforms. Using `pandas`, we begin by loading and inspecting the `spotify-2023.csv` file, examining its structure, checking for missing values, and previewing a few rows. This setup prepares the data for a comprehensive analysis. You can see the function and its output below.

```python

import pandas as pd

# Load the dataset
df = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')

# Display the first and last 5 rows to get an overview
pd.concat([df.head(), df.tail()])

```

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/64b1ca5b-3d56-4815-a6f0-a1a7ddaa7ef0" style="width: 49%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/4c2b90b0-d03f-4f7d-afd0-ff48f6af5148" style="width: 49%; margin-right: 5%;">
</div>

---

### Overview of Dataset

In this section, we will address key questions regarding the dataset, including the number of rows and columns, the data types of each column, and any missing values present.

- How many rows and columns does the dataset contain?
- What are the data types of each column? Are there any missing values?

```python

# Display the number of rows and columns
rows, columns = df.shape
print(f"The dataset contains {rows} rows and {columns} columns.\n")

# Display the data types of each column with aligned formatting
data_types = df.dtypes

print("\nColumn Data Types:")
for column, dtype in data_types.items():
    print(f"{column:<30}: {dtype}")

# Check for any missing values in the dataset
missing_values = df.isnull().sum()
missing_values = missing_values[missing_values > 0]

print("\nMissing Values in Each Column:")
for column, count in missing_values.items():
    print(f"{column:<30}: {count}")

```

![image](https://github.com/user-attachments/assets/37ea0d48-4b23-4e2a-944a-6d9d47ec7d7b)

#### Key Insights:

- __Dataset Structure__: The dataset contains __953 rows__ and __24 columns__, indicating a substantial amount of data on popular songs.
- __Data Types__: Most columns are numeric (e.g., `int64` for counts and percentages) and categorical (e.g., `object` for strings), which suggests diverse data suitable for various analyses.
- __Missing Values__: There are __50 missing entries__ in `in_shazam_charts` and __95__ in `key`, indicating potential gaps in data that may affect analysis but can be addressed through imputation or exclusion in specific analyses.

---

### Basic Descriptive Statistics

This section aims to provide insights into the dataset by calculating key statistical metrics for the streams column, which will help us understand track popularity. Furthermore, we will explore the distributions of released_year and artist_count to uncover patterns and potential anomalies in the data. You can see the function and its output below.

- What are the mean, median, and standard deviation of the streams column?
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

```python

import matplotlib.pyplot as plt
import seaborn as sns

# Convert streams to numeric, forcing errors to NaN
df['streams'] = pd.to_numeric(df['streams'], errors='coerce')

# Calculate mean, median, and standard deviation for the streams column
mean_streams = df['streams'].mean()
median_streams = df['streams'].median()
std_streams = df['streams'].std()

# Display the results with comma formatting
print(f"Mean: {mean_streams:,.0f}")
print(f"Median: {median_streams:,.0f}")
print(f"Standard Deviation: {std_streams:,.0f}")

# Set the style of seaborn
sns.set(style="whitegrid")

# Released Year Distribution starting from 2023
plt.figure(figsize=(12, 6))
released_year_distribution = df['released_year'].value_counts().sort_index(ascending=True)  
sns.barplot(x=released_year_distribution.index, y=released_year_distribution.values, color='#EAB8E4')  
plt.title('Released Year Distribution')
plt.xlabel('Year')
plt.ylabel('Number of Songs')
plt.xticks(rotation=45)
plt.gca().invert_xaxis()  # Invert x-axis to show 2023 first
plt.tight_layout()
plt.show()

# Artist Count Distribution
plt.figure(figsize=(12, 6))
sns.countplot(x='artist_count', data=df, order=df['artist_count'].value_counts().index, color='#EAB8E4')  
plt.title('Artist Count Distribution')
plt.xlabel('Number of Artists')
plt.ylabel('Number of Songs')
plt.tight_layout()
plt.show()

```

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/b4ebf57b-4855-4c87-b4ff-0f7d6c7fe3ca" width="300" height="165" style="width: 33%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/dd5efde9-75fe-4ff7-9018-3f92b12957a2" style="width: 33%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/d1581191-0118-45df-bd87-caf41d52485d" style="width: 33%; margin-right: 5%;">
</div>

#### Key Insights:

- __Dataset Structure__: The dataset contains __953 rows__ and __24 columns__, indicating a substantial amount of data on popular songs.
- __Data Types__: Most columns are numeric (e.g., `int64` for counts and percentages) and categorical (e.g., `object` for strings), which suggests diverse data suitable for various analyses.
- __Missing Values__: There are __50 missing entries__ in `in_shazam_charts` and __95__ in `key`, indicating potential gaps in data that may affect analysis but can be addressed through imputation or exclusion in specific analyses.
