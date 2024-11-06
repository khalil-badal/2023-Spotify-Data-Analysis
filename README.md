## Python - Exploratory Data Analysis on Spotify 2023 

Welcome to my Exploratory Data Analysis in Python! This repository showcases Spotify’s 2023 metrics, uncovering trends like the most-streamed songs, top artists, genre preferences, and more. In this Data Analysis, we will be using different Python libraries such as Pandas, Matplotlib and Seaborn. 

### What You'll Find Here:
- Data Exploration: A detailed overview of the dataset, including its structure, variables, and key statistics.
- Musical Trends: Uncovering emerging trends in music genres, release dates, and popular themes.
- Top-Performing Artists: Analyzing the top-performing artists, their most popular tracks, and their musical styles.
- Platform Popularity: Comparing the popularity of tracks across different streaming platforms.
- Visualizations: Interactive visualizations to bring data to life and aid in understanding complex relationships.

### Exploratory Data Analysis Structure:

#### Overview of Dataset 
- How many rows and columns does the dataset contain?
- What are the data types of each column? Are there any missing values?
  
#### Basic Descriptive Statistics
- What are the mean, median, and standard deviation of the streams column?
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
  
#### Top Performers
- Which track has the highest number of streams? Display the top 5 most streamed tracks.
- Who are the top 5 most frequent artists based on the number of tracks in the dataset?
  
#### Temporal Trends
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
  
#### Genre and Music Characteristics
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?
  
#### Platform Popularity
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?
  
#### Advanced Analysis
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

## Python Codes

```python
# Start
# Import all the necessary libraries for the Exploratory Data Analysis 
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Read the CSV file, specifying the encoding to handle special characters
df = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')

# Display the first five rows of the dataframe
df.head()
```
![image](https://github.com/user-attachments/assets/ee80c16f-4cfd-4308-833c-b5dd3a6ec85e)

```python
# Display the first last five rows of the dataframe
df.tail()
```
![image](https://github.com/user-attachments/assets/2d5c4724-15c7-46d3-b240-0bf4953cb12c)


## Overview of Dataset

- How many rows and columns does the dataset contain?

```python
# Use pandas to identify the number of rows and the number of columns
# Identify the number of rows
print ("Rows:", df.shape[0])
print ("Columns:", df.shape[1])
```
![image](https://github.com/user-attachments/assets/f5d87cbd-d371-4a36-a1e5-bafd07026247)


- What are the data types of each column? Are there any missing values?
  
```python
# Identify the number of data types in each column
print(df.dtypes)
```
![image](https://github.com/user-attachments/assets/fc92f71d-b585-49d3-8693-0c6f9a0908c5)

# Check for missing values in each column

![image](https://github.com/user-attachments/assets/68585687-6e6f-42f0-a734-960479f85e45)

```python
print(df.isnull().sum())

# Use pandas to fill every song that has a missing key with "C"
df.iloc[:, 15] = df.iloc[:, 15].fillna("C")

```
In this spreadsheet, we can see that the key "C" is not found in any of the songs, and there are song that has a missing key 
This is an obvious pattern because C major is one of the most common key in songwriting
Use pandas to fill every song that has a missing key with "C"

## Basic Descriptive Statistics

```python
# Summary statistics for numerical columns
print(df.describe())
```
![image](https://github.com/user-attachments/assets/7dabe9b5-0778-44d4-8277-8376ae075342)



- What are the mean, median, and standard deviation of the streams column?

```python
# Convert the 'streams' column to numeric, setting errors='coerce' to turn non-numeric values to NaN
df.loc[:, 'streams'] = pd.to_numeric(df['streams'], errors='coerce')

# Calculate the statistics and round to 2 decimal places
# Calculate the mean
mean_streams = round(df['streams'].mean(), 2)
# Calculate the median
median_streams = round(df['streams'].median(), 2)
# Calculate the mode
std_streams = round(df['streams'].std(), 2)

# Display the output
print ("Mean of the number of streams:", mean_streams)
print ("Median of the number of streams:", median_streams)
print ("Standard Deviation of the number of streams:", std_streams)

```
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

```python
# Using seaborn, set up a figure with a 2x2 grid of subplots, specifying the figure size for better visualization
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# Adding a title for the entire figure
fig.suptitle('Distribution and Trends of Released Year and Artist Count')

# Plotting a histogram for 'released_year' with 20 bins, enabling kernel density estimation (KDE), and setting color
sns.histplot(df['released_year'], bins=20, kde=True, ax=axes[0, 0], color='purple')
# Setting the title, x-axis, and y-axis labels for the released_year histogram
axes[0, 0].set_title('Distribution of Released Year')
axes[0, 0].set_xlabel('Released Year')
axes[0, 0].set_ylabel('Frequency')

# Plotting a histogram for 'artist_count' with 20 bins, enabling KDE, and setting a different color
sns.histplot(df['artist_count'], bins=20, kde=True, ax=axes[0, 1], color='yellow')
# Setting the title, x-axis, and y-axis labels for the artist_count histogram
axes[0, 1].set_title('Distribution of Artist Count')
axes[0, 1].set_xlabel('Artist Count')
axes[0, 1].set_ylabel('Frequency')

# Creating a boxplot for 'released_year' to show data distribution, trends, and outliers
sns.boxplot(x=df['released_year'], ax=axes[1, 0], color='purple')
# Setting the title and x-axis label for the released_year boxplot
axes[1, 0].set_title('Boxplot of Released Year')
axes[1, 0].set_xlabel('Released Year')

# Creating a boxplot for 'artist_count' to show data distribution, trends, and outliers
sns.boxplot(x=df['artist_count'], ax=axes[1, 1], color='yellow')
# Setting the title and x-axis label for the artist_count boxplot
axes[1, 1].set_title('Boxplot of Artist Count')
axes[1, 1].set_xlabel('Artist Count')

# Adjusting layout to reduce overlap and improve appearance; adding space for the main title
plt.tight_layout(rect=[0, 0.03, 1, 0.95])

# Displaying the plots
plt.show()

 ```
## Top Performers

- Which track has the highest number of streams? Display the top 5 most streamed tracks.


