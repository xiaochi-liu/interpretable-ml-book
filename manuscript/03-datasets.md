


# Datasets {#data}
Throughout the book all the models and techniques will be applied on real datasets, which are freely available online.
We will be using different datasets for different tasks:
classification, regression and text classification.

## Bike Sharing Counts (Regression) {#bike-data}
This dataset contains daily counts of bike rentals from bike sharing company [Capital-Bikeshare](https://www.capitalbikeshare.com/) in Washington D.C., along with weather and seasonal information.
The data was kindly open sourced by Capital-Bikeshare and the folks from @bike2013 have added the weather data and the seasonal information.
The goal is to predict how many rental bikes will be out on the street given weather and day. The data can be downloaded from the [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset).


For the examples, new features were introduced and not all original features were used.
Here is the list of features that were used:

- season : spring (1), summer (2), autumn (3), winter (4).
- holiday : Binary feature indicating if the day was a holiday (1) or not (0).
- yr: The year (2011 or 2012).
- days_since_2011: Number of days since the 01.01.2011 (the first day in the dataset). This feature was introduced to account for the trend, in this case that the bike rental service became more popular over time.
- workingday : Binary feature indicating if the day was a workingday (1) or weekend / holiday (0).
- weathersit : The weather situation on that day
    - Clear, Few clouds, Partly cloudy, Cloudy
    - Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist
    - Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds
    - Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog
- temp : Temperature in degrees Celsius.
- hum: Relative humidity in percent (0 to 100).
- windspeed: Wind speed in km per hour.
- cnt: Count of total rental bikes including both casual and registered. The count was used as the target in the regression tasks.

You can look at a sample of days here:


|    |season |yr   |mnth |holiday    |weekday |workingday  |weathersit |      temp|     hum| windspeed|  cnt| days_since_2011|
|:---|:------|:----|:----|:----------|:-------|:-----------|:----------|---------:|-------:|---------:|----:|---------------:|
|669 |WINTER |2012 |OKT  |NO HOLIDAY |TUE     |WORKING DAY |MISTY      |  6.954554| 82.5455| 14.271603| 1096|             668|
|685 |WINTER |2012 |NOV  |NO HOLIDAY |THU     |WORKING DAY |MISTY      |  7.118349| 62.0417| 10.250129| 5445|             684|
|209 |FALL   |2011 |JUL  |NO HOLIDAY |THU     |WORKING DAY |GOOD       | 28.620849| 58.3333| 11.958093| 4390|             208|
|605 |FALL   |2012 |AUG  |NO HOLIDAY |MON     |WORKING DAY |GOOD       | 25.056651| 73.0417|  8.625111| 6917|             604|
|467 |SUMMER |2012 |APR  |NO HOLIDAY |WED     |WORKING DAY |GOOD       |  8.388712| 46.9565| 19.783358| 4862|             466|
|377 |SPRING |2012 |JAN  |NO HOLIDAY |THU     |WORKING DAY |MISTY      |  9.977500| 80.2917| 12.124789| 4097|             376|
|535 |SUMMER |2012 |JUN  |NO HOLIDAY |MON     |WORKING DAY |MISTY      | 18.711651| 77.7917| 11.707982| 5099|             534|
|98  |SUMMER |2011 |APR  |NO HOLIDAY |FRI     |WORKING DAY |MISTY      |  7.784151| 83.6250| 15.208464| 1471|              97|
|476 |SUMMER |2012 |APR  |NO HOLIDAY |FRI     |WORKING DAY |GOOD       | 16.753349| 69.4583| 10.041357| 7290|             475|
|510 |SUMMER |2012 |MAY  |NO HOLIDAY |THU     |WORKING DAY |GOOD       | 22.785000| 71.6667| 11.584032| 6770|             509|

## YouTube Spam Comments (Text Classification) {#spam-data}
As an example for text classification we will be using 1956 comments from 5 different YouTube videos.
Thankfully the authors that used this dataset in an article about spam classification made the data  [freely available](http://dcomp.sor.ufscar.br/talmeida/youtubespamcollection/) [@alberto2015tubespam].

The comments were  collected through the YouTube API from five of the ten most viewed videos
on YouTube in the first half of 2015. All of the 5 videos are music videos.
One of them is "Gangnam Style" from Korean artist Psy. The other artists were Katy Perry, LMFAO, Eminem, and Shakira.


You can flip through some of the comments. The comments had been hand labeled as spam or legitimate.
Spam has been coded with a '1' and legitimate comments with a '0'.


|CONTENT                                                                                                                                                                | CLASS|
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----:|
|Huh, anyway check out this you[tube] channel: kobyoshi02                                                                                                               |     1|
|Hey guys check out my new channel and our first vid THIS IS US THE  MONKEYS!!! I'm the monkey in the white shirt,please leave a like comment  and please subscribe!!!! |     1|
|just for test I have to say murdev.com                                                                                                                                 |     1|
|me shaking my sexy ass on my channel enjoy ^_^                                                                                                                         |     1|
|watch?v=vtaRGgvGtWQ   Check this out .                                                                                                                                 |     1|
|Hey, check out my new website!! This site is about kids stuff. kidsmediausa  . com                                                                                     |     1|
|Subscribe to my channel                                                                                                                                                |     1|
|i turned it on mute as soon is i came on i just wanted to check the  views...                                                                                          |     0|
|You should check my channel for Funny VIDEOS!!                                                                                                                         |     1|
|and u should.d check my channel and tell me what I should do next!                                                                                                     |     1|

You can also go over to YouTube and have a look at the comment section.
But please don't get trapped in the YouTube hell, ending up watching videos about monkeys stealing and drinking cocktails from tourists on the beach.
Also the Google Spam detector probably has changed a lot since 2015.

[Watch the view-record breaking video "Gangnam Style" here](https://www.youtube.com/watch?v=9bZkp7q19f0&feature=player_embedded)

## Risk Factors for Cervical Cancer (Classification) {#cervical}

The cervical cancer dataset contains indicators and risk factors for predicting if a woman will get cervical cancer.
The features contain demographics (e.g. age), habits, and medical history.
The data can be downloaded from the [UCI Machine Learning repository](https://archive.ics.uci.edu/ml/datasets/Cervical+cancer+%28Risk+Factors%29) and is described by @fernandes2017transfer.

The subset of features, which are used in this book are:

- (int) Age
- (int) Number of sexual partners
- (int) First sexual intercourse (age)
- (int) Num of pregnancies
- (bool) Smokes yes (1) or no (1)
- (int) Smokes (years)
- (bool) Hormonal Contraceptives yes (1) or no (0)
- (int) Hormonal Contraceptives (years)
- (bool) IUD: Intrauterine device yes (1) or no (1)
- (int) IUD (years): Number of years with an intrauterine device
- (bool) STDs: Ever had a sexually transmitted disease? Yes (1) or no (0)
- (int) STDs (number): Number of sexually transmitted diseases.
- (int) STDs: Number of diagnosis
- (int) STDs: Time since first diagnosis
- (int) STDs: Time since last diagnosis
- (bool) Biopsy: Biopsy results "Healthy" or "Cancer". Target outcome.

As the biopsy serves as the gold standard for diagnosing cervical cancer, the classification task in this book used the biopsy outcome as the target.
Missing values for each column were imputed by the mode (most frequent value), which is probably a bad solution, because the value of the answer might be correlated with the probability for a value being missing.
There is probably a bias, because the questions are of a very private nature.
But this is not a book about missing data imputation, so the mode imputation will suffice!


| Age| Number.of.sexual.partners| First.sexual.intercourse| Num.of.pregnancies| Smokes| Smokes..years.| Hormonal.Contraceptives| Hormonal.Contraceptives..years.| IUD| IUD..years.| STDs| STDs..number.| STDs..Number.of.diagnosis| STDs..Time.since.first.diagnosis| STDs..Time.since.last.diagnosis|Biopsy  |
|---:|-------------------------:|------------------------:|------------------:|------:|--------------:|-----------------------:|-------------------------------:|---:|-----------:|----:|-------------:|-------------------------:|--------------------------------:|-------------------------------:|:-------|
|  18|                         4|                       15|                  1|      0|       0.000000|                       0|                               0|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  15|                         1|                       14|                  1|      0|       0.000000|                       0|                               0|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  34|                         1|                       15|                  1|      0|       0.000000|                       0|                               0|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  52|                         5|                       16|                  4|      1|      37.000000|                       1|                               3|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  46|                         3|                       21|                  4|      0|       0.000000|                       1|                              15|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  42|                         3|                       23|                  2|      0|       0.000000|                       0|                               0|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  51|                         3|                       17|                  6|      1|      34.000000|                       0|                               0|   1|           7|    0|             0|                         0|                                1|                               1|Cancer  |
|  26|                         1|                       26|                  3|      0|       0.000000|                       1|                               2|   1|           7|    0|             0|                         0|                                1|                               1|Healthy |
|  45|                         1|                       20|                  5|      0|       0.000000|                       0|                               0|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
|  44|                         3|                       15|                  1|      1|       1.266973|                       0|                               0|   0|           0|    0|             0|                         0|                                1|                               1|Healthy |
