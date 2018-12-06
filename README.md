# Race for the Senate: Using Machine Learning to Predict Election Outcomes

<p>This project was completed as my final project for General Assembly's Data Science Immersive program in October 2018. I chose this project because it allowed me to combine several interests I developed over the course of the program: exploring socioeconomic data, combining diverse datasets, using geospatial tools, data visualization, and, of course, machine learning. </p>

<p> I have worked for multiple non-partisan political research organizations and have done my best to follow those objective principles in this analysis. </p>

## Research Question:
What will the results be for the 2018 Senate elections?




## Goals
The goals of this project are to:
1. <p>Compile a dataset of past election results and corresponding contextual data for the particular state, year, and jurisdiction of the election.
2. <p>Build a predictive model for Senate election results
3. <p>Use the model to predict outcomes of the 2018 Senate Elections.

For a full description of the data gathering, data exploration, and modeling methodology, please see the **[Technical Report](https://github.com/AlexZadel/dsi_capstone/blob/master/technical_report.md)** contained in this repository.




## Executive Summary


The goal of this project is to build a model to predict Senate election results based on economic data, census surveys, and prior election outcomes from every year since 1976.

- The data for this project was obtained from public data sets and government soruces. For a full explanation of the source data and compiled data set, please see the [data dictionary](https://github.com/AlexZadel/dsi_capstone/blob/master/data_dictionary.md).

- The final models predict with roughly 90% accuracy on testing data. It was then used to predict the outcomes of the 2018 Senate elections.

- The models are designed to be politically unbiased in the sense they do not value one predicted outcome over another. In this sense, the value of the model lies in correct predictions. Therefore this model was calibrated to maximize accuracy rather over another metric.

- This model shows promise, but there are still many ways it could be improved given sufficient time and data. The model was also designed with the specific intention of being general enough to be applied to House of Representatives and gubernatorial campaigns. The concluding section of this report goes into deeper detail on these possibilities.

This project was completed as my capstone project for the Data Science Immersive program at General Assembly.



## Technologies Used
Data consolidation and cleaning was done using Python and Microsoft Excel.
<<<<<<< HEAD
- <p> **Modeling:**[scikit-learn](http://scikit-learn.org/stable/)
- <p> **Data Management**: [pandas](https://pandas.pydata.org/), [numpy](http://www.numpy.org/)
- <p> **Visualizations**: [Matplotlib](https://matplotlib.org/), [Seaborn](https://seaborn.pydata.org/) , [QGIS](https://qgis.org/en/site/)


=======
- <p> **Modeling:** (scikit-learn)[http://scikit-learn.org/stable/]
- <p> **Data Management**: [pandas](https://pandas.pydata.org/), [numpy](http://www.numpy.org/)
- <p> **Visualizations** : [Matplotlib](https://matplotlib.org/), [seaborn](https://seaborn.pydata.org/)


## Note:
States with Senate Elections in 2018:
- Arizona
- California
- Connecticut
- Delaware
- Florida
- Hawaii
- Indiana
- Maine
- Maryland
- Massachusetts
- Michigan
- Minnesota (2)
- Mississippi (2)
- Missouri
- Montana
- Nebraska
- Nevada
- New Jersey
- New Mexico
- New York
- North Dakota
- Ohio
- Pennsylvania
- Rhode Island
- Tennessee
- Texas
- Utah
- Vermont
- Virginia
- Washington
- West Virginia
- Wisconsin
- Wyoming
