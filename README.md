# Danceability, Energy, and Song Popularity Analysis

Final Project for DSC 80 @ UCSD <br>
by Scarlett Jeffries &#128516;

## Introduction
I am choosing to work with a Spotify dataset, since I love music! Specifically, I enjoy upbeat and lively music, so I am interested in exploring if <strong>danceability</strong> and <strong>energy</strong> can predict the <strong>popularity</strong> of a song. Further, I am interested in exploring whether this relationship is counfounded by the <strong>genre</strong> of the song or the <strong>popularity</strong> of the artist. <br>

This analysis will first explore the dataset to understand the variables I am working with, with the goal of using the data to predict a particular song's popularity based on its danceability and energy scores. Next, it will explore whether the difference in popularity based on danceability and energy varies by genre by looking at 5 distinct genres: pop, hip-hop, rock, dance, and indie. Lastly, it will explore whether the association between song popularity and danceability/energy is confounded by artist popularity, and potentially explore the predictive model relative to the popularity of the artist. <br>

Using the <strong>Spotify Music Track</strong> dataset adapted from the Spotify Dataset 1921-2020 by Yamac Eren Ay on Kaggle. This dataset contains data on the song metadata and artist data, which I will be combining to look at a few key features for this analysis. The columns of interest are:
- `artists`: the name of the artist, or artists, if there are multiple.
- `track name`: the song name
- `song popularity`: the Spotify popularity score between 0–100 based on total plays and recency, where higher = more popular
- `danceability`: how suitable the track is for dancing, based on tempo, rhythm stability, and beat strength (a score between 0–1)
- `energy`: perceptual intensity and activity of the track: fast, loud, and noisy tracks score high, whereas classical music scores low (a score between 0–1)
- `track genre`: the genre of the track, as labeled by Spotify
- `artist popularity`: the artist-level popularity score between 0–100 indicating how popular an artist is (distinct from track popularity)

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
In order to make the dataset more organized for the purpose of the analysis, I went through the following steps after loading in both the `music_tracks` and `artists` data sets.
1. The `music_tracks` dataset listed multiple artist names separated by semicolons if a particular song had multiple artists. I first separated the artists into a list of artist names for each song, then exploded the `artists` column such that each row in the dataset for a song with multiple artists contained the same metadata, and each artist in the song had their own row.
2. I performed a left-merge between the `music_tracks` and `artists` data, joining them on the column representing the name of the artist.
3. Selected columns of interest for the analysis, which were `track_id`, `artists`, `track_name`, `popularity_x`, `danceability`, `energy`, `track_genre`, `name`, and `popularity_y`, with popularity_x and popularity_y representing song popularity and artist popularity, respectively. I later updated these column names to explicitly be `song_popularity` and `artist_popularity`.
4. Since I exploded the dataset to get the rows on an artist level, I brought back to the song-level since I am interested in song popularity for this analysis. I grouped the data by the `track_id`, and stored the `artists` for songs with multiple artists as a list of artist names, and did the same thing for the `artist_popularity` column. 
5. I converted the `artists_popularity` column to be the mean popularity score of all of the artists on the song. Since artist popularity is something I am interested in working with for this analysis, taking the mean seemed to represent the general popularity of the artist well.
6. Lastly, I selected only 5 genres of interest for this analysis, and filtered the data for only songs labeled as `pop`, `hip-hop`, `rock`, `dance`, or `indie` since these genres cover a diverse range of music, and generally contained a sufficient amount of data as these are popular genres.
<br>
Take a look at the first few rows of the finalized dataset below: <br>

|   level_0 |   index | artists                                                  | song_name               |   song_popularity |   danceability |   energy | genre   |   artist_popularity |
|----------:|--------:|:---------------------------------------------------------|:------------------------|------------------:|---------------:|---------:|:--------|--------------------:|
|         0 |       3 | ['Jordan Sandhu']                                        | Teeje Week              |                62 |          0.679 |    0.77  | hip-hop |                58   |
|         1 |      98 | ['Black Eyed Peas']                                      | I Gotta Feeling         |                 1 |          0.746 |    0.793 | dance   |                86   |
|         2 |     110 | ['Bryan Adams']            | Merry Christmas         |                 0 |          0.683 |    0.511 | rock    |                79   |
|         3 |     198 | ['Dua Lipa']                                             | Break My Heart          |                78 |          0.73  |    0.729 | dance   |                95   |
|         4 |     215 | ['MC STAN']                                              | Astaghfirullah          |                58 |          0.653 |    0.725 | hip-hop |                57   |
|         5 |     247 | ['Mac Miller', 'Ty Dolla $ign']                          | Cinderella              |                 0 |          0.408 |    0.533 | hip-hop |                87.5 |
|         6 |     347 | ['Charlie Puth']                                         | One Call Away           |                 4 |          0.667 |    0.613 | dance   |                82   |
|         7 |     352 | ['Wiz Khalifa', 'Girl Talk'] | Big Daddy Wiz           |                 0 |          0.9   |    0.795 | dance   |                71   |
|         8 |     384 | ['Lizzo', 'Pink Panda']                                  | Boys - Pink Panda Remix |                 0 |          0.853 |    0.938 | hip-hop |               nan   |
|         9 |     402 | ['Bryan Adams']            | Summer Of '69           |                 0 |          0.5   |    0.908 | rock    |                79   |

### Univariate Analysis
First, I wanted to take a look at the general distribution of song popularity scores, to get a better understanding of the data.
<iframe
  src="assets/song_popularity.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As you can see, there seems to be a disproportionate amount of songs with a popularity score between 0-4. In order to reduct the effects of this on the analysis, while still preserving its impact in the data, I filtered the `song_popularity` to only contain songs with a popularity score greater than 2. See the results of that below.
<iframe
  src="assets/song_popularity2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
Since I am interested in understanding `danceability` and `energy` scores so that they can be used to predict `song_popularity`, I decided to look at the relationship between each of those features and song popularity.
<iframe
  src="assets/danceability_popularity.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/energy_popularity.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As you can see, there is not a strong correlation between either `danceability` or `energy` with `song_popularity`. Since there is a very slight correlation, I still plan on using them, but should investigate other factors that influence `song_popularity` as well. I decided to look at the relationship between `artist_popularity` and `song_popularity` since, logically, songs released by popular artists will likely be popular since popular artists have dedicated fans to listen to their music. See the outcome of this plot, and the positive correlation between artist and song popularity, below.
<iframe
  src="assets/artist_song_popularity.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
I decided to further investigate how `artist_popularity`, `danceability`, `energy`, and `song_popularity` differs by genre. In the table below, the average of each of these features by genre is listed, sorted by decreasing average `song_popularity`. There are a couple interesting values that I noticed:
- `indie` has generally lower scores across all features
- `dance` has a significantly higher average artist popularity
- `pop` has the highest average song popularity
- `hip-hop` has the highest average danceability and energy, yet their average song popularity is one of the lowest

| genre   |   artist_popularity |   danceability |   energy |   song_popularity |
|:--------|--------------------:|---------------:|---------:|------------------:|
| pop     |             70.4764 |       0.590764 | 0.601188 |           64.1845 |
| rock    |             75.4531 |       0.5825   | 0.68252  |           62.6429 |
| dance   |             82.5419 |       0.679802 | 0.691587 |           62.2961 |
| hip-hop |             63.9738 |       0.709017 | 0.703697 |           60.4102 |
| indie   |             50.0678 |       0.609059 | 0.608306 |           54.0118 |

Since these features differ across genres, I will be sure to keep that in mind in later sections of this analysis.

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

