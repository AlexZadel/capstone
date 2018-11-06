# Data Dictionary

This document serves as a guide to the datasets and variables used in this project.




## Data Sets and Sources

All data were obtained from public documents or government published information:

- **Election Outcomes**: All election outcomes were scraped from Wikipedia entries on senate elections by year. [Example page here](https://en.wikipedia.org/wiki/United_States_Senate_elections,_1976) <p>

- **Census Data:** All data on population, sex, race, and age are from the dicennial census -- intermediate years are projected based on the nearest cenesus surveys available -- and were obtained through the [Integrated Public Use Microdata Series and the National Historical Geographic Information System](https://www.nhgis.org/).  <p>

- **Economic Variables:**  All data on unemployment was obtained through the [Bureau of Labor Statistics Data Portal](https://www.bls.gov/data/).

- **Presidential Approval:** Historical (and current) data on Presidential approval was obtained from the [American Presidency Project](https://www.presidency.ucsb.edu/statistics/data/presidential-job-approval) at University of Califronia Santa Barbara.

---




# Feature/Variable Dictionary
Variable  |Variable Name   |Data Type   |Description   |
--|---|---|---|--
Entry Identifier  |race_id   |Object   |Unique observation identifier concatenated from year, postal code, and office(sen)
Office identifier  |office_id |Object   |Identifier for particular Senate seat, concatenated from postal code and office (sen)
Location-Date ID  |loc_date_id   | Object  | Identifier for state and election year
State Name  | state  |Object   |  Name of State where election took place
State Postal Code  | abbrev  |Object   |  Two character postal code for state identification
Election Year  |  year |Int64   |  Year of Election
**Target Variable**  | **GOP_win**  | **Int64**   | **Binary variable indidcating a GOP win or loss in the observed election**
 Winning Candidate  |winner   |Object   | Winner of election
 Runner-Up Candidate  | rival  | Object | Runner Up in Election
Repubican Predecessor  |     pred_GOP | Int64  | Binary variable indicating if office was previously held by a Republican
  Democratic Predecessor|     pred_DEM | Int64  | Binary variable indicating if office was previously held by a Democrat
Unopposed Election  |    unopposed  |Int64  | Binary variable indicating if the winner was unopposed in the general election
Republican Incumbent Running  |    inc_GOP_running | Int64|  Binary variable indicating if incumbent Republican is running for reelection
Democratic Incumbent Running  |    inc_DEM_running | Int64 | Binary variable indicating if incumbent Democrat is running for reelection
Sitting Republican President  |    prez_GOP  |Int64 | Binary variable indicating if the current President is a Republican
Effects of Pres. Approval on GOP  |    approval_effects_GOP  |Float64  | Effects of current Presidential approval on GOP candidate
Effects of Pres. Approval on Dem.  |    approval_effects_DEM  |Float64   | Effects of current Presidential approval on Democratic candidate
Effects of National Unemployment on GOP  |    nat_UR_effects_GOP  | Float64 | Effect of national unemployment rate on GOP candidate
Effects of National Unemployment on Dem.  |    nat_UR_effects_DEM  | Float64 | Effect of national unemployment rate on Democratic candidate
Effects of National Unemployment on GOP  |    state_UR_effects_GOP| Float64 | Effect of state unemployment rate on GOP candidate
Effects of National Unemployment on Dem.  |   state_UR_effects_DEM| Float64 | Effect of state unemployment rate on Democratic candidate
Percent Male  |    percent_male  | Float64  | Percent of reported population that is male
Percent Female  |     percent_female | Float64  | Percent of reported population that is female
Percent Children  |      percent_age_under18  |Float64 | Percent of reported population that is under 18 years of age
Percent Aged 18 to 29  |      percent_age_18to29 | Float64 | Percent of reported population that is between 18 and 29 years of age
Percent Aged 30 to 59  | percent_age_30to59  | Float64  |  Percent of reported population that is between 30 and 59 years of age
Percent Over 60  |    percent_age_60over  | Float64 | Percent of reported population that is over 60 years of age
Percent White  |    percent_race_white_nh  | Float64 | Percent of reported population that is White
Percent Black  |    percent_race_black  |  Float64 | Percent of reported population that is Black
Percent Native American  |    percent_race_natamer  | Float64  | Percent of reported population that is Native American
Percent Hispanic  |    percent_race_hispanic | Float64  | Percent of reported population that is Hispanic
