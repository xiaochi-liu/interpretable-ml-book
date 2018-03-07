    devtools::load_all("../")

    ## Loading iml.book

    ## Loading required package: knitr

    ## Warning: package 'knitr' was built under R version 3.4.3

    ## Warning: replacing previous import 'BBmisc::isFALSE' by
    ## 'backports::isFALSE' when loading 'mlr'

    ## Warning in as.POSIXlt.POSIXct(Sys.time()): unknown timezone 'zone/tz/2018c.
    ## 1.0/zoneinfo/Europe/Berlin'

Datasets
========

Throughout the book all the models and techniques will be applied on
real datasets, which are freely available online. We will be using
different datasets for different tasks: classification, regression and
text classification.

Bike Sharing Counts (Regression)
--------------------------------

This dataset contains daily counts of bike rentals from bike sharing
company [Capital-Bikeshare](https://www.capitalbikeshare.com/) in
Washington D.C., along with weather and seasonal information. The data
was kindly open sourced by Capital-Bikeshare and the folks from
@bike2013 have added the weather data and the seasonal information. The
goal is to predict how many rental bikes will be out on the street given
weather and day. The data can be downloaded from the [UCI Machine
Learning
Repository](http://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset).

For the examples, new features were introduced and not all original
features were used. Here is the list of features that were used:

-   season : spring (1), summer (2), autumn (3), winter (4).
-   holiday : Binary feature indicating if the day was a holiday (1) or
    not (0).
-   yr: The year (2011 or 2012).
-   days\_since\_2011: Number of days since the 01.01.2011 (the first
    day in the dataset). This feature was introduced to account for the
    trend, in this case that the bike rental service became more popular
    over time.
-   workingday : Binary feature indicating if the day was a
    workingday (1) or weekend / holiday (0).
-   weathersit : The weather situation on that day
    -   Clear, Few clouds, Partly cloudy, Cloudy
    -   Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist
    -   Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light
        Rain + Scattered clouds
    -   Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog
-   temp : Temperature in degrees Celsius.
-   hum: Relative humidity in percent (0 to 100).
-   windspeed: Wind speed in km per hour.
-   cnt: Count of total rental bikes including both casual and
    registered. The count was used as the target in the regression
    tasks.

You can look at a sample of days here:

    set.seed(42)
    data("bike")
    bike.display = bike[sample(1:nrow(bike), size=50), ]
    knitr::kable(bike.display[1:10,])

<table>
<thead>
<tr class="header">
<th></th>
<th align="left">season</th>
<th align="left">yr</th>
<th align="left">mnth</th>
<th align="left">holiday</th>
<th align="left">weekday</th>
<th align="left">workingday</th>
<th align="left">weathersit</th>
<th align="right">temp</th>
<th align="right">hum</th>
<th align="right">windspeed</th>
<th align="right">cnt</th>
<th align="right">days_since_2011</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>669</td>
<td align="left">WINTER</td>
<td align="left">2012</td>
<td align="left">OKT</td>
<td align="left">NO HOLIDAY</td>
<td align="left">TUE</td>
<td align="left">WORKING DAY</td>
<td align="left">MISTY</td>
<td align="right">6.954554</td>
<td align="right">82.5455</td>
<td align="right">14.271603</td>
<td align="right">1096</td>
<td align="right">668</td>
</tr>
<tr class="even">
<td>685</td>
<td align="left">WINTER</td>
<td align="left">2012</td>
<td align="left">NOV</td>
<td align="left">NO HOLIDAY</td>
<td align="left">THU</td>
<td align="left">WORKING DAY</td>
<td align="left">MISTY</td>
<td align="right">7.118349</td>
<td align="right">62.0417</td>
<td align="right">10.250129</td>
<td align="right">5445</td>
<td align="right">684</td>
</tr>
<tr class="odd">
<td>209</td>
<td align="left">FALL</td>
<td align="left">2011</td>
<td align="left">JUL</td>
<td align="left">NO HOLIDAY</td>
<td align="left">THU</td>
<td align="left">WORKING DAY</td>
<td align="left">GOOD</td>
<td align="right">28.620849</td>
<td align="right">58.3333</td>
<td align="right">11.958093</td>
<td align="right">4390</td>
<td align="right">208</td>
</tr>
<tr class="even">
<td>605</td>
<td align="left">FALL</td>
<td align="left">2012</td>
<td align="left">AUG</td>
<td align="left">NO HOLIDAY</td>
<td align="left">MON</td>
<td align="left">WORKING DAY</td>
<td align="left">GOOD</td>
<td align="right">25.056651</td>
<td align="right">73.0417</td>
<td align="right">8.625111</td>
<td align="right">6917</td>
<td align="right">604</td>
</tr>
<tr class="odd">
<td>467</td>
<td align="left">SUMMER</td>
<td align="left">2012</td>
<td align="left">APR</td>
<td align="left">NO HOLIDAY</td>
<td align="left">WED</td>
<td align="left">WORKING DAY</td>
<td align="left">GOOD</td>
<td align="right">8.388712</td>
<td align="right">46.9565</td>
<td align="right">19.783358</td>
<td align="right">4862</td>
<td align="right">466</td>
</tr>
<tr class="even">
<td>377</td>
<td align="left">SPRING</td>
<td align="left">2012</td>
<td align="left">JAN</td>
<td align="left">NO HOLIDAY</td>
<td align="left">THU</td>
<td align="left">WORKING DAY</td>
<td align="left">MISTY</td>
<td align="right">9.977500</td>
<td align="right">80.2917</td>
<td align="right">12.124789</td>
<td align="right">4097</td>
<td align="right">376</td>
</tr>
<tr class="odd">
<td>535</td>
<td align="left">SUMMER</td>
<td align="left">2012</td>
<td align="left">JUN</td>
<td align="left">NO HOLIDAY</td>
<td align="left">MON</td>
<td align="left">WORKING DAY</td>
<td align="left">MISTY</td>
<td align="right">18.711651</td>
<td align="right">77.7917</td>
<td align="right">11.707982</td>
<td align="right">5099</td>
<td align="right">534</td>
</tr>
<tr class="even">
<td>98</td>
<td align="left">SUMMER</td>
<td align="left">2011</td>
<td align="left">APR</td>
<td align="left">NO HOLIDAY</td>
<td align="left">FRI</td>
<td align="left">WORKING DAY</td>
<td align="left">MISTY</td>
<td align="right">7.784151</td>
<td align="right">83.6250</td>
<td align="right">15.208464</td>
<td align="right">1471</td>
<td align="right">97</td>
</tr>
<tr class="odd">
<td>476</td>
<td align="left">SUMMER</td>
<td align="left">2012</td>
<td align="left">APR</td>
<td align="left">NO HOLIDAY</td>
<td align="left">FRI</td>
<td align="left">WORKING DAY</td>
<td align="left">GOOD</td>
<td align="right">16.753349</td>
<td align="right">69.4583</td>
<td align="right">10.041357</td>
<td align="right">7290</td>
<td align="right">475</td>
</tr>
<tr class="even">
<td>510</td>
<td align="left">SUMMER</td>
<td align="left">2012</td>
<td align="left">MAY</td>
<td align="left">NO HOLIDAY</td>
<td align="left">THU</td>
<td align="left">WORKING DAY</td>
<td align="left">GOOD</td>
<td align="right">22.785000</td>
<td align="right">71.6667</td>
<td align="right">11.584032</td>
<td align="right">6770</td>
<td align="right">509</td>
</tr>
</tbody>
</table>

YouTube Spam Comments (Text Classification)
-------------------------------------------

As an example for text classification we will be using 1956 comments
from 5 different YouTube videos. Thankfully the authors that used this
dataset in an article about spam classification made the data [freely
available](http://dcomp.sor.ufscar.br/talmeida/youtubespamcollection/)
\[@alberto2015tubespam\].

The comments were collected through the YouTube API from five of the ten
most viewed videos on YouTube in the first half of 2015. All of the 5
videos are music videos. One of them is "Gangnam Style" from Korean
artist Psy. The other artists were Katy Perry, LMFAO, Eminem, and
Shakira.

You can flip through some of the comments. The comments had been hand
labeled as spam or legitimate. Spam has been coded with a '1' and
legitimate comments with a '0'.

    data(ycomments)
    knitr::kable(ycomments[1:10, c('CONTENT', 'CLASS')])

<table>
<thead>
<tr class="header">
<th align="left">CONTENT</th>
<th align="right">CLASS</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Huh, anyway check out this you[tube] channel: kobyoshi02</td>
<td align="right">1</td>
</tr>
<tr class="even">
<td align="left">Hey guys check out my new channel and our first vid THIS IS US THE MONKEYS!!! I'm the monkey in the white shirt,please leave a like comment and please subscribe!!!!</td>
<td align="right">1</td>
</tr>
<tr class="odd">
<td align="left">just for test I have to say murdev.com</td>
<td align="right">1</td>
</tr>
<tr class="even">
<td align="left">me shaking my sexy ass on my channel enjoy ^_^</td>
<td align="right">1</td>
</tr>
<tr class="odd">
<td align="left">watch?v=vtaRGgvGtWQ Check this out .</td>
<td align="right">1</td>
</tr>
<tr class="even">
<td align="left">Hey, check out my new website!! This site is about kids stuff. kidsmediausa . com</td>
<td align="right">1</td>
</tr>
<tr class="odd">
<td align="left">Subscribe to my channel</td>
<td align="right">1</td>
</tr>
<tr class="even">
<td align="left">i turned it on mute as soon is i came on i just wanted to check the views...</td>
<td align="right">0</td>
</tr>
<tr class="odd">
<td align="left">You should check my channel for Funny VIDEOS!!</td>
<td align="right">1</td>
</tr>
<tr class="even">
<td align="left">and u should.d check my channel and tell me what I should do next!</td>
<td align="right">1</td>
</tr>
</tbody>
</table>

You can also go over to YouTube and have a look at the comment section.
But please don't get trapped in the YouTube hell, ending up watching
videos about monkeys stealing and drinking cocktails from tourists on
the beach. Also the Google Spam detector probably has changed a lot
since 2015. Watch the view-record breaking video "Gangnam Style" below:

<iframe width="560" height="315" src="https://www.youtube.com/embed/9bZkp7q19f0" frameborder="0" allowfullscreen>
</iframe>
Risk Factors for Cervical Cancer (Classification)
-------------------------------------------------

The cervical cancer dataset contains indicators and risk factors for
predicting if a woman will get cervical cancer. The features contain
demographics (e.g. age), habits, and medical history. The data can be
downloaded from the [UCI Machine Learning
repository](https://archive.ics.uci.edu/ml/datasets/Cervical+cancer+%28Risk+Factors%29)
and is described by @fernandes2017transfer.

The subset of features, which are used in this book are:

-   (int) Age
-   (int) Number of sexual partners
-   (int) First sexual intercourse (age)
-   (int) Num of pregnancies
-   (bool) Smokes yes (1) or no (1)
-   (int) Smokes (years)
-   (bool) Hormonal Contraceptives yes (1) or no (0)
-   (int) Hormonal Contraceptives (years)
-   (bool) IUD: Intrauterine device yes (1) or no (1)
-   (int) IUD (years): Number of years with an intrauterine device
-   (bool) STDs: Ever had a sexually transmitted disease? Yes (1) or
    no (0)
-   (int) STDs (number): Number of sexually transmitted diseases.
-   (int) STDs: Number of diagnosis
-   (int) STDs: Time since first diagnosis
-   (int) STDs: Time since last diagnosis
-   (bool) Biopsy: Biopsy results "Healthy" or "Cancer". Target outcome.

As the biopsy serves as the gold standard for diagnosing cervical
cancer, the classification task in this book used the biopsy outcome as
the target. Missing values for each column were imputed by the mode
(most frequent value), which is probably a bad solution, because the
value of the answer might be correlated with the probability for a value
being missing. There is probably a bias, because the questions are of a
very private nature. But this is not a book about missing data
imputation, so the mode imputation will suffice!

    data("cervical")
    knitr::kable(cervical[1:10, ])

<table>
<thead>
<tr class="header">
<th align="right">Age</th>
<th align="right">Number.of.sexual.partners</th>
<th align="right">First.sexual.intercourse</th>
<th align="right">Num.of.pregnancies</th>
<th align="right">Smokes</th>
<th align="right">Smokes..years.</th>
<th align="right">Hormonal.Contraceptives</th>
<th align="right">Hormonal.Contraceptives..years.</th>
<th align="right">IUD</th>
<th align="right">IUD..years.</th>
<th align="right">STDs</th>
<th align="right">STDs..number.</th>
<th align="right">STDs..Number.of.diagnosis</th>
<th align="right">STDs..Time.since.first.diagnosis</th>
<th align="right">STDs..Time.since.last.diagnosis</th>
<th align="left">Biopsy</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="right">18</td>
<td align="right">4</td>
<td align="right">15</td>
<td align="right">1</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="even">
<td align="right">15</td>
<td align="right">1</td>
<td align="right">14</td>
<td align="right">1</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="odd">
<td align="right">34</td>
<td align="right">1</td>
<td align="right">15</td>
<td align="right">1</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="even">
<td align="right">52</td>
<td align="right">5</td>
<td align="right">16</td>
<td align="right">4</td>
<td align="right">1</td>
<td align="right">37.000000</td>
<td align="right">1</td>
<td align="right">3</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="odd">
<td align="right">46</td>
<td align="right">3</td>
<td align="right">21</td>
<td align="right">4</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">1</td>
<td align="right">15</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="even">
<td align="right">42</td>
<td align="right">3</td>
<td align="right">23</td>
<td align="right">2</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="odd">
<td align="right">51</td>
<td align="right">3</td>
<td align="right">17</td>
<td align="right">6</td>
<td align="right">1</td>
<td align="right">34.000000</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">7</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Cancer</td>
</tr>
<tr class="even">
<td align="right">26</td>
<td align="right">1</td>
<td align="right">26</td>
<td align="right">3</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">1</td>
<td align="right">2</td>
<td align="right">1</td>
<td align="right">7</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="odd">
<td align="right">45</td>
<td align="right">1</td>
<td align="right">20</td>
<td align="right">5</td>
<td align="right">0</td>
<td align="right">0.000000</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
<tr class="even">
<td align="right">44</td>
<td align="right">3</td>
<td align="right">15</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="right">1.266973</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">0</td>
<td align="right">1</td>
<td align="right">1</td>
<td align="left">Healthy</td>
</tr>
</tbody>
</table>
