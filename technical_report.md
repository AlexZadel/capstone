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

## Background (IN PROGRESS)

<p> Before jumping in to the technical portion of this report, it is worth reviewing some background on political modeling to give context for what data and features were chosen for modeling.</p>

<p>When most of us think about using data to predict elections, we often think of polling data from campaigns or news outlets that use a phone sample to estimate the voting behavior of an entire state or district. </p>

<p>  </p>

---

## Data
<p> With that understanding of political modeling under our belts, we can now examine the technical details of this project. As always, that process begins with data. </p>

#### Unit of Analysis
<p> For this project, I saw myself as  having two possible units of analysis: a single candidate (and therefore multiple entries per contest) or a single election (between two or more candidates). I chose to use a single election as the unit of analysis for a few reasons. </p>

<p> Firstly, having multiple entries in the same race presents some potenital issues with multicollinearity since any economic or demographic data for opposing candidates would be identical except for political party. While the relationship between party, incumbency, and economic trends certainly leads to a "don't rock the boat" mentality in good economic times -- or the inverse in bad times -- I worried about the multiple candidate phenomenon under-valuing the role of those matching variables.</p>

<p> Secondly, using candidates as the unit of analysis overlooks the dynamic relationship between any candidates in a race. Elections are not strictly zero-sum since turnout is a key element, but the fact is that any vote for a candidate is one against the other. Obviously candidates in the same election are not indepdent of one another. Separating candidates would in a way imply that they alone and their profile (and context) drive the number of votes they get or whether they win or lose a race. But that is simply not true. Elections are a comparison and to accurately represent that dynamic, candidates should be considered in tandem as part of a single unit: the election. </p>

<p> Lastly, treating the question as a classification as opposed to a regression was a key factor in this decision as well. I will go further into that decision in the modeling section, but in my work that choice weighed significantly on how to structure the data. As a classification, it made more sense to classify a win or loss (as mentioned, oriented to the GOP as a win or loss) rather than a win or a loss for a particular candidate -- again due to multicollinearity concerns. </p>

Having established what structure we want for the data, we can move to the discussion of how data was gathered and from what sources.

#### Data Gathering and Cleaning

###### Election Data
<p> The first priority in terms of data gathering is election results themselves. Not only will these outcomes be the targets of our model, but given the importance of incumbency in elections, they are a valuable source of feature data as well. Since these results are public information, we can feasibly gather all Senate election results in U.S. history. (We will see later why 1976 was chosen as our start day)

<p> Surprisingly there is no (publicly-available) data set that gives quite the information I wanted for this analysis, so we had to find the outcomes another way. Fortunately, however, there are detailed Wikipedia pages on every federal and gubernatorial election. These pages thankfully include summary tables for each senate race, including the names of candidates, their party affiliation, incumbent status, and results (usually in percentages). </p>

<p> To extract this data I used several scraping functions that can be found in the [scraper notebook.](https://github.com/AlexZadel/dsi_capstone/blob/master/notebooks/1__scraper.ipynb)  As you will see, there are several different scraping functions. Given the crowd-sourced workings of Wikipedia, some pages had different table formats and therefore different scrapers had to be used for some pages. You will also see extraction functions for gubernatorial and House of Representatives elections. This data was indeed scraped, but is absent from the repository as it was not used in the current scope of the project.</p>

<p>  </p>

###### Census Data
<p>   </p>

<p>   </p>

<p>   </p>

###### Economic Data



## Exploratory Data Analysis



## Modeling


## Results



## Looking Ahead
