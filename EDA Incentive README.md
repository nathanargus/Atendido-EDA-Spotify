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

To ensure data integrity and prevent errors during analysis, missing values were filled with zeros. This method allows for consistent calculations and visualizations without omitting any entries, maintaining the usability of the dataset. The function can be seen below.

```python

# Fill missing values with zero
df.fillna(0, inplace=True)

```

#### Key Insights:

- __Dataset Structure__: The dataset contains __953 rows__ and __24 columns__, indicating a substantial amount of data on popular songs.
- __Data Types__: Most columns are numeric (e.g., `int64` for counts and percentages) and categorical (e.g., `object` for strings), which suggests diverse data suitable for various analyses.
- __Missing Values__: There are __50 missing entries__ in `in_shazam_charts` and __95__ in `key`. To maintain dataset integrity, these missing values were filled with zeros, ensuring consistent calculations and enabling comprehensive analysis.

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
print("Mean:", "{:,.0f}".format(mean_streams))
print("Median:", "{:,.0f}".format(median_streams))
print("Standard Deviation:", "{:,.0f}".format(std_streams))

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

# Boxplot for Released Year
plt.figure(figsize=(15, 6))
sns.boxplot(x=df['released_year'], color='#EAB8E4')
plt.title('Boxplot of Released Years')
plt.xlabel('Released Year')
plt.grid(axis='y')
plt.show()

# Boxplot for Artist Count
plt.figure(figsize=(15, 6))
sns.boxplot(x=df['artist_count'], color='#EAB8E4')
plt.title('Boxplot of Artist Count')
plt.xlabel('Number of Artists')
plt.grid(axis='y')
plt.show()

# Summary of Outliers
def summarize_outliers(df, column):
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    
    # Detect outliers
    outliers = df[(df[column] < lower_bound) | (df[column] > upper_bound)][column]
    num_outliers = outliers.count()
    outlier_range = (outliers.min(), outliers.max())
    
    return num_outliers, outlier_range

# Apply to 'released_year' and 'artist_count'
released_year_outliers = summarize_outliers(df, 'released_year')
artist_count_outliers = summarize_outliers(df, 'artist_count')

# Display summary
print("Released Year:", released_year_outliers[0], "outliers,", "Range:", released_year_outliers[1])
print("Artist Count:", artist_count_outliers[0], "outliers,", "Range:", artist_count_outliers[1])

```
Mean: 513,597,931<br>
Median: 290,228,626<br>
Standard Deviation: 566,803,887

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/dd5efde9-75fe-4ff7-9018-3f92b12957a2" width="300" height="200" style="width: 49%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/d1581191-0118-45df-bd87-caf41d52485d" width="300" height="200" style="width: 49%; margin-right: 5%;">
</div>

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/9257420a-32fc-4b67-8661-47090f4626b2" width="300" height="200" style="width: 49%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/ef78941a-e00a-4a54-b8e6-e45d030508e5" width="300" height="200" style="width: 49%; margin-right: 5%;">
</div>

Released Year: 151 outliers, Range: (1930, 2016)<br>
Artist Count: 27 outliers, Range: (4, 8)

#### Key Insights:

- The streams column analysis reveals a mean of __513,597,931__, a median of __290,228,626__, and a standard deviation of __566,803,887__.
- __Artist Count Distribution__: The majority of songs in the dataset are created by a single artist, with fewer songs featuring collaborations. There’s a noticeable drop in song counts as the number of contributing artists increases, indicating that solo performances are the dominant trend in this dataset. Collaborative projects involving three or more artists are relatively rare, which might reflect the music industry’s focus on solo or duo releases.
- __Released Year Distribution__: 2022 stands out as the year with the highest number of song releases, followed closely by 2023 and 2021. This surge in releases over the past few years may be attributed to the growing popularity of digital platforms, which make it easier for artists to produce and distribute their music. The drop-off in releases for earlier years suggests that the dataset is skewed towards more recent releases, capturing a snapshot of the recent growth in music production and distribution.
- __Outliers__: For released_year, there are __151 outliers__ with a range from __1930 to 2016__, while artist_count shows __27 outliers__ ranging from __4 to 8__. The visualizations depict trends in released years and artist contributions, highlighting patterns and potential anomalies in the dataset.

---

### Top Performers

In this section, we analyze the performance of tracks within the dataset to identify standout songs and artists. By examining the total number of streams, we can pinpoint the track with the highest popularity. Additionally, we will investigate which artists frequently appear in the dataset, shedding light on the most prolific contributors to this collection of popular songs. This analysis will provide valuable insights into trends in music consumption and artist visibility. You can see the function and its output below.

- Which track has the highest number of streams? Display the top 5 most streamed tracks.
- Who are the top 5 most frequent artists based on the number of tracks in the dataset?

```python

# Sort and display the top 5 tracks by streams
top_tracks = df[['track_name', 'artist(s)_name', 'streams']].sort_values(by='streams', ascending=False).head()

# Combine track names and artist names for clearer labels
top_tracks['label'] = top_tracks['track_name'] + ' - ' + top_tracks['artist(s)_name']

# Plot the bar graph
plt.figure(figsize=(10, 6))
bars = plt.barh(top_tracks['label'], top_tracks['streams'], color='#EAB8E4')
plt.xlabel('Streams')
plt.title('Top 5 Most Streamed Tracks')
plt.gca().invert_yaxis()  # Highest streams at the top

# Add stream counts inside each bar
for bar, stream_count in zip(bars, top_tracks['streams']):
    plt.text(bar.get_width() - (bar.get_width() * 0.05),  # Position slightly left of bar end
             bar.get_y() + bar.get_height() / 2,
             f'{int(stream_count):,}', va='center', ha='right', color='black')

plt.tight_layout()
plt.show()

# Split the 'artist(s)_name' column, then "explode" to separate each artist into its own row
df['artist(s)_name'] = df['artist(s)_name'].str.split(',')
exploded_artists = df.explode('artist(s)_name')

# Remove any leading/trailing spaces and count occurrences of each artist
exploded_artists['artist(s)_name'] = exploded_artists['artist(s)_name'].str.strip()
top_artists = exploded_artists['artist(s)_name'].value_counts().head(5)

# Plot the bar graph for the top 5 most frequent artists
plt.figure(figsize=(10, 6))
artist_bars = plt.barh(top_artists.index, top_artists.values, color='#EAB8E4')
plt.xlabel('Number of Tracks')
plt.title('Top 5 Most Frequent Artists by Track Count')
plt.gca().invert_yaxis()

# Add track counts inside each bar
for bar, track_count in zip(artist_bars, top_artists.values):
    plt.text(bar.get_width() - (bar.get_width() * 0.05),  # Position near the end of each bar
             bar.get_y() + bar.get_height() / 2,
             f'{track_count}', va='center', ha='right', color='black')

plt.tight_layout()
plt.show()

```

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/88bc135e-f015-4234-a1f0-8d88bd64fb68" width="300" height="250" style="width: 49%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/97645707-16f8-4b06-b6e9-a3c2a5fe6cf4" width="300" height="250" style="width: 49%; margin-right: 5%;">
</div>

#### Key Insights:

- __Top Streamed Tracks__: The track with the highest streams is "_Blinding Lights_" by __The Weeknd__, followed by "_Shape of You_" by __Ed Sheeran__ and "_Someone You Loved_" by __Lewis Capaldi__. These top five tracks each surpass two billion streams, highlighting their immense popularity.
- __Top Artists by Track Count__: __Bad Bunny__ leads as the most frequent artist in the dataset, appearing on __40 tracks__. __Taylor Swift__ and __The Weeknd__ follow with __38 and 37 tracks__ respectively, demonstrating their consistency in releasing popular music. __Kendrick Lamar__ and __SZA__ each have __23 tracks__, further highlighting their significant presence in the music landscape.

---

### Temporal Trends

This section focuses on analyzing temporal trends in the dataset, specifically examining the annual and monthly release patterns of tracks. By plotting the number of tracks released over time, we aim to identify any significant trends or fluctuations in the music landscape. Additionally, we will explore whether certain months see a higher volume of releases, helping to uncover patterns that may inform us about industry dynamics and seasonal variations in music production. You can see the function and output below.

- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

```python

# Load the dataset
# df = pd.read_csv('your_dataset.csv') # Uncomment and adjust this line to load your dataset

# Ensure the 'released_year' and 'released_month' columns are of the correct data type
df['released_year'] = pd.to_numeric(df['released_year'], errors='coerce')
df['released_month'] = pd.to_numeric(df['released_month'], errors='coerce')

# Count the number of tracks released per year
tracks_per_year = df['released_year'].value_counts().sort_index()

# Plot the number of tracks released per year
plt.figure(figsize=(12, 6))
sns.lineplot(x=tracks_per_year.index, y=tracks_per_year.values, marker='o', color='#d46ec8')
plt.title('Number of Tracks Released per Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Count the number of tracks released per month
tracks_per_month = df['released_month'].value_counts().sort_index()

# Plot the number of tracks released per month
plt.figure(figsize=(12, 6))
sns.lineplot(x=tracks_per_month.index, y=tracks_per_month.values, marker='o', color='#d46ec8')
plt.title('Number of Tracks Released per Month')
plt.xlabel('Month')
plt.ylabel('Number of Tracks')
plt.xticks(ticks=range(1, 13), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.tight_layout()
plt.show()

```

<div style="display: flex;">
  <img src="https://github.com/user-attachments/assets/78772803-5e3b-401f-ba68-7a7c32e8ea89" width="300" height="250" style="width: 49%; margin-right: 5%;">
  <img src="https://github.com/user-attachments/assets/39c66b8f-9d17-4205-999e-8d46d47df11f" width="300" height="250" style="width: 49%; margin-right: 5%;">
</div>

#### Key Insights:

- __Track Release Trends Over Time__: The dataset shows a significant increase in track releases in recent years, particularly post-2010, with a sharp peak around 2020. This suggests either a surge in music production or an increase in the dataset's capture rate of tracks over time, possibly due to digital platforms.
- __Seasonal Variation in Monthly Releases__: Track releases vary month-to-month, with peaks in January and May, indicating potential strategic release timings (e.g., start of the year or mid-year). The lowest activity is in August, possibly reflecting a seasonal dip in music production or marketing.

---

### Genre and Music Characteristics

In this section, we will explore the relationships between the number of streams and various musical attributes, such as beats per minute (bpm), danceability percentage, and energy percentage. We will assess which of these attributes most significantly influences streaming numbers. Additionally, we will investigate the correlation between danceability and energy, as well as between valence and acousticness, to understand how these musical characteristics interact with each other. You can see the function and output below.

- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

```python

# Calculate the correlation matrix
correlation_matrix = df[['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%', 'instrumentalness_%', 
                  'liveness_%','speechiness_%']].corr()

# Create a heatmap
plt.figure(figsize=(15, 6))
sns.heatmap(correlation_matrix, annot=True, fmt='.3f', cmap='PiYG')
plt.title('Correlation Heatmap of Streams and Musical Attributes')
plt.show()

```

![image](https://github.com/user-attachments/assets/66346211-8b04-4480-995b-9fd63bbbb294)

#### Key Insights:

- __Influence of Musical Attributes on Streams__: The correlation between _streams_ and musical attributes such as *bpm, danceability_%*, and *energy_%* is weak. Specifically, *streams* has correlations of **-0.002** with *bpm*, **-0.105** with *danceability_%*, and **-0.026** with *energy_%*, indicating that these attributes individually have minimal influence on streaming popularity. This suggests that other external factors may be more influential in driving streams.
- **Correlation Between Danceability_% and Energy_%**: There is a positive correlation of **0.198** between *danceability_%* and *energy_%*. Although this correlation is moderate, it suggests that tracks with higher danceability tend to have higher energy, making them suitable for high-energy environments like clubs or workout playlists.
- **Correlation Between Valence_% and Acousticness_%**: The correlation between *valence_%* (mood positivity) and *acousticness_%* is very weak at **-0.082**. This indicates that there is no strong relationship between the mood of a track and its acoustic nature, suggesting that positive or negative moods can appear equally in both acoustic and non-acoustic tracks.

---

### Platform Popularity

In this section, we will analyze the popularity of tracks across different music streaming platforms: Spotify, Deezer, and Apple Music. By comparing the number of tracks listed on these platforms, we can identify trends in platform usage and determine which platform tends to feature the most popular tracks. This analysis will provide insights into user preferences and the distribution of music across these major streaming services. You can see the function and output below.

- How do the numbers of tracks in spotify_playlists, deezer_playlist, and apple_playlists compare? Which platform seems to favor the most popular tracks?
