---
date: '2021-12-19T12:50:54.000Z'
title: Exploring the mood difference between Rock and Pop music
tagline: >-
  Course Project for STA304, Surveys, Sampling and Observational Data (Image:
  Fandom)
preview: >-
  It is often believed that rock music is louder and has stronger beat than pop
  music, is this really the case?
image: >-
  https://drive.google.com/uc?export=view&scale.option=fit&id=1fnDlsCn8T0EBBeus5zFD7Fs5HKz29yBv
---

# Abstract
In this paper, we have explored how energy and valence (positiveness) differs between rock music and pop
music, as many suggest that rock music is more energetic and pop music is more depressing. We have utilized
music data from the Spotify API by fetching Spotify’s official rock/pop playlists, and we cleaned these data to
include the audio features of the tracks for the analysis. The statistical method of propensity score matching
is implemented to match rock/pop pairs that have a similar chance of being rock, which we computed using
logistic regression. Then, the balance between rock and pop was checked and we concluded the analysis by
fitting linear regression models to estimate the energy and valence of a music piece, based on the genre and
audio features. Results from the regressions support the hypothesis that rock music is more energetic and
positive compared to pop music, and the correlating factors such as loudness and danceability also showed to
be statistically significant. However, the assumptions for propensity score matching are not fully met, and
model assumptions were simply assumed. These are weaknesses of this paper and should be the focus of
future researches.

# Key words
Music, Rock Music, Pop Music, Mood, Energy, Positiveness, Marginal Propensity Score Matching, Logistic
Regression, Linear Regression, Spotify

# Introduction
Music has been with us for centuries, and it is becoming more popular over the past decades. There are many
genres of music, in particular, there is Rock music, also referred to as rock and roll, which emerged in the
United States in the 1950s and had become more and more popular until today (Frith, 2021). Although it
is difficult to define rock music accurately, there is some consensus on the “strong beat” and “loudness” of
this genre (Frith, 2021). In this paper, we will explore the uniqueness of rock music compared to popular
(pop) music, which is, by Merriam-Webster’s definition, “music written and marketed with the intention of
achieving mass distribution and sales now principally in the form of recordings” (Merriam-Webster). Some
definitions also refer to pop music as “the softer alternative of rock” (Difference Between, 2011). As any song
that goes viral or is commonly heard of can be classified as pop music, it is possible for a track to be both
rock and pop, making the boundary between the two genres ambiguous.

For this paper, we will leverage Spotify, a music streaming provider, its playlists, and its public database to
fetch data on the representative rock and pop pieces.

# Research Question
We will focus on the two aspects of music, energy, and valence. In the context of our data, energy refers to
the “perceptual measure of intensity and activity”, and valence refers to the “musical positiveness conveyed
by the track” (Spotify). Energy and valence can be used together to represent the mood, such as low energy
and low valence might represent pure depressing, where high energy and low valence can evoke anger. Hence,
the research question is, “**How does the mood (energy/valence) of Rock music differ from Pop
music?**”.

Hopefully, by the end of the paper, we can utilize data to provide further classifications between rock music
and pop music, in terms of the different moods that the two genres lead to. In addition, we will also
incorporate measures, such as loudness and tempo, to provide further explanations and distinctions. This
highlights the importance of this analysis, which is to utilize representative rock and pop music pieces to
explore the differences between the genres and provide objective comparisons. We will use data to justify
the differences, such as is rock music actually louder and more energetic? If you think rock music is more
depressing than pop music, is it actually the case? Is it solely depressing or is it more aggressive and “amplifies
the message it is trying to send” through louder volume and stronger beats (Bhatia, 2017)? We will consider
these aspects in the analysis.

Globally, identifying the differences between rock music and pop music can greatly help the music industry
understand the underlying features of one genre, and also realize the characteristics of the audiences for this
genre. By knowing the distinctive traits of one genre, artists can emphasize their production on these traits
and maximize the advantages to attract more listeners.

# Terminology

To identify the differences, we will utilize the statistical tool of marginal propensity score matching, which also
requires linear regression and logistic regression and their supplementary tools. Basically, we are estimating
the chance for a music piece to be classified as Rock, using logistic regression, and match and compare pieces
with similar chances of being rock, but in fact one is rock and one is pop. Then, we examine the actual
impact of being rock music on energy and valence using linear regression on the matched data. We will
elaborate on the methodology later.

To forecast the genre of a song, we will utilize the audio features including danceability, mode, tempo, and
many more. Generally, these are music characteristics that describe how the music sound. For example,
different modes may express different emotions, such as a major mode usually sounds happy and a minor
mode sounds sad (NME Blog, 2013). We will elaborate on the characteristics and their definitions when we
discuss them in the methodologies.

# Hypothesis

As mentioned in the previous paragraphs, it is commonly perceived that rock music is louder and more
energetic than popular music, perhaps due to the emphasis on electric instruments. Therefore, we hypothesize
that the analysis will yield a result suggesting that compared to popular music, rock music will be more
energetic. As for valence, prior research suggests that for Rock’s subgenre, Heavy Metal, “listeners [are] more
anxious, depressed” (Besant, 2013). However, Heavy Metal is only a part of rock, and “Pop-rock and power
metal are almost always upbeat and happy”, inferring inconsistent perceptions on the positiveness of rock
music (Head, 2017). On the other hand, research suggests that pop music is “by far, the most depressing
lyrics of all genres since 2008” (Berkowitz, 2019). Therefore, we can hypothesize that pop music will have a
lower valence, or rock will have higher valence on average.

# Data
## Data Collection

In this paper, we will utilize the Spotify API, which is similar to a port where developers can fetch data
about all the songs, albums, playlists that are on Spotify. To collect data about the representative rock and
pop pieces, we have browsed through the playlists on Spotify and purposefully chose a few. As mentioned in
the introduction, the classification of rock and pop music can be vague, so we decided to only use the official
playlists provided by Spotify, which should be more authentic and rigorous in the classification process. We
also aimed to match the sample size for the two genres. In the end, we chose 2 rock playlists and 4 pop
playlists, listed below:

Rock:
1. Rock Classics: “Rock legends & epic songs spanning decades, that continue to inspire generations.”
ID:37i9dQZF1DWXRqgorJj26U
2. Rock Party: “The ultimate rock party playlist!” ID:37i9dQZF1DX8FwnYE6PRvL

Pop:
1. Pop International: “The best in English of the last decade.” ID:37i9dQZF1DX1ngEVM0lKrb
2. Pop Party: “All the jams to get the party going!” ID:37i9dQZF1DWXti3N4Wp5xy
3. Classic Pop Picks: “A trip down pop’s memory lane.” ID:37i9dQZF1DX4v0Y84QklHD
4. Soft Pop Hits: “Listen to easy songs from your favorite artists!” ID:37i9dQZF1DWTwnEm1IYyoj
There are some overlaps between playlists of the same genre, which we will remove the duplicates. When
combining the two genres’ playlists into a full list, we notice that the songs are unique, meaning that none of
them are both rock and pop.

## Data Cleaning
As mentioned earlier, we fetched the data on the 6 playlists using the Spotify API. After setting up the API
connection by authenticating the keys, we followed the procedure below to make sure our data set consists of
what we need:
1. For each playlist, we utilize the function to get playlist audio features, which returns a large data set
including all the audio features for all the songs in the playlist. We are only interested in some of the
variables in certain ways.
2. Extract the artist’s name from the track.artist variable, and include it in the data set as a new variable.
3. Change the track duration from milliseconds to seconds by dividing by 1000.
4. Extract the first 4 characters in the track release data, which represents the year released, and change
it to numeric.
5. Select the following variables: name, artist, album name, valence, energy, danceability, key, loudness,
mode, speechiness, acousticness, instrumentalness, liveness, tempo, time_signature, id, duration_sec,
popularity, year
6. For the name variable, we want to remove any suffix such as “- Live at . . . ”, “- Remastered..”, or “(Live
at)”, which we can do so by looking for the dash symbol or bracket.
7. Then, we remove any duplicate song by looking for duplicate song names.
8. Lastly, we remove any song with missing values to prepare us for the regressions, which require
non-missing values.

The above procedure prepares the cleaned data set for one playlist. To merge multiple playlists:
1. First use the above procedure to obtain the cleaned data set for the new playlist.
2. For each song in the new playlist, if the same song does not exist in the full playlist, add this song to
the full playlist.

We created two playlists representing the two genres, which we have 295 pop tracks and 222 rock tracks.
There is a small discrepancy between the sample sizes, but they are roughly similar. Lastly, we merge the
two genres into a full playlist of 517 songs to be used to the analysis. We also make sure there is no duplicate
songs in the full playlist, which we observe that all 295 + 222 = 517 are kept, so the songs are either rock or
pop.

# Variables and summaries
Now we will explore some of the traits of the full data set. We will also compare the two genres.

## Energy
Energy represents the intensity of the music. Ranging from 0.0 to 1.0, a lower value represents lower intensity
and calmer music, and a higher value represents more energetic music.

Energy is one of the variables that we are interested in the difference between the two genres. In fact, the
average energy for rock music is 0.76 compared to pop music’s 0.64. This supports our hypothesis that rock
music is generally more energetic. In Figure 1, notice how energy is distributed differently between the two
genres, with a clear trend indicating rock music is on the high side for energy.

In addition, Figure 1 suggests that for both genres, energy is generally left-skewed, with most of the tracks
having medium to high energy. This may affect the normality assumption for the regressions, which we have
to look out for.

![Figure1](https://drive.google.com/uc?export=view&scale.option=fit&id=1hvDmOPDLY5LdGzlERijfpAL2cFh9r6nv)

## Valence
Valence measures positiveness, it also ranges from 0.0 to 1.0, where the lower value represents depressing and
the higher value represents happiness and joy.

We explore the distribution of valence similarly, and it suggests that the average positiveness of rock music
is 0.62, higher than pop music’s 0.5. Again, this coincides with our hypothesis, which suggests that pop
music is the most depressing genre in general. In Figure 2, notice how valence is distributed more evenly and
normally for the two genres. Rock music is slightly leaning to the positive side, which caused its average
valence to be slightly higher than pop.
![Figure2](https://drive.google.com/uc?export=view&scale.option=fit&id=1F7dkfklkoQUi2wfeGZa1yA9PlFFGtfNq)

## Mood (Energy + Valence)
When we look at energy and valence in a 2-dimensional perspective in Figure 3, it illustrates the mood of
the song. Referencing RCharlie’s notation technique on the four quadrants, we notice that the blue dots,
representing rock music, are generally to the top right of the red, pop dots (Sentify by RCharlie). This
suggests that rock music is on average more happy and joyful compared to pop music, which is an outcome
due to the higher energy and higher valence.

We also notice how there might be a positive correlation between Energy and Valence, such that they tend
to increase or decrease simultaneously. For example, we do not see many Chill/Peaceful songs with high
positiveness but low energy.
![Figure3](https://drive.google.com/uc?export=view&scale.option=fit&id=1sdkk1KJrOzBUt2Ipq62pbfrz14rMlf_8)

## Loudness
Loudness is the “overall loudness of a track in decibels (dB)” (Spotify). As shown in Figure 4, these values
are negative, which is because decibel is “a comparison with a reference value”, and “a negative dB means
that you are a few times softer than that threshold” (Ashish, 2021). However, the interpretation is the same,
such that higher decibel represents louder volume.

As mentioned in the introduction, many definitions of rock identify the genre to be loud. However, as shown
in Figure 4, rock music is actually quieter compared to pop music, but it is also more spread out than pop
music. On the other hand, there are outliers, song much quieter than the rest, for both genres, which may be
causing the lower averages.
![Figure4](https://drive.google.com/uc?export=view&scale.option=fit&id=1TXvbMEqHDhx6criYxjQ8x8ncbQacYCNR)

## Year
Year is a categorical variable in our case, such that we have songs released from 1964 to 2021. When fitting
our regressions, we will utilize indicator variables to represent the year that the song’s released, which is more
appropriate than numerical variables because we expect the impact of year to be non-linear.

When looking at Figure 5, it indicates that the pop music in the data set is mostly from the years 2000 to
2021, which is different from rock music’s larger spread. This can be an issue when applying regressions
because generally, we know that if the song was released in 1964, for example, then the chance of it being
rock music is 100%. These points are almost leverage and outlier points that will attract the regression line
towards it (STA302). On the other hand, after we calculate the propensities, we would get a large imbalance
for the year variable because the rock and pop music in our data set have significantly different release years.

Considering these factors, we chose not to include year as a predictor variable due to the imbalance between
the two groups and its ability to affect the propensities.
![Figure5](https://drive.google.com/uc?export=view&scale.option=fit&id=15DTAk8bXy1muekAKuclLiHCBk_gn_-c-)

# Methods
To analyse the difference between rock music and pop music, we have utilized the statistical method of
Propensity Score Matching, which also required logistic regression, linear regression, and some supplementary
tools.
## Propensity Score Matching
Propensity Score Matching is a technique used to estimate the propensity, or chance, for an observation to be
treated. In our context, being treated means that the track is classified as “rock”, while non-treated is “pop”.
To apply this method, we have followed the procedure below:

1. First, we used logistic regression to estimate the propensity for an observation to be rock, using the
covariates of audio characteristics. The logistic regression model used is elaborated on below.
2. Then, we applied propensity score matching under the nearest neighbor matching technique implemented
in the ARM’s package. This technique matches observations, in pairs, with a similar propensity of
being treated or being rock, but actually one of them is rock and one of them is pop. We save all the
matched pairs from the previous step in a new data set, which will be used in step 4.
3. Then, we checked the quality of matching using t-tests on difference-in-means.
4. Finally, we examined the effect of being rock music on the energy and valence of the music using linear
regression on the matched data set. This means that we have the same amount of rock music and pop
music to construct these two regressions. The linear regression model used is elaborated below.

## Logistic Regression
In step 1 of PSM, we utilized the logistic regression model to estimate the propensity for a music track to be
treated, or being rock music. The logistic regression model is, simply put, estimating the probability for an
observation to be treated based on its audio features. We have the following audio features: danceability,
key, loudness, mode, speechiness, acousticness, instrumentalness, liveness, tempo, time signature, duration in
seconds, original year of release. Although most of them are very straightforward, we will look at them one
by one. The logistic regression model is presented below:
![LogisModel](https://drive.google.com/uc?export=view&scale.option=fit&id=1w1IvwmuwjQLbP3Mk7RyVVTiNZKBndF_7)
where p indicates the probability of the track being rock.
- β0 represents the intercept of the model. It is the log odds of being rock given the song has the key of
0, minor mode, time signature of 1/4, and all other audio features equal to 0. Since it does not make
sense for a song to be 0 seconds long, or have a 0 tempo, the intercept is irrelevant under this context.
- β1 represents the slope of danceability, xdanceability. Danceability is a numerical variable ranging from
0.0 to 1.0, which a higher value means that the song is more danceable (Spotify). Hence, the coefficient
represents how the log odds change when danceability changes, holding everything else constant.
- β2 represents the slope of key, xkey. Key is a categorical variable taking integer values from 0 to 11,
which “map to pitches using standard Pitch Class notation” (Spotify). As the model drops the case
when key equals zero, the coefficients on the remaining 10 indicator variables represent the change in
the log odd compared to the case when key equals zero.
- β3 represents the slope of loudness, xloudness. Loudness was defined in the previous section. The
coefficient represents how the log odds change when loudness changes, holding everything else constant.
- β4 represents the slope of mode, xmode. Major mode is represented by 1, and minor mode is represented
by 0 (Spotify). The coefficient represents the change in the log odds when the song is in a major mode,
as the other case is dropped to avoid multicollinearity.
- β5 represents the slope of speechiness, xspeechiness. Speechiness ranges from 0.0 to 1.0, measuring the
“presence of spoken words in a track” relative to the existence of music, with a higher value representing
a larger presence of spoken words (Spotify). Hence, the coefficient represents the change in log odds
when the presence of spoken words changes in the track, holding everything else constant.
- β6 represents the slope of acousticness, xacousticness. Acousticness ranges from 0.0 to 1.0, representing
if the music is acoustic, which is when the music is “not electronically modified”, and a higher value
corresponds to more acoustic (Spotify; Merriam-Webster). Hence, the coefficient represents the change
in log odds when the music is more acoustic or more electric, holding everything else constant.
- β7 represents the slope of instrumentalness, xinstrumentalness. Instrumentalness ranges from 0.0 to 1.0,
“predicting whether a track contains no vocals”, and the larger the value, the less the vocal (Spotify).
Hence, the coefficient represents the change in log odds when the number of vocals changes in the song,
holding everything else constant.
- β8 represents the slope of liveness, xliveness. Liveness ranges from 0.0 to 1.0, detecting the presence of
audiences in the recording, with a higher value indicating a greater likelihood of a live performance
(Spotify). Hence, the coefficient represents the change in log odds when the song is predicted to be
performed live, holding everything else constant.
- β9 represents the slope of tempo, xtempo. Tempo is the overall estimated beats per minute of the song,
with higher values indicating a faster beat and vice versa (Spotify). Hence, the coefficient represents
the change in log odds when the pace of the song changes, holding everything else constant.
- β10 represents the slope of time signature, xtimesignature. “The time signature ranges from 3 to 7
indicating time signatures of ‘3/4’, to ’7/4”, but there is also a rare 1/4 signature (Spotify). This is
another measure for the pace of the track, but it is categorical since it takes integer values in a certain
range. Hence, the coefficient represents the relative difference in log odds when the time signature is
other than 1/4, the dropped subcategory, holding everything else constant.
- β11 represents the slope of duration, in seconds, xduration(s). This is straightforward, hence the coefficient
represents the change in log odds when the length of the song changes, holding everything else constant.

To convert the log odds into the actual probabilities, the following conversion is done:
![LogOdd](https://drive.google.com/uc?export=view&scale.option=fit&id=1uhZyeU-W_msbT1bSUtjFEcVA4ejcH-Ks)
where x is the log-odds calculated by the right-hand side.

## Linear Regression
After propensity score matching, we utilized linear regression on the matched data set to estimate the energy
and valence of a song, given the 12 variables that represent the audio features we used in the logistic model,
and also the treatment, so whether or not it is a rock song. We used two separate models to estimate energy
and valence accordingly, but the predictors are identical. Generally, the linear regression model used is:
![LinearRegression](https://drive.google.com/uc?export=view&scale.option=fit&id=13MwKlOcADd62SF1mTYKV5R_A9IzlgD-M)
Now, the response variable is either Energy or Valence, both ranging from 0.0 to 1.0 as introduced earlier.

- β0 represents the intercept of the model, or the energy or valence of the song when all the numerical
predictors equal to zero. Similarly, since it does not make any sense for a song to have 0 seconds in
length, the intercept is irrelevant under this context.
- β1 represent the slope of rock, xrock. This is the only difference in the predictor variables between the
logistic model and the linear models. Rock is an indicator variable denoting if the song is classified
as rock or not, where 1 represents rock and 0 represents pop. The coefficient represents the relative
difference in energy/valence when the song is classified as rock, compared to pop.
- β2 to β12 represent similar terms as to the β1 to β12 in the logistic model. The only difference is that
now they refer to the change in energy or valence, instead of the change in log odds.
- ϵ represents the error term, which is to allow for some deviations between the estimated energy or
valence to the actual values. Given the assumptions of linear regression, the error term is expected to
have a mean of zero and have the same spread for any given predictor combinations (STA302).

## Hypothesis Test
We have used hypothesis testing in two cases, first, check significance in predictors, and second, check the
balance assumption after propensity score matching.

By assuming that the regression assumptions are satisfied, we can interpret the p-values on the predictors,
which is calculated based on hypothesis tests. In this case, the null hypothesis suggests that the predictor is
not significant in affecting the response variable, such as being rock does not significantly affect the song’s
energy, and the alternative hypothesis suggests that there is a significant relationship between this predictor
and the response. At a 5% significance level, we have rejected the null hypothesis and concluded that there is
a significant impact when the calculated p-value is less than 0.05.

To check if the treatment group and control group have balanced characteristics for the observable variables
(the 12 audio features), we have utilized t-tests on the difference in means using the built-in t.test function in R.
This function computes the p-value based on the mean values in the two groups, the standard deviations, and
sample sizes. The null hypothesis for this test states that the means between the two genres are no-different,
which suggests balance. On the other hand, the alternative hypothesis suggests that the two groups have
statistically different means, which indicates a violation in the balance pattern that we wish to see. To
interpret the p-value, at a 5% significance level, we conclude that balance is achieved the then p-value is
greater than 0.05, and this is what we ideally want to observe.

## Assumptions
As stated earlier, fitting the regressions and trusting the hypothesis test results, requires certain assumptions
to hold. This mainly relies on the error terms to be uncorrelated, have a zero-conditional mean and constant
variable for any given predictor combinations, and are also normally distributed (STA302). In addition, the
songs should be randomly chosen and represent a variety of audio features (STA302). In this paper, we will
simply assume that these assumptions hold, which is a limitation.

Meanwhile, we will also check the balance assumption for the propensity score matching. Propensity
score matching is optimized when we have a “balance between the treatment and comparison group on
observable traits” (Week 9 slides). This means that for the matched songs, we want the two genres to have
similar audio features. Ultimately, we want rock music to be similar to pop music, and perhaps only differ in
the energy and valence, because it helps to mimic the randomization in an experimental design and “overcome
issues of selection bias that plague non-experimental methods” (The World Bank). By having a balance in
the audio features, we would get similar propensities between rock and pop music, making it easier to find
pairs. On the other hand, if we do not have a good balance, we may have to match rock and pop music that
are very different in terms of their audio features, which may directly affect the energy and valence.

# Results
To present the result of analysis, we will follow the four steps mentioned in the methodology of Propensity
Score matching.
## Logistic Regression Result
For the logistic regression model used to estimate the log odds of a song being rock, the full result is shown
in Figure A.1 in the appendix. Due to the large number of indicator variables for the categorical variables,
such as there are 56 indicators used to denote the year of release, we will only choose one subcategory to look
at for each categorical variable. The simplified result is shown below: For the logistic model:
![LogisResult](https://drive.google.com/uc?export=view&scale.option=fit&id=1qlHkC9TL19a0mruPslHwSal0g6_EpFoT)
where the hat above the coefficients denote that these are estimated values based on the our data, and p is
the probability of being rock. The estimated coefficients are shown in Table 1:
![Table1](https://drive.google.com/uc?export=view&scale.option=fit&id=1b_t8aq-OUi90epzwa2JHcDCB6K61DeZT)
Generally, the results of logistic regression suggests that besides the variables of instrumentalness and liveness,
which have positive influences on the probability for the track to be rock, all other audio features have
a negative impact on the probability. When we look at the two significant predictors as shown with the
apostrophes in Figure A.1, it is shown that when the danceability increases by 0.5, the log odds of the
probability for the track being rock decreases by -3.53. On the other hand, when the acousticness decreases
by 0.5, the log odds of the probability for the track being rock increases by 2.32. Recall that acousticness
represents the presence of electric instruments, such that lower acoustic indicates more electronically modified
audio. Hence, this coincides with the fact that rock music emphasizes on the use of electric instruments such
as electric guitar and electric bass.

## Matching Result
After obtaining the propensity scores, we utilized the matching function in the ARM package to pair tracks
(one rock with one pop) that have similar propensity of being rock. Some pairs are shown in Table 2:
![Table2](https://drive.google.com/uc?export=view&scale.option=fit&id=14_ndlgJfyxzE23B45nDatZdzN1m5JswU)
Through Table 2, we notice that in order to pair all the 222 rock tracks, which is the genre with a smaller
size, some rock tracks had to be paired to pop music that have completely different propensity scores. This
is because we ran out of pop music that has similar propensities, or used them in other matches. Figure 6
illustrates the distribution of the differences in propensity scores between all the matched pairs. It is obvious
that although many pairs have differences smaller than 0.25, more than 50% of the pairs have a difference
greater than 0.5. This indicates that although we are using the nearest neighbor approach to match the
observations, due to the differences in propensity scores between rock and pop music, many pairs were forced
to match, even when their propensity scores are not similar.
![Figure6](https://drive.google.com/uc?export=view&scale.option=fit&id=1VlHTXMJ42Um6iU5wTx5Dv9w01GSMxMwX)

## Matching Assumption
To explore why many pairs are not well matched, we will examine the balance in the audio features between
the two genres. In Table 3, the mean of each variable for each genre is listed, as well as the p-value from the
different in means hypothesis test.
![Table3](https://drive.google.com/uc?export=view&scale.option=fit&id=1LMyT8tDHvoFJPgHNlMHyX86MBU7Y4B9T)
Under a 5% significance level, Table 3 indicates that out of the 11 observable traits used to calculate the
propensity, 6 traits are showing significant differences between their means. In other words, for the 222 pop
music and 222 rock music in the matched data set, there are clear discrepancies in the danceability, loudness,
acousticness, instrumentalness, tempo, and duration between the two genres. This implies that we do not
have a good balance in the covariates and the randomization is not well mimicked.

## Linear Regression Result
Finally, we have utilized the matched data sets to fit the linear regressions to estimate the energy and valence
of a song. Similarly, we have simplied the model to only include one subcategory for a categorical variable.
The full models are shown in Figure A.2 and Figure A.3 in the appendix.

The first model to predict a song’s energy is shown below:
![Model1](https://drive.google.com/uc?export=view&scale.option=fit&id=1-1T0j6znhLBBPwYxbkjg8kw-_pvbpApA)
where the hat above the coefficients denote that these are estimated values based on the our data, and energy
is the value ranging from 0.0 to 1.0 representing the intensity of the song.

The estimated coefficients are shown in Table 4:
![Table4](https://drive.google.com/uc?export=view&scale.option=fit&id=1SlfKqCblvE4kNW8wxk8oA_Idg-jchhDQ)
The second model to predict a song’s valence is shown below:
![Model2](https://drive.google.com/uc?export=view&scale.option=fit&id=1aC-iLXsuYV6gMrMG4esMyMsuggUDXzID)
where the hat above the coefficients denote that these are estimated values based on our data, and valence is
the value ranging from 0.0 to 1.0 representing the positiveness of the song.
The result indicates that rock music has significantly greater values for energy, where compared to pop music
and holding everything else constant, being classified as rock music leads to an average 0.14 increase in
the energy level. This corresponds to our initial hypothesis, supporting the claim that rock music is more
energetic than pop music. Besides being rock, we also notice that the audio features of loudness, speechiness,
acousticness, and tempo also show significant influence on the energy of a song, under a 5% significance level,
as shown in Figure A.2 column 1.
![Table5](https://drive.google.com/uc?export=view&scale.option=fit&id=1Kmg_7heTLPL7N4vvfISB_Q792V_iXmBw)
In addition to energy, we also notice that rock music has significantly greater values for valence or positiveness.
Compared to pop music, and holding everything else constant, rock music is on average showing 0.25 points
higher for valence, which also supports our hypothesis for the fact that pop music is more negative compared
to rock. In addition, the audio features of danceability and loudness are also showing statistical significance
in affecting the song’s valence.

All analysis for this report was programmed using R version 4.1.2. Specifically, the tidyverse package was
used for data transformation, data cleaning, and figure plotting. The plyr package and the ddply() function
was used to construct summary data. The openintro package was used to construct the header of the paper.
The usethis package was used to setup the R environment to hide the Spotify Client ID and Secret ID.
The kable and kableExtra packages were used for making tables, as well as the markdown language. The

glm() function was used to fit the logistic regression models, and the lm() function was used to fit the linear
regressions. The broom package and tidy function are used to extract the results of regression models and
store them in tibbles. The arm package and the matching() function was used to perform the propensity
score matching technique using the nearest neighbor approach.

# Conclusion
## Summary
Throughout this paper, we have focused on the research question of how energy and valence differ between the
music genres of rock and music. Initially, we hypothesized with background research that rock music should
be more energetic and more positive, hence, have higher values for energy and valence. The hypothesis was
based on common definitions of rock music, pop music, public opinions on the two genres as well as existing
research. We collected song data from the music streaming platform, Spotify, which are representative tracks
for the two genres, and utilized the statistical method of Propensity Score Matching to analyse the impact of
being rock music on energy and valence.

Propensity Score Matching required us to first apply the logistic regression, in which we used 11 audio features
to estimate the propensity or the probability, for a song track to be rock. Results of logistic regression indicate
that the audio features of danceability, loudness, acoustics, instrumentalness, and duration are significant in
affecting the probability for a song to have the genre of rock.

Then, we used the nearest neighborhood matching method, as implemented in the matching function in the
ARM package, to match rock and pop music pairs that have a similar propensity of being rock music. By
checking the balance of audio features between the two groups, we identify that the two genres are quite
different in terms of audio features such as danceability and loudness. This may hinder the performance of
propensity score matching, a limitation in this study.

Finally, we used the data set that included all the matched pairs to estimate the energy and valence of a
track based on the 11 audio features and its actual genre. The results supported the hypothesis such that
compared to pop music and holding everything else constant, rock music is on average having 0.14 higher
energy and 0.25 higher valence, which is more energetic and more positive than pop music.

## Elaborating on the Results
To elaborate on the final result from the two linear regression models, we will also interpret the significant
audio features that affect energy or valence. In the model used to estimate energy, loudness is shown to have
a positive influence on energy, which means that a 1 decibel increase in loudness will lead to a roughly 0.04
increase in energy. As mentioned in the introduction, it is commonly perceived that rock music is louder
than other genres. This indicates a positive correlation between rock and loudness, which justifies why both
predictors show a positive influence on energy. The same principle applies to acousticness, such that because
rock music emphasizes the use of electrical instruments, they are less acoustic and have a negative correlation
with acousticness. Therefore, we observe the negative impact of acousticness on energy.

On the other hand, the second model estimating valence showed less significant predictors, with only rock,
danceability, and loudness being statistically significant in impacting the positiveness of the music. All three
variables have positive influences on valence, especially for danceability, the coefficient is 0.85, which means
that when the danceability increases by 0.5, and holding everything else constant, then valence will increase
by 0.425. This is quite a large impact as valence only takes on values from 0.0 to 1.0. In fact, this should
be easily justified as dancing is an indication of happiness or joyfulness. In addition, when we think about
the music that people dance to, they tend to be louder in order to encourage physical movements from the
listeners. This is reflected by the positive influence that loudness has on valence.

## Weaknesses
In this paper, we have made several assumptions regarding our data and methodology. Although it simplified
the process, there could be flaws in the results due to biasedness and violations.

In our data collection process, we relied on the fact that the playlists chosen from Spotify are representative
of the corresponding genres. For instance, the rock songs collected should represent the rock genre sufficiently
by including songs of smaller subgenres of rock, like heavy metal or Christian rock. If the songs are not
representative, but rather biased towards a certain type of rock or pop, then the data set would fail to depict
the actual traits of rock and pop music, making our analysis biased and inaccurate. This could easily be
the case because Spotify, as a for-profit company, could make playlists to make the users keep listening, or
purposefully include artists that they have exclusive copyrights.

During the analysis process, we have identified an imbalance between rock music and pop music in our
matched data set. This may lead to poor utilization of Propensity Score Matching as we are not able to
create or mimic a randomized experiment, by having songs with random audio features be randomly assigned
to be rock or pop. However, this is difficult to overcome under this context because the two genres will always
be different in terms of the audio features, not only for the energy and valence.

In addition, we did not explicitly check model assumptions for the logistic regression and linear regressions. If
assumptions are violated, then it may lead to biased and inaccurate estimated coefficients and significance. If
the result of the logistic regression is inaccurate, thus, the estimated propensities are biased, then it implies
that the matching process will also be inaccurate since the propensities are incorrect. This can lead to the
entire analysis being inaccurate.

## Next Steps
In future researches, we recommend targeting the above-mentioned weakness and overcoming them. For the
data collection process, we recommend a more rigorous selection process to make the data more representative
of the genre. This can be achieved in several ways, such as expanding the sample size, removing controversial
tracks, or considering rock music in other languages. By improving the data set, perhaps it will also yield a
more balanced and random distribution in the audio features for the two genres.

Future research should also pay more attention to the model diagnostics for the regression models. When
assumptions are violated, then the model should be fixed and improved by methodologies such as transforming
variables and changing predictors (STA302). To obtain more trustworthy propensity score matching results,
the logistic regression model used in step one should be optimized before moving further. Lastly, the two
linear regression models should also have the assumptions checked to make sure the results are accurate.

## Remarks
In conclusion, this paper found that rock music is generally more energetic and positive than pop music, using
the statistical method of propensity score matching and its supplementary tools. Future researches should
focus on improving the data collection process and model diagnostics to improve the validity of the research.

# Bibliography & Appendix
Please refer to the file [here](https://drive.google.com/file/d/16JVzyNp70vmo6OxAoJxk-Sz5AAs-sSgo/view?usp=drive_link)

























