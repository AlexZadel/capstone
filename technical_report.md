# Technical Report
October 19, 2018

## Executive Summary


The goal of this project is to build a model to predict Senate election results based on economic data, census surveys, and prior election outcomes from every year since 1976.

- The data for this project was obtained from public data sets and government soruces. For a full explanation of the source data and compiled data set, please see the [data dictionary](https://github.com/AlexZadel/dsi_capstone/blob/master/data_dictionary.md).

- The final models predict with roughly 90% accuracy on testing data. It was then used to predict the outcomes of the 2018 Senate elections.

- The models are designed to be politically unbiased in the sense they do not value one predicted outcome over another. In this sense, the value of the model lies in correct predictions. Therefore this model was calibrated to maximize accuracy rather over another metric.

- This model shows promise, but there are still many ways it could be improved given sufficient time and data. For example, more economic indicators at the state and national level would give a fuller picture of the economic backdrop. One key limitation was a lack of *historical* campaign finance data in a practical format. Given time or more analysts, I can see a finance dimension to the model being a powerful addition.

- The model was also designed with the specific intention of being general enough to be applied to House of Representatives and gubernatorial campaigns. The concluding section of this report goes into deeper detail on these possibilities.
----

## Introduction

<p>On November 6, 2018, people will vote across the United States for the next set of politicians to represent them at the state and federal levels. In 33 states, that includes in a candidate for the United State Senate. As the upper chamber of the legislative branch, the Senate holds key powers such as confirming the Executive Cabinet, nominating and confirming federal judges, and passing passing legislation. In the current polarized atmosphere, these elections have attracted more public attention than ever before.</p>

<p> Given these high stakes, politicians and the news media have placed a high premium on being able to project the outcomes of these elections, whether it be to attract voters or viewers. Political pollsters have been a key feature in politics for decades and academics have tried to build models to capture the complexity of elections to uncover trends and predict future results. *In this project, I built one of these models using machinee learning tools and economic, political, and demographic data.*</p>

<p> For this project, I limited my scope to projecting outcomes in the Senate, but the the same model -- with some minor alterations -- could be used to predict other statewide elections as well. Further data extraction and processing would be necessary to apply the model to different geographies, such as the U.S. House of Representatives, but the principles and process of this analysis would be similar. </p>

#### An Academic Look at Politics
This project is intended as an academic analysis of election trends rather than a tool to direct campaign strategy for one party or another. Along those lines, I have done my best to minimize any political bias in creation of this model. That being said, the nature of modeling means making decisions to orient the model in one way or another for the sake of analysis. In particular, I have designed the classification to be a boolean outcome on whether the GOP candidate in the race wins the election. This decision was made by a coin flip rather than due to any mathematical or political motivation. In the future, I hope to update the model with a randomization tool to make the actual modeling process party-blind.

#### Report Outline
<p> This report details the process of building this model.  First, I will review the data sources and processing that was done to build the primary dataset used in the model. Next I will review the exploratory data analysis I conducted on this data and any issues that I found or alterations that I made based on what I found in that analysis. Then I will describe my modeling process and how I selected and tuned models before deciding on a final model to use for the 2018 projections. Finally, I will review the results of my modelling, particularly how the model performed based on certain metrics and any patterns in the races it calls incorrectly. In that section, I will also look at the model's predictions for 2018.</p>

---

## Data
<p> With that understanding of political modeling under our belts, we can now examine the technical details of this project. As always, that process begins with data. All data citations and a complete data dictionary can be found in this [data dictionary.](https://github.com/AlexZadel/dsi_capstone/blob/master/data_dictionary.md)  All data scraping and most data cleaning was done in Python, though Microsoft Excel was used for certain format changes due to relative ease.</p>

#### Unit of Analysis
<p> For this project, I saw myself as  having two possible units of analysis: a single candidate (and therefore multiple entries per contest) or a single election (between two or more candidates). I chose to use a single election as the unit of analysis for a few reasons. </p>

<p> Firstly, having multiple entries in the same race presents some potenital issues with multicollinearity since any economic or demographic data for opposing candidates would be identical except for political party. While the relationship between party, incumbency, and economic trends certainly leads to a "don't rock the boat" mentality in good economic times -- or the inverse in bad times -- I worried about the multiple candidate phenomenon under-valuing the role of those matching variables.</p>

<p> Secondly, using candidates as the unit of analysis overlooks the dynamic relationship between any candidates in a race. Elections are not strictly zero-sum since turnout is a key element, but the fact is that any vote for a candidate is one against the other. Obviously candidates in the same election are not indepdent of one another. Separating candidates would in a way imply that they alone and their profile (and context) drive the number of votes they get or whether they win or lose a race. But that is simply not true. Elections are a comparison and to accurately represent that dynamic, candidates should be considered in tandem as part of a single unit: the election. </p>

<p> Lastly, treating the question as a classification as opposed to a regression was a key factor in this decision as well. As a classification, it made more sense to classify a win or loss (as mentioned, oriented to the GOP as a win or loss) rather than a win or a loss for a particular candidate -- again due to multicollinearity concerns. </p>

Having established what structure we want for the data, we can move to the discussion of how data was gathered and from what sources.

#### Data Gathering and Cleaning

###### Election Data
<p> The first priority in terms of data gathering is election results themselves. Not only will these outcomes be the targets of our model, but given the importance of incumbency in elections, they are a valuable source of feature data as well. Since these results are public information, we can feasibly gather all Senate election results in U.S. history. (We will see later why 1976 was chosen as our start day)

<p> Surprisingly there is no (publicly-available) data set that gives quite the information I wanted for this analysis, so we had to find the outcomes another way. Fortunately, however, there are detailed Wikipedia pages on every federal and gubernatorial election. These pages thankfully include summary tables for each senate race, including the names of candidates, their party affiliation, incumbent status, and results (usually in percentages). </p>

<p> To extract this data I used several scraping functions that can be found in the [scraper notebook.](https://github.com/AlexZadel/dsi_capstone/blob/master/notebooks/1__scraper.ipynb)  As you will see, there are several different scraping functions. Given the crowd-sourced workings of Wikipedia, some pages had different table formats and therefore different scrapers had to be used for some pages. You will also see extraction functions for gubernatorial and House of Representatives elections. This data was indeed scraped, but is absent from the repository as it was not used in the current scope of the project.</p>

<p> The next step with the election data was to clean it and transform it to a format that fit our modeling techniques. I began by doing some small edits in Excel, specifically to generate the entry ids.  Given that the data was scraped, it needed to be cleared of special characters, which was done using regular expressions in Python. The pertinent information also needed to be extracted from larger strings, such as the names of candidates, their party affiliations, and incumbency information. As you can see in the [Cleaning Notebook,](https://github.com/AlexZadel/dsi_capstone/blob/master/notebooks/2__cleaner.ipynb) I used several small functions with regular expressions to extract data and simply included them in my central cleaning functions.</p>

In the same notebook, I also did initial feature engineering on the election data. For example, I created all of the incumbency, party, and win/loss binary features. Again refer to the [data dictionary](https://github.com/AlexZadel/dsi_capstone/blob/master/data_dictionary.md) for a complete list. The elections data was then exported as a clean CSV file.

###### Census Data
<p> The next set of data that I gathered was census data in order to incorporate demographic effects in our model. My goal was to have a set of demographic variables for each state for each election year in order to incorporate what is essentially a demographic snapshot into each campaign vector. </p>

<p> While the American Community Survey has been a source for annual data since 2005, I did not want to limit our study to such a short period of time. Instead I relied on dicennial census data to provide demograhic information. While the census is severely [flawed, ](https://github.com/AlexZadel/dsi_capstone/blob/master/data_dictionary.md) it seems the best option to capture information as far back as the 1970s. I was able to use their [data portal](https://www.census.gov/geo/maps-data/) to obtain the data for the years and variables of interest.</p>

<p>  Fortunately census data is (usually) already broken down by year and geographic area, so the only cleaning necessary for this data set was minor formatting and reorienting to have it match the format needed to join the datasets. </p>

<p>Of course, census data is only a snapshot but for the years of the survey alone. To incorporate data for the years between census surveys, I used a simple linear projection for each decade, assuming the rate of demographic change between census years was perfectly linear. This was done for every decade separately -- i.e. 1970 and 1980 projected 1976 and 1978 while 1980 and 1990 were used for the entire 80s decade. While this assumption is obviously incorrect, I believe it approximates the demographic figures reasonably well. </p>

I chose three categories of demographics to incorporate into the model: age, race, and sex. Unfortunately, time did not allow for a full breakdown of these variables (such as the number of women under 30 by race). Instead I used totals of each and converted them into percentages of the total population to try both in modeling. I also bundled age categories. The census breaks them down in greater detail than I believed necessary. The data dictionary details the final ranges that were used.

###### Economic Data
Studies have shown that the health of the economy is a major factor in the success of the majority party in elections. When the economy is healthy, voters have faith in the government and are more likely to vote for the majority party to stay in power. When the economy is ailing, the majority party often faces backlash at the polls.

Given that trend, I wanted to be sure to incorporate some type of economic data into the model. Time and data limitations led me to limit the economic indicators to the national and state unemployment rates. That said, the unemployment rate, though economically complex, is a metric that voters understand on a basic level and is therefore helpful if our goal is to capture economic optimism rather than actual economic strength, which can be difficult to measure for economists, let alone everyday voters.

This data was obtained through the Bureau of Labor Statistics, which provides state data going back to 1976. This limitation inspired the time scope of the project, though it frankly is a good balance of providing a great deal of data but not going so far back in history to when elections took place on TV like have in the past few decades.

Fortunatley this data, like census data, is already cleaned and geographically partitioned, so the only cleaning/transformation of this data was to generate the race_ids and limit the data to particular months of interest. I chose to use the unemployment rate in October of each year with the idea that it captures the economic mood of the country right before elections, which seemed more sound than annual averages that can vary dramatically in extreme economic conditions.

###### Presidential Approval Data
Alan Abramowitz, a leading researcher on American politics, recenty told NPR that Presidential approval is one of the greatest factors that voters consider, whether consciously or subconsciously, when voting for other major offices. Even if there is no Presidential election in a given year, voters look at party leadership when considering the party to vote for in their own state.

Fortnately, presidential approval data has been regularly collected and recorded for decades, even weekly in the last few years. I was able to obtain this data from the [American Presidency Project](http://www.presidency.ucsb.edu/data/popularity.php?pres=&sort=time&direct=ASC&Submit=DISPLAY) at University of California at Santa Barbara. Again this data came already cleaned. My only task was to limit to the time periods of interest (again, October) and to average values for years where multiple surveys were conducted.

A tricky part of incorporating this type of data is obviously that the President changes! The controlling party may also change. I had to account for approval rating have a different effect in cases of a GOP or Democratic President. I did this by essentially creating a set of interaction terms. One interation term was the effect of the approval rating on GOP candidates and the other was for Democratic candidates. Structuring the variables like this means that when a President is from one party, their approval rating will have one effect on the GOP candidate in the race and another on the Democratic candidate. If the President is from the other, the effect flips. (Put another way, one party's good approval is a bad thing for the other). I also used the net approval rating (approval rate - disapproval rate) to calculate the effects so that the effect will be flipped in the case of a particularly popular or unpopular sitting President. As a note, this data is **not** differentiated by state and therefore only captures national sentiment. I was unable to find a useful breakdown by state, but hope to include it in a later iteration of the model.

###### Joining Data
All of these data sets were merged in python based on a unique race_id. The race_id was generated as a concatenation of the election year, the state, and the office (for all this was "sen"). For example, a 2012 Arizona election would have a race_id of "AZ_sen_2012". Since senate elections are staggered, this strategy worked for most races. In cases where a speical election cause a doubling up of ID's, the id values were manually changed to distinguish them when merging data.

With our data joined to a single set of elections and their political, economic, and social contexts, we are ready to move to data analysis and modeling.

## Exploratory Data Analysis
As much data as there is in this project, there is actually not a great amount of exploratory analysis that is immediately relevant or clear for the scope of this project. This is not because there is too little information but simply too much. A demographic, economic, and political analysis of a state could warrnat a study -- or book -- all its own. But we are interested in macro trends rather than state trends over time for this particularly study.

That said, there are important pieces of information for us to check before jumping in to analysis. We want to make sure we understand the distribution of and relationship between our variables and any trends that may correlate strongly with time. For example, we know that the Hispanic population of Arizona has monotonically increased over time. But that doesn't mean that Arizona hasnt followed unrelated trends in other states as well. We want to be sure that even if a variable is changing one way over time, it is not solely dominating the politics of a state. Ideally, we could look at this trend for every variable in every state. But with limited time, we instead want to be able to assume that each election is independent from one another and time to be able to model our outcomes.

##### Variable correlation

Here is the heatmap for our full set of variables (excluding identifier variables):

![](https://github.com/AlexZadel/dsi_capstone/blob/master/visuals/heatmap.png?raw=true)


A few notes from the heatmap:
- The first thing that jumps out is of course the high positive correlation block in the middle of the map, specifically with the total population data for different groups. Fortunately there is a pretty simple explanation for that pattern. Each total population variable correlates rather strongly with the others because states with more population will have more of all of them! It's certainly not a 1:1 ratio, though for large racial groups and for the two sexes (listed), it is rather close. This is just a reflection that we expect more of every type of person in places where we have more people in general. It is a good trend to note when it comes to building our model though.

- We can see another two patches of red and blue checkerboards in the upper left of the map. The larger is the variables for Presidential approval effects and the smaller is the incumbent party and candidate columns. It is not surprising that we see this pattern considering these are binary variables (or interact with binary variables), so they should have nearly perfect correlation or inverse correlation. Certainly we want to keep that in mind for dropping one category when we go to model.

- A similar checkerboard pattern appears in the right corner, though not as clearly. This is once again because of needing to leave out a group for variables that sum to 1.

-The year row, the very first in the map, is reassuring to any concerns about time correlation. We can see that the balance between different age groups has changed over time, which we know reflects demographic changes in the country. But this correlation is not so high as to be a major concern. There is a pattern to the Presidential approval as well but that is not entirely surprising given that Presidencies are fixed temporal terms so we'll get some continuity with multiple terms or if a party keeps its hold on the Presidency.

- Looking at our target variable, 'GOP_win', we can see that the biggest correlation comes with incumbent candidates. Some light relationship with racial balance variables could indicate where some of the distinguishing variation originates as well.

##### Variable Distribution
There are few variables where it is worth checking their distribution to be certain they do not introduce bias into our model.

| Variable        | Mean     | Percentage |
|-----------------|----------|------------|
| GOP_win         | 0.496847 | 49.6%      |
| pred_GOP        | 0.518285 | 51.8%      |
| inc_GOP_running | 0.409836 | 41.0%      |
| inc_DEM_running | 0.360656 | 36.1%      |
| prez_GOP        | 0.602774 | 60.3%      |
| percent_female  | 0.508948 | 50.9%      |
| unopposed       | 0.005044 | 0.5%       |


We can see from this table that the two major parties have each one about half of the Senate elections in our analysis period. They were also the incumbent party in roughly half of those races. (Unsurprisingly those two numbers are close given the incumbency advantage). Furthermore from the two incumbent variables, we can see that an incumbent candidate is running in roughly 76% of the elections in our dataset. That could certainly be a source of bias to be concerned with when trying to project years where there are no incumbents.

We have a balance between the sexes throughout the observation period, which is important given the gender cap in voting behavior. The age and racial balances change over time, as we saw in the heatmap above.

Finally, we an see that we have a few cases in our model where candidates have run unopposed. This is particularly ucommon in Senate elections, but certainly happens. Given that they are a strange case, it may be worth excluding them from the model, though there are so few that bias seems unlikely to have a major effect. That said, those elections can still tell us something, because maybe there is a reason nobody from the other party wanted to run? Maybe the thought they had no chance!

As I noted before, looking through each variable for each state might provide more insights, but time prohibits that depth of exploration, so we are forced to assume relative independence between elections.

## Modeling
#### Last Preparations
All of the steps desribed here can be found in the [Modeling Notebook]https://github.com/AlexZadel/dsi_capstone/blob/master/notebooks/5__Model%20Building.ipynb)

With EDA complete, we can begin modeling, but we need to start by addressing some of the issues that we found in the EDA analysis.

Firstly, we want to begin by using the percentage values for race, age, and sex. Given their high correlation, there could be serious pitfalls in using the total values. So we start by dropping all the total amount columns in the census section.

Next I dropped an "odd man out" for each collinear/binary group: "percent_male", "percent_under_18", "percent_race_natamer".

From there, we can begin modeling.

#### Modeling
To be as rigorous as possible, I ran the data through seven types of classifiers: Logistic Regression, KNeighbors Classification, Decision Tree Classification, Bagging Classification, Random Forest Classification, Adaboost Classification, and a Support Vector Machine Classifier.

##### Cross Validation Score
I began by calculating cross validation scores for each model: <p>

| Model | Mean CV Score | Std CV Scores |
| ------ | ---------- | ---- |
| Logistic Regression  | 0.8081  | 0.02737  |
| KNeighbors Classification  | 0.7995  | 0.02922 |
| Decision Tree Classification  | 0.7946  |  0.01872 |
| Bagging Classification  |   0.8433  | 0.02082   |
| Random Forest Classification  | 0.8333  | 0.01506  |
| Adaboost Classification  | 0.8367  | 0.03256  |
| Support Vector Machine Classifier  | 0.8081  |  0.04062

There are quite a few good scores in these preliminary tests, which is promising for modeling. The all have solid CV Scores and low score standard deviations.


##### Modeling with GridSearchCV

The next step is to run each of these models through a grid search. In order to incorporate cross validation, I chose to use the GridSerachCV package. As you can see in the notebook,

| Model | Training Accuracy | Test Accuracy |
| ------ | ---------- | ---- |
| Logistic Regression  | 82.0%  | 83.9%  |
| KNeighbors Classification  |  99.4% |  89.9% |
| Decision Tree Classification  | 99.5%  |  87.9% |
| Bagging Classification  |  96.0% | 87.9%   |
| Random Forest Classification  |  99.5%  | 90.9%  |
| Adaboost Classification  | 93.3%  |  81.4% |
| Support Vector Machine Classifier  | 81.2%  | 82.4% |

##### Final Model: Choosing Between Two
Based on the results of the grid search, I chose two models to further refine and test: the KNeighbors Classifier and the Random Forest Classifier. Given their high training scores, they are clearly overfit in the grid search versions. However, their test accuracy values are still high. My hope was that by tuning these models further, I can cut back on the overfitting while reaching 90% with the test accuracy.

Once again, I ran a grid search, but over a broader range of parameters for each model.
Next I used the information from those grid searches to tune the two classifiers further, with the hope of preventing the models from learning so far as to overfit.

These were the final results of the two models:

- KNeighbors Classifier: <br>
Training Accuracy:  <br>
Test Accuracy:   <br>


- Random Forest Classifier: <br>
Training Accuracy:  <br>
Test Accuracy:   <br>


## Results
