#
# St. Paul Police Stops and Identity

Section 2

A&#39;di Dust

#
## Introduction

## Introduction to Topic

The United States has grappled with racially motivated policing since its inception. In the summer of 2020 with the killing of George Floyd, the twin cities area became a poster child of harmful policing practices. With this intense scrutiny, many connections have been made quantitatively, qualitatively, anecdotal, and historically linking racial bias to excess force in policing. This report will focus further on personally invasive policing practices, such as searches, and how different identities might correlate with these practices.

## Research Questions

Beyond disciplinary action such as citations and police brutality, there is evidence that invasive police protocols are inappropriate, ineffective, and trauma inducing, especially for those communities that are already most impacted by policing (Bandes). The first question this report asks is if the number of police searches correlated to subject sex and race, which are demographics that are provided in the dataset and also most prevalent to the current investigations of police practices. Who of the population within this dataset is being searched the most and the least?

After investigating _who_ is and is not being searched by police after being stopped, the next question addresses what some of the recordable outcomes of being searched might be. This report asks if a subject is more likely to be cited based on the different searches that are conducted on them, including frisks and general searches. This is a direct effect of being stopped that can be compared to searches and seen as a general outcome of stopping subjects. After seeing who is being searched, it is useful to know the consequences of these searches for them.

#
## Data

## Context

The dataset used for the analysis in this report was collected and cleaned by the Stanford Open Policing Project. Further description of their work and the data files can be found at [https://openpolicing.stanford.edu/](https://openpolicing.stanford.edu/).

This dataset contains 675,156 cases, each of which represents a time someone was stopped and cited by the police in Saint Paul. Each case was collected at some documented time between 1/1/2001 and 12/31/2015. The Minnesota state and local police stations where this data was collected from are required to keep record of citations and stops, and the Stanford Open Policing Project collected, standardized, and aggregated data from these stations. They created these datasets by filing requests for publicly available data for each state/city they collected data on and then aggregated numbers based on common variables they could deduce from the public datasets. Theoretically, this dataset should be the complete population of stops/citations between 2001 and 2015 in Minnesota. This is assuming police record all that they are supposed to and the Stanford Open Policing project was given all records.

The Stanford Open Policing Project collected and standardized 16 variables for the creation of this dataset. Each variable represents characteristics of the subject who was stopped and the nature of the stop, along with outcomes.

Logistical information about the stop, including time, location and police station grid number are included.

This report focuses on the demographic descriptions of subjects and outcomes. Subject race is categorized as one of either _Asian_, _Black, Hispanic, Other,_ and _White_. There is also a variable including the raw race of the subject, which is the category in the original police station dataset from which this aggregate dataset derives race. Subject sex is also included and is recorded in the categorical binary as either male or female. The last demographic variable included is subject age, which is recorded in years and is only available for less than 15% of the cases.

Outcome variables included types of searches and citations. Outcome variables are double encoded in a citation and an outcome variable, which contain binary _true_ if a citation was issued or _false_ or _NA_ if a citation was not issued. If a citation is not issued, there is no clear indication if there was a warning given or something else. The types of search variables are if a frisk was performed, a search was conducted, or a vehicle was searched. Each search variable is _true_ if that type of search was performed in that case, and _false_ or _na_ if not performed.

## Cleaning

One transformational change I made to the policing dataset included creating a numeric variable that sums the number of searches that was conducted on each stopped subject. I did this by adding 1 to the sum if a frisk was performed, if a search was conducted, or if the vehicle was searched. The maximum number of searches was 3 and the minimum was 0. I created this variable because I was interested in how race and sex would effect the total searches conducted, and not just any one search on its own.

The other transformation I performed was turning the citation variable from a boolean into a numeric variable of 0 for false and 1 for true. This was simply so that I could more easily use this variable to create a logistic regression model predicting if subjects received citations.

#
## Is the number of police searches correlated to subject sex and race?

## Exploratory Data Analysis

The above visualizations describe the proportion of the different number of searches performed (0, 1, 2, or 3) based on the subject&#39;s sex and race. In the chart on the left, Black subjects made up a majority of those searched 1, 2, and 3 times. About 46% of those searched once, 42% of those searched twice, and 50% of those searched three times were Black. White subjects were the majority of those who weren&#39;t searched, as 40% of those not searched were White. In the chart on the right, men were stopped more than women overall. Over half of those searched 0 and 2 times were men, 85% of those searched once were men, and 89% of those searched 3 times were men. These charts clearly show that subjects stopped in St. Paul were primarily men, White, or Black, and that men or Black subjects were also the most highly searched. 
![pic1_stat155](https://user-images.githubusercontent.com/56092297/149683916-ef5dd3ce-76e4-4b03-b0fb-968ed578efcf.PNG)

## Model Creation

In the creation of this multiple linear regression model, I decided to include race, sex, and the interaction between race and sex as predictor variables for the sum of police searches. I transformed variables to create a sum of police searches including the three types of search, which serves as a measure of the amount of privacy invasion took place during the stop. I chose to include race and sex as variables since they are the main demographic variables in the dataset. I did not use age, since only around 13% of the population have reported age, which would eliminate the majority of the dataset and not be particularly helpful in identifying larger trends. I originally fit the model using sex and race as seperate indicator variables, but due to the historical implications of sex in relation to race and characteristics of combination identities, I decided to use sex and race as an interaction variable as well.

## Fitted Model

| **Model Coefficient** | **Estimate** | **95% Confidence Interval**** (LB, UB) **|** P-Value** |
| --- | --- | --- | --- |
| **Intercept** | 0.05 | (0.03, 0.06) | <= 0.0001 |
| **raceBlack** | 0.07 | (0.06, 0.09) | <= 0.0001 |
| **raceHispanic** | 0.06 | (0.03, 0.08) | <= 0.0001 |
| **raceOther** | 0.10 | (0.06, 0.14) | <= 0.0001 |
| **raceWhite** | 0.04 | (0.02, 0.05) | <= 0.0001 |
| **sexMale** | 0.19 | (0.17, 0.20) | <= 0.0001 |
| **raceBlack\*sexMale** | 0.18 | (0.17, 0.20) | <= 0.0001 |
| **raceHispanic\*sexMale** | 0.11 | (0.09, 0.13) | <= 0.0001 |
| **raceOther\*sexMale** | -0.09 | (-0.13, -0.04) | 0.0003 |
| **raceWhite\*sexMale** | -0.04 | (-0.06, -0.02) | <= 0.0001 |

## Model Interpretation

This linear model compares the number of searches done (out of 3 possible search types) on those stopped with the average number of searches performed on Asian Women who were stopped in St. Paul, 0.05 searches. The coefficient for &quot;other&quot; race for women is 0.10, signifying that on average, those women with &quot;other&quot; race are searched 0.10 more times than Asian women. Out of all female drivers stopped, women with &quot;other&quot; race were the most searched, with an average of 0.15 searches total. This coefficient has a p-value of less than 0.0001, indicating the small likelihood that the dataset would contain this extreme of values should no relationship between being an other race women and being searched more than Asian women exists. On the other hand, Asian women were the least searched of those stopped, with an average of 0.05 searches of the group that was stopped.

On average, Asian males who were stopped were searched a greater amount than their female counterparts of the same race. The coefficient for men is 0.19, meaning that on average, Asian men were searched 0.19 more times than Asian women. The male and Black male coefficients have the same confidence interval, indicating that we are 95% confident that Asian men on average are searched between 0.17 and 0.20 times more than Asian women. This is such that we would expect 95% of samples from the total population to generate 95% confidence intervals including the true coefficient. Similarly, we are 95% confident that stopped drivers of Black race in comparison to stopped drivers of Asian race are on average searched between 0.17 and 0.20 times more among male drivers in comparison to female drivers. As neither of these ranges include 0, we can conclude that the coefficients indicate a positive relationship between black drivers compared to asian drivers who are male compared to female. Both variables also have p-values less than 0.0001, indicating the small likelihood of having this range of coefficients should the null hypothesis of Asian women being searched more than &quot;other&quot; women to be true. On the other end of the spectrum, those with race labeled &quot;other&quot; were searched on average 0.09 times less than their Asian counterparts if they were male, rather than female.

## Model Evaluation 
![pic2-stat155](https://user-images.githubusercontent.com/56092297/149683939-80143074-07b3-474b-9954-f8869f26fe30.PNG)


Generally, the plot of residuals stays around zero, although there is a trend of overestimation (inherent to the fact that the maximum number of stops is 3 and the values are discrete). The model tends to overestimate the number of searches performed on all groups of people in the dataset. Our model overestimates more often the number of searches performed on Black and Hispanic people who were stopped than other races. The spread of residual values are all fairly evenly spaced throughout the residual plots, other than the boxes for Black and Hispanic subjects, who had a larger 50% quartile range. The model also overestimates for male subjects more than female subjects, as well as having a larger spread of residuals for male subjects Each plot has approximately the same amount of outliers. The R2 value is fairly low, with around 3.6% of variance in number of searches being explained by the model. Residual standard error is a bit high, with most of the predicted values being within 1.58 searches (2 times the Residual Standard Error) of the actual amount of searches that were conducted. This is high since the range of numbers of searches is between 0 and 3, meaning that the RSE is about half of this range.

Although R2 and Residual Standard Error are not the strongest measures of fit for this model, they are slightly better than if I didn&#39;t use an interaction variable. The fact that the data includes discrete outcomes between 0 and 3 stops could also have an impact on these measures of fit. Overall though, these variables don&#39;t explain the whole story on why some individuals were searched more than others. Despite this, the model does indicate that by just using these two variables, we can describe some differences in the amounts of searches being conducted.

#
## Is a subject more likely to be cited based on the searches conducted on them?

## Exploratory Data Analysis
![pic3-stat155](https://user-images.githubusercontent.com/56092297/149683949-6728fa52-374c-482d-baaa-9717611c0cd6.PNG)

##


The above data visualizations indicate that the large majority of subjects were not cited, frisked, or searched. Of those that were frisked, around 22% of subjects were cited, whereas those subjects who were not frisked were cited about 13% of the time. There were even less people who were searched, and similarly 24% of searched subjects were cited, whereas 13% of unsearched subjects were cited. These visualizations demonstrate that while invasive practices such as frisks and searches are uncommon, they generally don&#39;t turn up a substantial amount of citations more than those who went searched and frisked.

## Model Creation

To answer the question of how searches might correlate with the dealing of citations for stopped drivers, I decided it might be interesting to consider the different types of searches and how each one independently affects the citation outcome. I originally considered simply using a quantitative variable representing the sum of all searches conducted on a person, but later decided that the categorical descriptions of types of searches would be more descriptive in understanding the effects of these searches. As I am looking at the effect of searches, I decided not to include race or sex to avoid any confounding factors. I simply wanted to see first who is getting searched more from the linear model and then the results of searches in the second model. Below describes the likelihood of stopped Minnesotan drivers being cited if a search was conducted or if a frisk was performed on them.

## Fitted Model

| **Model Coefficient** | **Estimate** (exponentiated) | **95% Confidence Interval** (exponentiated) | **P-Value** |
| --- | --- | --- | --- |
| **Intercept** | 0.15 | [0.149, 0.151] | <= 0.0001 |
| **friskPreformed** | 1.46 | [1.407, 1.524] | <= 0.0001 |
| **searchConducted** | 1.41 | [1.348, 1.473] | <= 0.0001 |

## Model Interpretation

This logistic regression model suggests a relationship between invasive searches conducted by police and citations within the data sample. Of those who were frisked, holding searches constant, the odds of being cited were greater than those who weren&#39;t frisked by a factor of 1.46. This holds statistical significance, as the p-value is less than 0.0001, so we conclude that we have a less than 0.001% chance of having collected data this extreme should the null hypothesis, that a stopped driver being frisked does not result in a change in the odds of being cited, be correct. The increase in likelihood of a driver being cited if they are frisked is also supported by the confidence interval. We are 95% confident that the coefficient describing the odds of being cited if the driver is frisked is between 1.41 and 1.52 times higher than if the driver was not frisked, holding searches constant. This is such that we would expect 95% of samples from the total population to generate 95% confidence intervals including the true coefficient. This spectrum does not include 1, giving evidence against the null hypothesis, that a stopped driver being frisked does not result in a change in the odds of being cited.

Similarly, stopped drivers with searches conducted were more likely to be cited than those who didn&#39;t, holding whether they were frisked constant. Those who were searched out of the stopped drivers had odds of being cited that increased by a factor of 1.41 compared to those who were not searched. Once again, this value has a statistically significant p-value less than 0.0001, indicating that there is a less than 0.01% chance for us to have collected this data should the null hypothesis, that a stopped driver being searched does not result in a change in the odds of being cited, be true. We are 95% confident that stopped drivers that are searched have odds of getting a citation increased by a factor of between 1.35 and 1.47 times compared to those stopped drivers who were not searched holding frisking constant.

## Model Evaluation
![pic4-stat155](https://user-images.githubusercontent.com/56092297/149683953-3ad7dcde-d164-4fa4-8318-b7e4131e85a0.PNG)



In order to evaluate this model, a threshold of 0.131 is used, indicating that the model will predict a citation if the probability of receiving one predicted is 13.1% or more. If the threshold is lower, the model never predicts a citation, whereas a higher threshold leads to less accuracy.

The above box plot shows that the median predicted probability of those that were and were not cited in reality based on the model is around 13%. This is at least partially due to the fact that there were so few drivers that were cited or searched in reality. The accuracy of this model is 81.3%, indicating that the predictions made by the model are correct most of the time. The specificity is 92.1%, indicating the percent of time which the model predicts a citation wouldn&#39;t be given when it actually wasn&#39;t given. The false positive rate is 7.9%, so the model rarely predicts that a citation was not given if it was given in reality. This model performs well in this area most likely due to the fact that a majority of the stopped drivers were not cited. On the other hand, the model performed poorly when predicting that citations were given. The sensitivity of 13.9% represents the proportion of the time the model predicts a citation would be given when a citation was actually given. The false negative rate of 86.1% indicates the amount that the model fails to predict that a citation would be given.

Since we are more interested in those who were cited than those who aren&#39;t, the incorrect predictions for those who were cited indicates that this model is not sufficient to predict citation outcomes on its own. Although there are models that might describe more variability in citations, this model was chosen because it indicates that the use of searches and frisks have little substantial and predictive impact on the legal outcome of the search (being cited). Since we only accurately predict citations based on searches and frisks less than 15% of the time, these descriptive variables likely play a small role in the actual process and reasoning behind citations.

#
## Conclusions

## General Takeaways

Within this report, careful analysis of St. Paul vehicle police stops were conducted to focus on who is being searched more, and if being searched is promoting negative outcomes for the stopped driver. The linear regression model performed shows that, yes, it does appear from this dataset that certain groups of people were being searched on average more frequently than others. Although the conclusions made from our model only explain some of the reason for discrepancy in searches, it is interesting to see that based on race and sex, drivers could be stopped more often than others. Specifically, the groups of drivers who were Black, Hispanic, and/or male were stopped more frequently than other races. Specifically in the case of Black men. The average differences in stops between all of these groups were under 1 (on a scale from 0 to 3 searches), so the effect might not be sizable in some cases, but for groups like Black males, an average of 0.58 searches could indicate a meaningful difference from having no searches conducted, considering the psychological effects of privacy invasion.

After finding that certain groups were likely to be searched more than others, the logistic regression model looks at how different search types (frisking and searching) impacted the likelihood of a driver being cited. Although there was a statistically significant correlation between being searched and/or frisked and being cited, the model itself does a poor job at predicting citations just using frisking and searches. The model could be improved by looking at other factors such as race and sex demographics or stop reasons, but since the focus of the model was on searches and frisks, the use of these two variables only allows for a closer look at how they alone might effect the outcome. There isn&#39;t enough evidence to conclude that conducting searches and performing frisks made a sizable difference in whether or not a person was found to be engaging in illegal activity that requires a citation.

## Limitations and Ethical Considerations

This dataset is a step towards identifying possible problematic distributions of searches for demographic identities. The use of this dataset allows for more accountability in terms of police interaction with drivers if this data is collected, shared, and visualized.

Despite the possibility for ethical benefits of using this dataset, there are various limitations and considerations to make as the data is analyzed. First, there is a chance of mis-identification of races, which could impact categorical assumptions and predictions based on these categories, specifically in the linear regression model. This is especially impactful for those in the &#39;other&#39; category, who could be of any racial group and mixed race individuals who are not specifically categorized separately in this dataset. There is also possible harm in making comparative assumptions on data that was collected through a conglomerate of different police stations/sources in both our model analyses. We also can&#39;t be sure that the categories consisting of &quot;no data&#39;&#39; are free of information bias because there is no way to know why that data wasn&#39;t collected (since each department collects information differently).

It is also important to keep in mind that the laws and norms for citations and stops are constantly changing, along with pressure on police to better document their stops, so there is a chance that older data isn&#39;t as representative of today&#39;s standards which is a possible sampling bias which could affect conclusions from both regression models. There have also been serious accusations that police purposely avoid documentation of incidents, so this could also have substantial effects on the data in the form of information bias.

#
## Appendix

## Works Cited

Bandes, Susan A., et al. &quot;The Mismeasure Ofterrystops: Assessing the Psychological and Emotional Harms of Stop and Frisk to Individuals and Communities.&quot; _Behavioral Sciences &amp; the Law_, vol. 37, no. 2, 2019, pp. 176–194., https://doi.org/10.1002/bsl.2401.

E. Pierson, C. Simoiu, J. Overgoor, S. Corbett-Davies, D. Jenson, A. Shoemaker, V. Ramachandran, P. Barghouty, C. Phillips, R. Shroff, and S. Goel. &quot;A large-scale analysis of racial disparities in police stops across the United States&quot;. _Nature Human Behaviour_, Vol. 4, 2020.
