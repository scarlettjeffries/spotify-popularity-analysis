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
After checking null values, out of the columns that I am working with, only the `artist_popularity` column contains null values. Overall, I am working with 1402, with 139 of them having an empty `artist_popularity` score. I think that this score could be NMAR, as new artists may not have a popularity score yet. For instance, an artist who just released their first song may have all of the other song metadata, however since the artist has not released music before, they may not yet have an `artist_popularity` score. In order to assess this, I performed a missingness dependency permutation test to investigate whether `artist_popularity` is dependent on the column, `artist`. 
- <strong>Null Hypothesis: </strong> the missing `artist_popularity` scores is independent of the `artist`
- <strong>Alternate Hypothesis: </strong> the missing `artist_popularity` scores is independent of the `artist`
- <strong>Test Statistic: </strong> using `.mean().var()` to compute the mean variance across artists for each simulation
- <strong>Significance Level: </strong>0.05
By repeatedly shuffling the artist popularity missingness 1000 times, I collected 1000 simulated mean variances. 
<iframe
  src="assets/missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed test statistic of <strong>0.169</strong> is represented as the red line in the plot. Since the p_value that we found <strong>0.0</strong> is < 0.05 which is the significance level that we set, we reject the null hypothesis, indicating that the `artist_popularity` does depend on the `artist`. I repeated this same procedure with the `danceability` and `artist_popularity`, and got a p-value of <strong>0.055</strong>, which is just over the significance level, meaning we cannot conclude that `artist_popularity` is dependent on `danceability`.

## Hypothesis Testing
As mentioned in one of the previous tables, `dance` artists have the highest average `artist_popularity` score across all of the genres I am looking at. In this section, I am conducting a permutation test to investigate whether dance artists are more popular than artists of other genres. More specifically:
- <strong>Null Hypothesis:</strong> Dance artists have equal artist popularity scores to non-dance artists
- <strong>Alternate Hypothesis:</strong> Dance artists are have higher artist popularity scores than non-dance artists
- <strong>Test Statistic: </strong> Difference in means (Dance artist mean - non-dance artist mean for each simulation)
- <strong>Significance Level: </strong>0.05
By repeatedly shuffling the genre 1000 times, I collected 1000 mean differences in artist popularity between dance and non-dance artists.
<iframe
  src="assets/dance_hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The observed test statistic of <strong>16.668</strong> is represented as the red line in the plot. Since the p_value that we found <strong>0.0</strong> is < 0.05 which is the significance level that we set, we reject the null hypothesis, indicating that the mean `artist_popularity` is indeed higher in `dance` artists than non-dance artists.

## Framing a Prediction Problem
### Problem Identification
After investigating the relationships between some variables, I plan to build a model to predict the <strong>popularity</strong> of a song based on its <strong>danceability</strong>, <strong>energy</strong>, and the <strong>artist's popularity</strong>. This is a regression problem to predict the `song_popularity` variable. Predicting the popularity of a song could be helpful for people who are planning to release music, especially upbeat music as I am using `danceability` and `energy` as features. Once a song is produced, the musician can esitmate or use sound analytics to find a `danceability` and `energy` score, so this information would be known at the time of prediction. Additionally, `song_popularity` changes according to users streams, however `danceability` and `energy` scores remain the same. I am also choosing to use `artist_popularity` as a feature, since as I explored above, there is a significant positive relationship between `artist_popularity` and `song_popularity`, meaning that it would likely be an influential factor when predicting song popularity.

 Note that danceability and energy were not found to be that strongly correlated to song popularity, but since they are more impactful for certain genres such as dance and rock, I am planning on using them in this model.

## Baseline Model
For the baseline model, I used a <strong>Linear Regression</strong> model with 3 predictors:
- `danceability`: the danceability score between 0-1
- `energy`: the energy score between 0-1
- `artist_popularity`: the artist popularity score between 5-100
    - The scores fall between 5-100 rather than 0-100, since there was a disproportionate amount of scores close to 0 that were causing higher RMSE of the model, so I chose to focus on scores 5-100 since it still includes low-popularity scores, while keeping a significant amount of the data.
    - Additionally, I dropped the rows with null artist popularity values, since there were only 139 out of 1402, which left me with 1263 data entries - a sufficient amount of data to build this model with.

All of these features are <strong>quantitative</strong> (numerical) variables, so no additional encoding happened at this stage.
I used <strong>StandardScaler</strong> to standardize the features, and built the model as a `sklearn` `Pipeline`. After fitting the model to the training data and predicting `song_popularity` on the test data, I found the performance of this model to be fairly poor. This model had a baseline RMSE of <strong>14.623</strong> and a R<sup>2</sup> of <strong>0.064</strong>.

The low R<sup>2</sup> indicates that very little of the variance of the `song_popularity` is explained by the features in the current regression model. I plan to improve it by tuning hyperparameters and adding features in the next section.

## Final Model
The final model I made added 3 new features:
- `upbeatness`: After plotting the relationship between danceability and energy, there was a slight positive correlation between the two, so I added a feature representing the `danceability` x `energy`
- `artist_popularity_relative`: Since there were distinct differences across genres as seen previously, I changed the `artist_popularity` to be relative to the genre it is in. The former `artist_popularity` score was replaced with the score relative to the genre
- I added `genre` as a <strong>One-Hot Encoded</strong> nominal (categorical) feature. As `genre` seemed to be an important factor throughout this analysis, I used <strong>ColumnTransformer</strong> to <strong>One-Hot Encode</strong> the genre, so that it can be used as a feature in the model.

Additionally, I changed the <strong>Linear Regression</strong> model to a <strong>Ridge Regression</strong> model. Since there are now more features than there previously were, ridge regression regularizes the model's coefficients to balance between a perfect fit and a generalized model. It penalizes the weight of the features' coefficients, so that the model can keep all features while still preventing overfitting. Additionally, the <strong>penalty (L2 regularization)</strong> is the <strong>hyperparameter</strong> I investigated to find the best penalty weight (alpha). I used <strong>GridSearchCV</strong> with the weight levels <strong>[0.01, 0.1, 1, 10, 100]</strong> to find the best estimator for the model. The best weight level (alpha) was <strong>1</strong>, resulting with the final model having a RMSE of <strong>14.0429</strong> and a R<sup>2</sup> of <strong>0.137</strong>.

While the RMSE did not shrink significantly, the R<sup>2</sup> nearly doubled. While <strong>0.137</strong> is still a fairly low level of variability explained by the features, the model performance definitely improved from the baseline model.

## Fairness Analysis
In the beginning, I questioned whether an artist's popularity would override the performance of the model, as a popular artist likely has a loyal fan base listening to the song. To evaluate the fairness of the model, I am going to see if the model performs better for less-popular artists vs popular artists. Specifically, I am defining "popular artists" as artists with a `popularity score` greater than or equal to <strong>70</strong>, and less-popular artists as artists with a popularity score less than <strong>70</strong>. For the evaluation metric, I will be using R<sup>2</sup>, since artist popularity is one of the predictors, and R<sup>2</sup> explain what proportion of the song popularity variance is explained by the predictors. R<sup>2</sup> was also the performance indicator that changed the most after improving the model, so I believe it would be a good statistic to evaluate the fairness of the model as well.

In this permutation test, the null hypothesis would claim that the model performs equally accurate for less-popular and popular artists, and the alternate hypothesis claims that the model performs more accurately for less-popular artists. I am choosing these groups under the assumption that a song by a popular artist will automatically be popular, which slightly counfounds/dominates the model, and that lower levels (meaning, an up and coming artist) of popularity allow the model to work more accurately.

- <strong>Null Hypothesis:</strong> The model performs with equal accuracy for less-popular and popular artists 
- <strong>Alternate Hypothesis:</strong> The model performs more accurately for less-popular artists than popular artists
- <strong>Test Statistic: </strong> Difference in R<sup>2</sup> (less-popular artist R<sup>2</sup> - popular artist R<sup>2</sup>)
- <strong>Significance Level: </strong>0.05
By repeatedly shuffling the genre 1000 times, I collected 1000 mean differences in R<sup>2</sup> between less-popular and popular artists. Additionally, I have plotted the distribution of test statistics and the observed statistic below, following the same format as the previous permutation test plots.
<iframe
  src="assets/artist_popularity_difference_perm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As we can see, the p-value is <strong>0.027</strong>, which is < 0.05 (threshold), meaning that the model does indeed perform better for less-popular artists who are not dominating the charts. Logically, this makes sense, as a song Taylor Swift releases is likely to be popular regardless of the danceability or energy, whereas these features may be more influential (therefore making the model more accurate) for an up and coming artist.


## Conclusion
Overall, `artist_popularity` seemed to be the most influential factor in predicting a song's popularity. Additionally, the ridge regression model predicted with the most accuracy for less-popular artists. While I wish the model was able to perform more accurately overall, there was still significant improvement betwen the baseline and final model. If you have read this far, thank you! And, if you have any <strong>danceable</strong> or <strong>energetic</strong> songs that are <i>not</i> popular yet, feel free to send them my way at scjeffries@ucsd.edu (I am always looking for new, fun songs). Thank you!
