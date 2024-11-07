# 2023-Spotify-Exploratory-Data-Analysis
Welcome to my Exploratory Data Analysis in Python! This repository showcases Spotify’s 2023 metrics, uncovering trends like the most-streamed songs, top artists, genre preferences, and more. We will use a CSV file obtained from Kaggle to extract and analyze the data using Python libraries such as Pandas, Matplotlib, and Seaborn.

## What You'll Find Here:
- Data Exploration: A detailed overview of the dataset, including its structure, variables, and key statistics.
- Musical Trends: Uncovering emerging trends in music genres, release dates, and popular themes.
- Top-Performing Artists: Analyzing the top-performing artists, their most popular tracks, and their musical styles.
- Platform Popularity: Comparing the popularity of tracks across different streaming platforms.
- Visualizations: Interactive visualizations to bring data to life and aid in understanding complex relationships.

## Exploratory Data Analysis Structure:

- Overview of Dataset
- Basic Descriptive Statistics
- Top Performers
- Temporal Trends
- Genre and Music Characteristics
- Platform Popularity
-  Advanced Analysis

## Codes, Results and Visualizations

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

(refer to the attached csv file for the whole dataset)

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


```python
# Check for missing values in each column
print(df.isnull().sum())
```
![image](https://github.com/user-attachments/assets/68585687-6e6f-42f0-a734-960479f85e45)
```python
# Use pandas to fill every song that has a missing key with "C"
df.iloc[:, 15] = df.iloc[:, 15].fillna("C")
```
In this spreadsheet, we can see that the key "C" is not found in any of the songs, and there are songs that have a missing key. This is an obvious pattern because C major is one of the most common keys in songwriting. Use Pandas to fill every song that has a missing key with "C."

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
![image](https://github.com/user-attachments/assets/d98249f7-5b97-4c23-aa73-733c10505078)

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
![image](https://github.com/user-attachments/assets/9ac464da-76c2-4be6-bb71-ac90377b664e)
In the graph above, we can conclude that music from the 20th century have been streamed the most. From this, we can also conclude that music from the 19th century are considered outliers. By observing the distribution of artist count, we can tell that solo artists have been the most dominant, followed by duos and trios, which are mostly features and collaborations. We can conclude that bands with 3 or more members have been outliers.

## Top Performers

- Which track has the highest number of streams? Display the top 5 most streamed tracks.
  
```python
# Finding the top 5 tracks by streams
top_tracks = df.sort_values(by='streams', ascending=False).head(5)

# Display the output
print("Top 5 Most Streamed Tracks:")
print(top_tracks[['track_name', 'streams']])
```
![image](https://github.com/user-attachments/assets/c2251a3b-b02f-4f4f-be37-6262e436e49d)

## Temporal Trends
- Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.

```python
# Plot the number of tracks released per year

# Setting the figure size to make the plot larger and easier to read
plt.figure(figsize=(10, 6))

# Creating a count plot to show the number of tracks released per year
sns.countplot(x='released_year', data=df, palette="Purples", hue='released_year', dodge=False, legend=False)

# Setting the title of the plot to indicate the visualization purpose
plt.title('Number of Tracks Released Per Year')

# Labeling the x-axis to show it's representing years
plt.xlabel('Year')

# Labeling the y-axis to indicate it's showing the count of tracks released
plt.ylabel('Number of Tracks')

# Rotating the x-axis labels by 45 degrees for better readability
plt.xticks(rotation=45)

# Display the plot
plt.show()

```
![image](https://github.com/user-attachments/assets/19a9d63d-4ddb-4a29-bdb0-c645dc34b62e)

From the graph above, we can conclude that most of the tracks in the dataframe are songs from the 2020 onwards.

- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?

```python
# Group the data by 'released_month' and count the tracks

# Create a Series where the index is 'released_month' and values are track counts
tracks_per_month = df.groupby('released_month').size()

# Creating a diverging color palette with as many colors as there are months (in 'tracks_per_month')
colors = sns.color_palette("coolwarm", len(tracks_per_month))

# Setting up the figure size for the bar plot to make it large and readable
plt.figure(figsize=(12, 6))

# Plotting the track counts per month as a bar plot, using custom colors from the palette
# - kind='bar' specifies that the plot type should be a bar chart
# - color=colors applies the diverging color palette to the bars
tracks_per_month.plot(kind='bar', color=colors)

# Adding a title to describe the plot’s purpose
plt.title('Number of Tracks Released Per Month')

# Labeling the x-axis to indicate it's showing the months of release
plt.xlabel('Month')

# Labeling the y-axis to indicate it's showing the count of tracks released
plt.ylabel('Number of Tracks')

# Rotating x-axis labels to 360 degrees (essentially no rotation) for horizontal readability
plt.xticks(rotation=360)

# Adjusting the layout to ensure all elements fit well within the figure
plt.tight_layout()

# Displaying the plot

plt.show()
```
![image](https://github.com/user-attachments/assets/a9b2ec4c-1573-4d7f-9b50-c32639f4f879)

From the graph above, we can conlude that the Month of January has the most releases, this could be because record labels prefer to release their predicted hit songs as a way to jumpstart a year.

## Genre and Music Characteristics

- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%.
- Which attributes seem to influence streams the most?

```python
# Correlation between streams and musical attributes

# List of musical attributes to correlate with streams
musical_attributes = ['bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']

# Compute correlation matrix for streams and specified musical attributes
correlations = df[['streams'] + musical_attributes].corr()

# Set the figure size for the heatmap plot
plt.figure(figsize=(9, 6))

# Plot the heatmap of the correlation matrix with annotations and a contrasting colormap
sns.heatmap(correlations, annot=True, cmap="RdBu_r", fmt=".1g", linewidths=0.5)

# Add title to the heatmap for clarity
plt.title("Correlations with Streams (RdBu_r Colormap)")

# Display the heatmap
plt.show()

```
![image](https://github.com/user-attachments/assets/b816bf96-994a-481f-b8a0-c98c64ac3f5b)

The heatmap shows that all musical attributes have a weak negative correlation with the number of streams a track receives. Among these, speechiness has the strongest inverse relationship with streams, meaning that tracks with lower speechiness tend to have slightly higher stream counts.

- Is there a correlation between danceability_% and energy_%?
```python
# Create a scatter plot with purple markers
sns.scatterplot(x='danceability_%', y='energy_%', data=df, color='purple')

# Add a title to the plot
plt.title('Danceability vs. Energy')

# Label the x-axis
plt.xlabel('Danceability (%)')

# Label the y-axis
plt.ylabel('Energy (%)')

# Display the plot
plt.show()
```
![image](https://github.com/user-attachments/assets/26e20ab0-2e95-4c28-82d9-af7749741b95)

The scattered points suggest there isn't a clear link between danceability and energy, meaning they might not be strongly related in the given dataset.

- How about valence_% and acousticness_%?

```python
# Calculate the correlation coefficient

# This measures the linear relationship between these two variables in the DataFrame 'df'
correlation = df['valence_%'].corr(df['acousticness_%'])

# Printing the calculated correlation to understand the strength and direction of the relationship
print("Correlation between valence_% and acousticness_%:", correlation)

# Creating a scatter plot to visually examine the relationship between 'valence_%' and 'acousticness_%'
sns.scatterplot(x='valence_%', y='acousticness_%', data=df)

# Adding a title to describe the purpose of the scatter plot
plt.title('Valence vs. Acousticness')

# Labeling the x-axis to show it's representing 'valence_%' values
plt.xlabel('Valence (%)')

# Labeling the y-axis to show it's representing 'acousticness_%' values
plt.ylabel('Acousticness (%)')

# Displaying the scatter plot

plt.show()
```

![image](https://github.com/user-attachments/assets/5a2ddcf6-8ada-4ced-a35c-c2e6ed8d02fd)

The points are widely scattered across the plot without a clear trend, suggesting that there is likely little to no correlation between valence and acousticness in this dataset.

## Platform Popularity
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare?

```python
# Count the number of tracks in each playlist
spotify_playlists_count = df['in_spotify_playlists'].sum()
spotify_charts_count = df['in_spotify_charts'].sum()
apple_playlists_count = df['in_apple_playlists'].sum()

# Print the results
print("Number of tracks in Spotify playlists:", spotify_playlists_count)
print("Number of tracks in Spotify charts:", spotify_charts_count)
print("Number of tracks in Apple playlists:", apple_playlists_count)

```
![image](https://github.com/user-attachments/assets/a6cd3df6-9dde-4587-9fb9-f29eb60f6d98)

Based on the displayed data above, the platform that seems to favor the most popular tracks is the spotify

## Advanced Analysis

- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
  
```python
# Calculating the average number of streams by 'key' and 'mode'

# groupby(['key', 'mode']) groups the data by 'key' and 'mode' values
# ['streams'].mean() calculates the average streams within each group
# unstack() reshapes the result to have 'key' as rows and 'mode' as columns for easier plotting
key_mode_patterns = df.groupby(['key', 'mode'])['streams'].mean().unstack()

# Filling any NaN values in the DataFrame with 0
# - inplace=True modifies 'df' directly
df.fillna(0, inplace=True)

# Inferring better data types for 'df' columns to optimize memory or operations
# - infer_objects attempts to find optimal data types, here without copying
df = df.infer_objects(copy=False)

# Creating a heatmap to visualize the average streams by 'key' and 'mode'
# - figsize sets the dimensions of the plot to 10x8 inches
plt.figure(figsize=(10, 8))

# Creating the heatmap from 'key_mode_patterns' data
# - annot=True displays the data values on each cell in the heatmap
# - fmt=".0f" formats the annotations to display as integers
# - cmap="RdYlBu_r" sets a red-yellow-blue color scheme, reversed for emphasis
sns.heatmap(key_mode_patterns, annot=True, fmt=".0f", cmap="RdYlBu_r")

# Adding a title to the heatmap
plt.title('Average Streams by Key and Mode')

# Labeling the x-axis to represent mode (0 for Minor, 1 for Major)
plt.xlabel('Mode (0=Minor, 1=Major)')

# Labeling the y-axis to represent the musical key
plt.ylabel('Key')

# Displaying the heatmap
plt.show()

# Calculating the total streams for tracks in Major mode
total_streams_major = key_mode_patterns['Major'].sum()

# Calculating the total streams for tracks in Minor mode
total_streams_minor = key_mode_patterns['Minor'].sum()

# Comparing total streams between Major and Minor modes
# Printing which mode has a higher total stream count or if they are equal
if total_streams_major > total_streams_minor:
    print("Tracks in Major mode have a higher total stream count.")
elif total_streams_major < total_streams_minor:
    print("Tracks in Minor mode have a higher total stream count.")
else:
    print("Tracks in Major and Minor modes have the same total stream count.")
```
![image](https://github.com/user-attachments/assets/8db9b005-11c0-4562-a07a-9ce8e9e0a5e6)


Based on the graph above, songs in Major mode are more streamed than Minor modes

- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.
  
```python
# Ensure columns used for playlist counts are numeric for aggregation
platform_columns = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']
# Iterate over each column in the platform_columns list
for col in platform_columns:
    # Check if the column exists in the DataFrame
    if col in df.columns:
        # Convert the column to numeric, handling potential errors
        df.loc[:, col] = pd.to_numeric(df[col], errors='coerce')
    # If the column doesn't exist, do nothing (pass)
    else:
        pass

# Sum across specified columns to get 'total_playlists' and assign it to a new column in the DataFrame
df.loc[:, 'total_playlists'] = df[platform_columns].sum(axis=1)

# Top 5 artists by playlist appearances
# Group and ensure the sum is numeric to avoid TypeError
top_playlist_artists = df.groupby('artist(s)_name')['total_playlists'].sum().sort_values(ascending=False).head(5)

# Visualization of the Top 5 Artists by Playlist Appearances
plt.figure(figsize=(12, 6))

# Change the color palette here (replace "viridis" with your desired palette)
sns.barplot(x=top_playlist_artists.index, y=top_playlist_artists.values, hue=top_playlist_artists.index, palette="Set3", legend=False)
plt.title('Top 5 Artists by Playlist Appearances')
plt.xlabel('Artist Name')
plt.ylabel('Total Playlist Appearances')
plt.xticks(rotation=45)  # Rotate x labels for better readability
plt.show()

```
![image](https://github.com/user-attachments/assets/ff71309d-859e-42a0-a435-0f7f5e764411)

Unfortunately, the provided dataset does not include a 'genre' column. Therefore, a genre-based analysis is not feasible at this time. A genre column would allow us to identify trends and preferences across different musical genres.

```python
end


```
## Key Findings and Insights

### Distribution of Music
Majority of top music from 2023 came from the 20th century, which implies that majority listens to newer music. Songs from the 19th century served as outliers.

### Legacy of Music
This dataset proved that music is timeless, despite the domination of music from the 20th century, some songs from the 19th are still listened to by people.

### The domination of solo artists in Mainstream Music
This dataset proved that solo artists became the most listened to in 2023, followed by duo, which came from collaborations from solo artists, and lastly followed by music credited to three or more people (band).

### January as the preferred month for releasing music
Releasing music early in the year can give it a head start on the charts, potentially leading to year-end solid positions and boosting an artist's career. Record labels often capitalize on the holiday season to build anticipation for these releases. It can also be observed that there are
less songs in "ber months" to avoid clashing with christmas songs. 

### Top Performers
"Blinding Lights" by The Weeknd has the highest number of streams, followed by "Shape of You" by Ed Sheeran and "Someone You Loved" by Lewis Capaldi. Taylor Swift is the artist with the most tracks in the dataset, followed by The Weeknd and Bad Bunny.

### Platform Popularity
Spotify appears to favor the most popular tracks based on the number of tracks in its playlists compared to Spotify charts and Apple playlists.

### Songwriting Trends: A Reliance Towards Major Keys
Tracks in Major mode have a higher total stream count compared to Minor mode. This could be because listeners to prefer to listen to songs that are in major key, which has a happy vibe compared to minor key that has a sad or dark vibe. Another contributing factor could be that it is because 
writers and producers prefer to write in major key because it is easier to write hooks in this way.

## Conclusion
This data analysis has been a fulfilling activity because I learned to manipulate large amounts of data. With this activity, I also improved my programming skills. 
As someone who loves music theory and would like to release songs on Spotify in the future, I am glad to learn the metrics and numbers behind hit songs. 

## References
https://www.youtube.com/@RickBeato
https://musicalligator.com/blog/best-time-to-release-music/
https://www.kaggle.com/code/maurosteban99/eda-l-spotify-songs-2023
https://chatgpt.com/

## Author
Khalil T. Badal
