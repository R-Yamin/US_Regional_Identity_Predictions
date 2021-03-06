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

### 3a. Written Responses

Looking at the data and breaking down the responses of how people identified with being either a southerner or midwesterner, there was an interesting spread of results. Below is the comparison of the responses of those that identified with being either southern or midwestern and the written responses:
![](https://github.com/R-Yamin/US_Regional_Identity_Predictions/blob/master/Saved%20Images/written%20responses%20and%20self%20identity.png)

Already it can be seen that many of the responses to the 4 categories for the labeled responses fall into the "Not all all" category, yet use similar word choices of those respondents from other categories.

### 3b. Voted States
Below are graphs of US states measured against the number of votes they recieved if they were thought to be part of the South or the Midwest. 

![](https://github.com/R-Yamin/US_Regional_Identity_Predictions/blob/master/Saved%20Images/States%20Votes%20by%20region.png)

The voting is also broken down by the 4 categories the respondents self-identified with. Though distinctions between the 4 categories within states might be difficult to determine due to the number of respondents, between states the distinction becomes clearer. Looking at the southern data, Georgia to Virginia clearly had the most votes, with a drop-off at West Virginia. A similiar pattern showed up for the midwestern data from Iowa to Michigan. This came to show that this category of voting would be important later to determine how to predict others' regions.

### 3c. Census Region
Looking at the Census region responses, the breakdown of self-identity to their actual locations showed particular patterns.
For the southern data below:

![](https://github.com/R-Yamin/US_Regional_Identity_Predictions/blob/master/Saved%20Images/South%20census%20region.png)

We can see that almost all of the "A lot" responses come from three regions in particular. The Midwestern data is even clearer:

![](https://github.com/R-Yamin/US_Regional_Identity_Predictions/blob/master/Saved%20Images/Midwest%20census%20region.png)

### 3d. EDA Summary
>* There are clear results for particular states that are believed to be southern or midwestern, though how those breakdowns happen are shown to be distinct between those that identify strongly with those regions than those who do not.
>* The census region data showed a clear breakdown of the 4 categories of regional identity, with those identifying a lot with south or midwest regions of the US falling into distinct regions.

Judging from the results above, the prediction models for these datasets would likely heavily rely on the states respondents voted for and their census region to determine how likely they are to be southern or midwestern.

## 4. Cross Validation and Testing Results

Three-fold cross validation was performed with the Support Vector Machine (SVM) and Random Forest Classifier algorithms. Breaking the data into training and testing data, a grid of parameters were given such that the hyperparameters could be tuned in order to achieve the highest level of accuracy possible for this model. For the South dataset, however, it was only possible to achieve a 49% accuracy result for the model using the SVM. This is likely due to the fact that with the southern dataset, there was no cleanly defined results. Looking at the distribution of the census regions based on self-identity above, though the "A lot" category is mainly in three census regions, the rest are dispered throughout the different regions of the US. This means that with such a wide distribution and no clearly defined line, achieving a higher accuracy for the model was difficult.

The SVM for the midwestern dataset had an highest accuracy of 61%, likely due to the fact that the census region data was more cleanly split into the two categories of West North Central and East North Central for the midwestern data. This would have helped in narrowing down at least two labels of interest for self-identity: “A lot” and “Not at all”. In fact, the other two labels of “Some” and “Not much” were not even given a precision and f1-score in the classification report for the SVM of the midwest dataset. 

For the Random Forest Classifier model, the results in accuracy were the same for the midwest data, but at least 2% improved accuracy for the south dataset, for a 51% accuracy for that data. While the random forest model worked slightly better than the SVM for the south dataset, they were pretty similar, despite the fact that the random forest had more information. This meant that the extra categories that were added to the Random Forest Classifier were probably not as significant as the first two categories of voted states and census region information.

## 5. Next Steps and Future Improvements
* __These were toy models based on a mock business proposal by marketing companies.__ Real data was used from surveys but because the data was from an online website, the types of questions asked and the respondents that answered were clearly more skewed towards the typical visitors to the website. Real buisnesses would ask more detailed questions and would more directly target different populations in the US.
* __More data would have been better to train and test the data on.__ While over 2000 samples were used for each dataset of the South and Midwest, because of how ambiguous the categorical information was, it was hard to create a predictive model for the categories that were not at the extreme ends of the spectrum of choices. With more data, it would most likely have improved the results for the categories of "Some" and "Not as much".
