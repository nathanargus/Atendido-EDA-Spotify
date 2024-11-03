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

---

### Overview of Dataset

- How many rows and columns does the dataset contain?
- What are the data types of each column? Are there any missing values?
