# Danceability, Energy, and Song Popularity Analysis

Final Project for DSC 80 @ UCSD using Spotify Dataset
by Scarlett Jeffries

## Introduction
I am choosing to work with a Spotify dataset, since I love music! Specifically, I enjoy upbeat and lively music, so I am interested in exploring if <strong>danceability</strong> and <strong>energy</strong> can predict the <strong>popularity</strong> of a song. Further, I am interested in exploring whether this relationship is counfounded by the <strong>genre</strong> of the song or the <strong>popularity</strong> of the artist. <br>

This analysis will first explore the dataset to understand the variables I am working with, with the goal of using the data to predict a particular song's popularity based on its danceability and energy scores. Next, it will explore whether the difference in popularity based on danceability and energy varies by genre by looking at 5 distinct genres: pop, hip-hop, rock, dance, and indie. Lastly, it will explore whether the association between song popularity and danceability/energy is confounded by artist popularity, and potentially explore the predictive model relative to the popularity of the artist. <br>

Using the <strong>Spotify Music Track</strong> dataset adapted from the Spotify Dataset 1921-2020 by Yamac Eren Ay on Kaggle. This dataset contains data on the song metadata and artist data, which I will be combining to look at a few key features for this analysis. The columns of interest are:
- `artists`: the name of the artist(s)
- `track_name`: the song name
- `song popularity`: Spotify popularity score from 0–100, based on total plays and recency. Higher = more popular
- `danceability`: how suitable the track is for dancing, based on tempo, rhythm stability, and beat strength (0–1)
- `energy`: perceptual intensity and activity: fast, loud, noisy tracks score high; classical scores low (0–1)
- `track genre`: genre of the track, as labeled by Spotify
- `artist popularity`: artist-level popularity score from 0–100, which is distinct from track popularity

## Data Cleaning and Exploratory Data Analysis
<iframe
  src="assets/song_popularity.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

