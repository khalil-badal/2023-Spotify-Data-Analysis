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

# Display the first last five rows of the dataframe
df.tail()

```
Overview of Dataset

- How many rows and columns does the dataset contain?

```python
# Use pandas to identify the number of rows and the number of columns
# Identify the number of rows
print ("Rows:", df.shape[0])
print ("Columns:", df.shape[1])

