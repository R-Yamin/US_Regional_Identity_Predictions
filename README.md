# Predicting Regional Identity in the USA
*A large part of a person’s identity derives from how they view their environment. Where they are located and how closely they identify with that area can help when trying to better understand them. This type of information is crucial in marketing. One of the key factors for selling any product, be it a physical item or a political person or body is to know the market in which you are selling, thus knowing the consumer’s identity.
 However, within the United States, outside of the state in which they live, citizens have a another category by which they self-identify: region. Yet, defining where the borders to a particular region are located can be difficult, particularly for the regions "South" and "Midwest". Within this project, we acquire data taken from an online survey and look into what patterns and trends are shared in respondents who closely identify with being from a particular region.*

## 1. Data
The dataset was acquired from the FiveThirtyEight github, from surveys that were created with similar questions regarding two regions of the US: the South and the Midwest. Survey takers could select how much they identify with being a southerner or midwesterner with the same categorical choices ranging from “A lot”, “Some”, “Not much”,  and “Not at all”. They could also write their own answers for how they would self-identify. General information was also collected such as zip code, income, education, and which region according to the census bureau each respondent was located. Additionally, each survey had a place where respondents could vote for which states they believed were midwestern or southern, with a different list of states for each of the two surveys. You can access the infomation in the link below:

>* [FiveThirtyEight Region Survey](https://github.com/fivethirtyeight/data/tree/master/region-survey)

## 2. Data Cleaning
[Data Cleaning Report](https://docs.google.com/document/d/1rRdOTVYpGogHpm_7PXoXaa9j04V-bOYeCEMEfd5BXTg/edit)

The purpose of cleaning the data was to organize the raw csv files that held the data of interest. This was necessary because when viewing the data within the files, each row representing an individual respondent had many empty cells of NAN that could be condensed. Therefore, the goal became to take the original 53 columns in the midwest dataset and the 59 columns in the south dataset and reduce them each into 9 columns, indexed by the actual respondents ID from the original surveys. These 9 columns would be:

>* Written in Responses
>* Degree of Identity (how much each respondent identified with the region in question)
>* Midwestern?/Southern? (which states the respondent voted as being midwestern or southern)
>* Zip Code
>* Gender
>* Age
>* Income
>* Education
>* Census region

### 2a. Removing Unnecessary Heading Data

Unnecessary heading rows made data organization difficult. Began cleaning by removing the first row, since the second row contained the possible choice responses and could be used as column names.

### 2b. Condensing Empty Cells into a Single Column

Many respondents had single responses for certain columns, yet there was a column for each possible response a respondent could give. This meant that much of the information from groups of columns could be condensed into a single column with a single answer the respondent gave, removing any other empty cells. By forward filling the answers along rows to a single column, then pulling that column into a clean dataframe with a new column name, it was possible to extract all the information of interest. This significantly reduced clutter, presenting clearer information for all the responses by the survey taker.
A difficult step came from the list of states that the survey taker could choose from in order to indicate which states they believed belonged to certain regions. Because each column was for a differen state, forward filling could not be used since all states in the row were needed. A new method was used with a lambda function that was able to select only the cells in the dataframe that had value, and then placing them all in a list, ensuring only the information of interest was extracted and compiled into a single column.


### 2c. Empty Values

Though many survey takers had filled in most of the reponses, some left areas blank. Two significant areas left blank were the areas of self-reported income and education. Since these areas were ones of interest for what personal factors can determine regional identity, it was decided to drop the rows that did not contain at least one value in those columns. This dropped 348 respondents from the midwest dataset and 267 respondents from the south dataset, leaving still over 2200 respondents from each dataset. This was a minimal loss compared to the original totals of 2778 from the midwest dataset and  2528 from the south dataset.

### 2d. Extra Data

Lastly, the dataframe for the median reported household income for each state was acquired by downloading the excel file locally from the Census Bureau website, then isolating the columns related to the year 2014. It can be found in the link below:
>* [Median Reported Household Income](https://www.census.gov/data/tables/time-series/demo/income-poverty/historical-income-households.html)

## 3. EDA
[EDA of cleaned South and Midwest data](https://github.com/R-Yamin/US_Regional_Identity_Predictions/blob/master/2.%20EDA%20of%20Region%20Data.ipynb)
