---
date: '2023-04-17T11:50:54.000Z'
title: >-
  Predicting traffic accident severity and the effect of Cannabis legalization,
  A machine learning approach
tagline: 'Course Project for ECO480 with Professor Clément Gorin, ML for Economists'
preview: >-
  17.6% of fatal traffic accidents were due to impaired drivers in Canada, how
  is this affected following the legalization of recreational cannabis since
  October 2018?
image: >-
  https://techcrunch.com/wp-content/uploads/2022/04/GettyImages-1285800836.jpg?w=730&crop=1
---

# Introduction

Understanding patterns in traffic accidents is essential to building a safe transportation system and preventing accident fatalities. Besides the environmental factors such as road condition and weather, human factor contributes a significant portion of all the accidents. In Canada, 17.6% of the fatal collisions were due to impaired drivers, meaning the driver(s) was under the influence of alcohol or drug (Transport Canada, 2022).

After receiving Royal Assent in June 2018, the Cannabis Act became effective on October 17, 2018, making recreational Cannabis accessible to the public beyond legal ages (Government of Canada, 2022). Despite the huge industry this Act opens and the start of a new era of Cannabis control, it is important to realize the potential harms to the society from the consumption of Cannabis.

Prior studies have shown significant inverse relationships between the consumption of Cannabis
and ability to drive, particularly for occasional smokers (Hartman et al., 2013). Intuitively, this
suggests a positive relationship between traffic accident severity and Cannabis legalization, which
will be explored in this essay.

## Data Sources

Data for this paper include the following:
1. Traffic accident data: The Traffic Accident Report provided by Quebec Data Partnership provides an exhaustive list of police-reported motor vehicle collisions in Quebec, Canada from 2011 to 2021 (Government of Quebec, 2022). The dataset provides some important factors that may affect crash severity, such as weather and road condition, and metadata such as date and severity.

2. Labour Force Characteristics: Provided by Statistics Canada on table 14-10-0287-01, this dataset provides monthly data on statistics such as population, employment and unemployment rate (Statistics Canada, 2023). Data for the province of Quebec is specifically extracted from the website and downloaded as a csv file for analysis.

3. Alcohol Consumption Data: To obtain alcohol consumption data, table 10-10-0010-01 is utilized, which provides the absolute volume for total per capita sales of alcohol beverages in Quebec, Canada (Statistics Canada, 2023).

4. GDP data: An additional predictor I will look at is the economic development level, characterized by the GDP. In Akinyemi’s paper, one investigated the relationship between economic growth and traffic accident in Nigeria, which I will adapt in the setting of Canada (Akinyemi, 2020). In his research, results suggested an inverse relationship between crash/fatality rates and GDP in the long run, which could be a result of better and safer transportation system built from GDP growth. Given this result, we would expect to see a higher negative effect of GDP on the likelihood of high severity accidents.

## Data Cleaning

To clean the traffic accident data, which will be referred to as the traffic data in the future, I first downloaded the 11 years of data from the Quebec Data Partnership’s website and stacking them as a single data frame.

## Selecting variables

To wrangle the data into the desired format, I first created a ‘month’ variable by extracting the month term from the accident date. Then, only relevant columns are selected, which I judged based on amount of missing values, unique categories, and possible relationship with accident severity.

Most importantly, since the Cannabis legalization took place in late October, 2018, I created a binary variable ‘cannabis’ to map the accidents from November 2018 and onwards to True, and False otherwise. This is similar to the technique used in a regression discontinuity analysis.

In particular, variables with mostly missing values are dropped since they will affect the number of observations available for modelling. When only some observations are missing or when the column is intuitively important, I replaced the missing values with either ‘Not Specified’ for categorical variables, or the average value for that column for the only numeric variable with missing values, ‘speed_limit’.

For the few categorical variables with a large number of unique values, such as the street number and nearby landmark, I dropped them for mainly three reasons. First, they may be too specific for the observation and could lead to generalizability and overfitting issues. Second, it is difficult to assess the unique categories and identify similar categories. Third, they will increase the computation cost, making it extremely to compute the results from different models. Lastly, for variables that are directly related to the severity level or unique identifier for the accident, they are removed accordingly.

## Translation

Since the data set is representing Quebec and provided by the Quebec Data Partnership, all columns
and values are written in French. Hence, for interpretability and ease of work, I translated them all
into English, except for a few columns where the values represent a location or region in Quebec.
The translation is relied on Google Translate and Deepl.

## Handling missing values

For the 21 categorical variables, including the response variable ‘severity’, missing values are mostly filled using a new group representing ‘Not Specified’. Except for a few exceptions, including the 1 observation that is missing the weekday variable, and about 30 observations with missing region or county, where those observations are removed from the data set completely, since this only consists of a small portion.

For the speed limit variable, since I want to treat it as a numeric variable, which allows me to evaluate the continuous effect of increase speed limit, I filled the missing values using the average speed limit. Although this may cause bias, such that the accidents with missing speed limit may be on roads without speed limit or unspecified, which could intuitively increase the level of severity, using the average speed limit attempts to balance out the bias such that hopefully it doesn’t pull the coefficient towards one side.

## Joining GDP, Employment, Alcohol

The data utilized for GDP and alcohol consumption representations are both annually, which is
on a larger time scale compared to the hourly accident data. To merge them with the traffic data,
I utilized the year variable as the key, so the traffic accident observations also include a variable
indicating the GDP and alcohol consumption statistic for the year it took place.
For employment rate, it is monthly, so I leveraged the ‘month’ variable created earlier along with
the ‘year’ variable to merge it with the traffic data.

# Explanatory Data Analysis

## Severity Time Trend
There are five severity levels, including from a scale of 1-5, “Property Damage Below Threshold”, “Property Damage”, “Light”, “Serious”, “Fatal”. “Light” refers to one or more slightly injured victims. “Serious” refers to at least one seriously injured victim, but no death. “Fatal” refers to at least one death from the accident within 30 days. 

Figure 1 depicts the time trend in traffic severity level in Quebec, from 2011 to 2021. We observe that the most common severity level is level two, which is accidents that lead to property damage. Noticeably the level one accidents have significantly decreased over the past decade, which suggests a negative time effect. For both level one and level two accidents, they are more common in the cold seasons, and less common during summer, which is intuitive since there could be more weather related accidents during cold seasons. The level three and level four accidents are the opposite, whereas they are more common during warmer months. Intuitively, this is when more pedestrians are present, which leads to higher severity levels.

The red dashed-line represents cut-off of Cannabis legalization, which if we compare the trend around the legalization, we observe no significant rise in the amount of accidents at any severity levels. However, note that the time period starting 2020 also includes the potential effect of Covid-19, which negatively impacts the traffic volume and hence, the amount of traffic accidents

## GDP, Employment, Alcohol Consumption Time Trend
When we examine the time trend of GDP, employment rate, and alcohol volume per capita, we notice a significant negative impact of Covid-19 on GDP and employment rate. Therefore, hopefully these two variables can help explain the effect of Covid-19 on traffic severity level, combined with the year and month variables.

For alcohol consumption, there appears to be a clear decrease from 2011 to 2014, but then rose slightly before fluctuating around 8.4-8.5, which may suggest a weak impact on the severity level.

![Figure 2a](https://drive.google.com/uc?export=view&scale.option=fit&id=1M1nf7glrvB4ebi6l9ovQ2MNL5oxUsTTL)
![Figure 2b](https://drive.google.com/uc?export=view&scale.option=fit&id=1YDDlbc6NTkQBVuPcud9IAQzsA_vqN74F)
![Figure 2c](https://drive.google.com/uc?export=view&scale.option=fit&id=1du8ffE0RUgSwNsI6pXbiX9oRH3ucl_FK)

## Other discussions
The full summary tables for each variable can be found in Appendix Table 1, which is divided into the training and testing data sets.

By comparing the frequency of each categorical variable or mean value of each numeric variable, we do not observe noticeable differences between the training data and test data, which suggests that the two groups should be similar, and the results will solely depend on the responses.

Since our focus in on the effect of Cannabis legalization, we observe that there is a total of 229908 traffic accidents from November 2018 to end of 2020, which will be used for training, and 100486 accidents in 2021, used for testing. Compared to a total of 1142427 accidents before the legalization, we certainly have a lot of data to work with, but it could be difficult to separate out the effect of Cannabis from the impact of Covid-19, since we are only using a Boolean variable to represent Cannabis consumption.

# Method
Given there are five severity levels, to keep things simple, I will fit one model for each severity level to predict if the observation belongs to that severity level. Although a multinomial technique could be utilized here, it is slightly beyond my understanding, so I will not risk to interpret it for now. In general, I will leverage 4 modelling techniques and compare their training and testing performance. For each model, I will try to interpret it if possible and examine the effect of Cannabis legalization.

## Train/Test split
To validate the models and prevent overfitting, the traffic data will be split into training data and testing data. Since our response is accident severity and response includes time variables, also that we are identifying the trend across time, we have a time series data set. Adapting Iranitalab and Khattak’s technique and common methods for modelling time series data, I will utilize the most recent year’s data (year 2021) as the test data set, and the prior for training the models. This is slightly unfortunate since we are loosing a lot of accidents after Cannabis legalization.

## Logistic Regression
The classic logistic regression used to classified observations by maximizing the likelihood of the observation belonging to a particular class. To improve the model, I will implement three models with different regularization settings: the base algorithm without any penalty term, a Ridge penalty using L2 norm, and a Lasso penalty using L1 norm.

## k-th Nearest Neighbor Classification
KNN is a clustering algorithm that classifies the observations based on similar observations by computing a distance metric that represents the “similarity” between observations. The main tuning parameter for KNN is the number of nearest neighbors (k) to compare to. Similar to Iranitalab and Khattak’s paper, I will also conduct a grid search on a k from 1 to 10, and choose the model with highest testing performance. Since KNN performs poorly on large datasets, as it needs to compute the distance between each observation to every other observation, I had to further subset my train/test datasets to reduce the runtime to a reasonable range. To do this, I randomly selected 1% of the data from the training set and 50% of the data from testing.

## Random Forest
Random forest leverages ensemble method to yield high performance, such that the prediction is based on a “forest” of decision trees, rather than relying on one particular tree. This may work well on high dimension data sets like the one present in this paper. To further improve testing performance, I will conduct a grid search on the tuning parameters including the number of trees to use and number of variables to consider in each tree. A three-fold cross-validation method with 20% validation data is utilized to improve model performance and prevent overfit. The grid is restricted to a 3-by-2 setting, with three folds, to reduce the time cost of performing each model.

## Feedforward Neural Network
Taking a step further in improving predictability, this paper will explore the performance of a simple feedforward neural network with two hidden layers, ReLU activation, and 20% dropout regularization to maximize performance and avoid overfitting. The large dropout fraction is due to the large amount of predictors available to us, which results in large number of neurons. The ADAM optimizer is utilized to control the learning rate and cross entropy for loss function. In addition, the explanatory variables are transformed using standard scaler to control the complexity during model fitting.

## Performance Analysis
To analyze the model performance, I will use the accuracy metric, since this is a classification problem. I will compare the training vs testing performance to judge if there is overfitting. Cross Validation may also be utilized depending on the methods above.

# Result
## Predictive Performance
Based on the testing performance, Random Forest models with 15 trees and using log2 of the amount of predictors is better than 10 trees with all of the predictors for the severity levels two, three, and five. For the severity level of four, reducing the number of features used at each note to the fraction of log2 of total predictors also improves testing performance slightly. 

Note that due to time constraint, the lasso logistic regression was forced to use a maximum of 50 iterations, which resulted in early stopping and convergence could be stopped abruptly. 

For the KNN model selection process, tuning parameters are chosen based on the test accuracies, and the visualization is present in the ‘analysis’ code but due to time constraint, not included in this report.

### Table 1: Predictive perforamnce on the models
![Table 1](https://drive.google.com/uc?export=view&scale.option=fit&id=1nOz2F5ICR0ut1rGHR7ZXHXI8O9Zpgez4)
### Figure 3a: Training accuracy across models
![Figure 3a](https://drive.google.com/uc?export=view&scale.option=fit&id=1xWSGLsVgaqGligT9VSSvxzvNyY7ogwbS)
### Figure 3b: Testing accuracy across models (typo on axis)
![Figure 3b](https://drive.google.com/uc?export=view&scale.option=fit&id=1RAXMzwzsrjNOFApu6HdB5Ut-6b-smvkJ)
In general, the models are all performing quite well for severity levels four and five, which means that we can leverage the models to accurately predict if an accident is ‘fatal’, or if an accident is ‘serious’.

We observe that the models have highly similar performance for severity levels four and five. For severity level of three and four, Random Forest outperformances the other algorithms in both the training and testing accuracy. Aligning with the related literature, k-th nearest neighbor also provides relatively well prediction accuracy. The higher prediction accuracy also suggests that these algorithms are more generalisable and prevents overfitting.

Comparing the accuracy for predicting severity levels two and three, we notice that logistic regression is slightly worse than the other algorithms. However, it is not dramatically worse than the others, so we can still benefit from the interpretability of logistic regressions to assess the effects of the variables.

## Interpretations
Due to the difficulties in interpreting k-th nearest neighbor models, we will mainly focus on interpreting the other models. Since the KNN does not outperform the other models significantly in our analysis, this should not affect our analysis by a large portion.

We will utilize the logistic regressions to evaluate the variable coefficients, and examine the feature importance for Random Forest and Neural Network.
 
### Table 2: Logistic Regression Coefficients on the above four variables
![Table 2](https://drive.google.com/uc?export=view&scale.option=fit&id=12qBauVmX0aaxZmqAnWV4bxhCBqefSdjZ)

### Cannabis
The estimated coefficient estimates for the Cannabis legalization paramater is varying between positive and negative values across severity levels. For the severity levels with consistent coefficient signs, we observe that Cannabis legalization decreases the chance of level one accidents, but
increases the chance of level five accidents. The former could be a time trend we observe in the previous sections. The latter aligns with the fact that Cannabis legalization could increase the chance of impaired driving, which is more likely to cause fatal accidents. However, the estimated coefficients are extremely small, and since the results are not highly consistent, we are not confident in concluding much relationship between Cannabis legalization and traffic severity level.

### GDP
The result for GDP is consistent across models for the same severity levels. We notice that an increase in GDP leads to a decrease in the chance of the accident being severity level two to five, which aligns with previous literature. GDP increase does increase the chance of level one accident, but this is reasonable since we can think of it as a downgrade of severity level.

### Employment Rate
For employment rate, the result indicates that a drop in employment rate increases the chance of most severity levels except level two, with an exception of lasso 3. A possible explanation could be that a decrease in employment leads to lower satisfaction and happiness levels, which leads to more anxiety and affecting driving abilities.

### Alcohol Consumption
The result for alcohol consumption does not reflect an increase in the chance of high severity accidents for increase in alcohol consumption, which may be due to the fact that our alcohol consumption data are not precise enough to capture the true time trend.

### Important Features
By examining the top 15 important features in each of the best Random Forest models, we obtain Table 3, which describes the number of times a feature belonged to the top 15 list.

We notice that the most important variables are employment rate, cars involved, speed limit, GDP, lane position, year, and alcohol consumption (volume_ppc). This suggests a heavier focus should be put in identifying relationships between these variables and traffic accidents in future researches.  Notice that except for Cannabis legalization, the above four variables of interest are all important, which suggests a potential underlying relationship.

### Table 3: Frequency of variable being in Top 15 variable in best RF models across
severity levels
![Table 3](https://drive.google.com/uc?export=view&scale.option=fit&id=1n5tmbAV-q9289ZAgahjVYcqGGxnPKYuD)

Figure 4 visualizes the top 15 important features in the best Random Forests for severity levels four and five. The important feature is measured by the decrease in Gini impurity related to that feature. Here we observe that employment rate is most important in prediction of these two severity levels.

![Figure 4a](https://drive.google.com/uc?export=view&scale.option=fit&id=1MWjqxCG21VM-putxJaUHSDWK2xaPQPDz)
![Figure 4b](https://drive.google.com/uc?export=view&scale.option=fit&id=1uCuGduKJ2Xc7Od90t7cDnYBApJbP9blV)
Although the Neural Network did not provide a surprisingly great performance for both training and testing data sets, we observe the feature importance using the SHAP package and SHAP value. Notice that employment rate, GDP, and year are still important variables. Surprisingly, the Neural Networks considered Cannabis legalization as a somewhat important feature in predicting high severity levels. However, this could be mistakenly used as a time variable to indicate the post Cannabis legalization period, which is affected by other time events.

### Figure 5a: Feature importance of Neural Network for Severity 4
![Figure 5a](https://drive.google.com/uc?export=view&scale.option=fit&id=1f9tzSgONm23zo6C3571Tzky5aQpTDMqD)
### Figure 5b: Feature importance of Neural Network for Severity 5
![Figure 5b](https://drive.google.com/uc?export=view&scale.option=fit&id=1fMreItBpYuQuO_E-zIY58M38NNFk8juo)

# Conclusion
In conclusion, this paper extended work done in previous literature that examines the relationship between traffic accidents and related factors by focusing the analysis in the region of Quebec, Canada.

In terms of model performance, we observe similar performance for low severity level and high severity levels. For the severity levels of ‘Property Damage’ and ‘Light injuries’, Random Forest outperforms the other models significantly both in the training and testing accuracies. This illustrates the Random Forest classifier’s advantage in preventing overfitting through bagging techniques. Aligning with previous research, KNN provides second best performance, but its poor interpretability characteristic affected the extent to which we can discuss its results. 

For the factors that affects traffic severity level, we have found employment rate, GDP, speed limits, cars involved, year, and others to be important in predicting severity levels in Random Forest and Neural Network models. Utilizing the results from logistic regression, we observe a roughly inverse relationship between GDP and severity, and inverse relationship between employment rate and severity. However, the coefficients are small and the practical significance is left to be analyzed.

Lastly, I did not find confidential evidence between traffic severity level and Cannabis legalization or alcohol consumption. For the latter, since only a Boolean variable is included to represent Cannabis legalization, the variable could be used as simple time cut-off in fitting the models and mislead the coefficients.

# Limitations
Due to the time constraint of this paper, models had to be fitted with a lot of time cost considerations. Hence, for future work, more tuning parameters could be considered and more time spent modelling. The result from this paper does provide insightful guidance in areas to be examined in the future, such that once again it is shown that traffic severity level is related to employment rate, GDP, and a general time trend.

# Bibliography
- Akinyemi, Y. (2020). Relationship between economic development and road traffic crashes and
casualties: Empirical evidence from Nigeria. Transportation Research Procedia, 48, 218–232.
https://doi.org/10.1016/j.trpro.2020.08.017
- Canada, H. (2022, October 26). Government of Canada. Canada.ca. Retrieved March 20,
2023, from https://www.canada.ca/en/health-canada/programs/engaging-cannabis-legalizationregulation-
canada-taking-stock-progress/document.html#a12
- Données Québec. (Auguest 25, 2022). Rapports d’accident [Dataset]. Retrieved from
https://www.donneesquebec.ca/recherche/dataset/rapports-d-accident
- Farmer, C. M., Monfort, S. S., & Woods, A. N. (2022). Changes in traffic crash rates after
legalization of marijuana: Results by crash severity. Journal of Studies on Alcohol and Drugs,
83(4), 494–501. https://doi.org/10.15288/jsad.2022.83.494
- Government of Canada, Statistics Canada. (2023, February 28). Gross domestic product
(GDP) at basic prices, by industry, monthly. Retrieved March 20, 2023, from
https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=3610043401&pickMembers%5B0%5D=2.1&pickMembers%
- Government of Canada, Statistics Canada. (2023, April 14). Gross domestic product,
expenditure-based, provincial and territorial, annual (x 1,000,000). Retrieved April 14, 2023, from
https://www150.statcan.gc.ca/t1/tbl1/en/tv.action?pid=3610022201
- Iranitalab, A., & Khattak, A. (2017). Comparison of four statistical and machine learning
methods for crash severity prediction. Accident Analysis & Prevention, 108, 27–36.
https://doi.org/10.1016/j.aap.2017.08.008
- Rebecca L Hartman, Marilyn A Huestis, Cannabis Effects on Driving Skills, Clinical Chemistry,
Volume 59, Issue 3, 1 March 2013, Pages 478–492, https://doi.org/10.1373/clinchem.2012.194381
- Sales of alcoholic beverages of liquor authorities, wineries and breweries, by value and volume, fiscal
years ended March 31. Open Government Portal. (2015, May 4). Retrieved March 20, 2023, from
https://open.canada.ca/data/en/dataset/c5004d74-90f3-49eb-82b8-30394d00913b
- Statistics Canada. (2023, February 24). Table 10-10-0010-02 Per capita sales of alcoholic beverages
by liquor authorities and other retail outlets, by value, volume, and absolute volume. Retrieved
March 21, 2023, from https://doi.org/10.25318/1010001001-eng
- Statistics Canada. (2023, February 24). Table 14-10-0287-01 Labour force characteristics,
monthly, seasonally adjusted and trend-cycle, last 5 months. Retrieved March 21, 2023, from
https://doi.org/10.25318/1410028701-eng
- Transport Canada. (2022, February 1). Canadian Motor Vehicle Traffic Collision Statistics:
2020. Transport Canada. Retrieved March 20, 2023, from https://tc.canada.ca/en/roadtransportation/
statistics-data/canadian-motor-vehicle-traffic-collision-statistics-2020
- Transport Canada. (2022, February 23). National Collision Database. Open Government Portal.
Retrieved March 21, 2023, from https://open.canada.ca/data/en/dataset/1eb9eba7-71d1-
4b30-9fb1-30cbdab7e63a
- Packages and programming scikit-learn (sklearn) package: Pedregosa, F., Varoquaux, G.,
Gramfort, A., Michel, V., Thirion, B., Grisel, O., … & Vanderplas, J. (2011). Scikit-learn: Machine
21
learning in Python. Journal of Machine Learning Research, 12(Oct), 2825-2830.
- SHAP package: Lundberg, S. M., & Lee, S. I. (2017). A unified approach to interpreting model
predictions. In Advances in Neural Information Processing Systems (pp. 4765-4774).
- Pandas package: McKinney, W. (2010). Data structures for statistical computing in Python. In
Proceedings of the 9th Python in Science Conference (pp. 51-56).
- NumPy package: Harris, C. R., Millman, K. J., van der Walt, S. J., Gommers, R., Virtanen, P.,
Cournapeau, D., … & Oliphant, T. E. (2020). Array programming with NumPy. Nature, 585(7825),
357-362.
- Python: Python Software Foundation. (2022). Python Language Reference, version 3.10. Available
at https://docs.python.org/3/reference/index.html
- Matplotlib package: Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. IEEE Annals
of the History of Computing, 9(03), 90-95.

# Appendix
Visit the PDF [here](https://drive.google.com/file/d/1y1Cw7qyFzACdRQAkRlcCJoZMLUKSKomc/view?usp=sharing)
