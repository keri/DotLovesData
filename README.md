# DotLovesData

# Purpose

To gain insight into two datasets the well being of NZ communities in Auckland based on their location, deprivation score, and crime data. Can we use this data to identify major predictors for productivity in the region denoted as GMP (gross metropolitan product) per capita which is a measure of goods and services produced within a metropolitan statistical area per person over a period of 
time. I used python, pandas, matplotlib, and sklearn for all analysis done.

# Questions to ask:

1) What to do with the null values and how is there enough information to infer missing values?
2) What are the changes in GMP per capital and Deprivation Index over time and do they follow the same trend?
3) Can we identify major indicators of GMP per capita with the data we have?

# What to do with NA values

There are several ways to deal with null values and I wanted to understand which null values there were and what communities they came from (to see if taking the mean was appropriate). A particular question I had in mind when looking at null values was there enough information available to infer a value and if I deleted those rows, what information would we lose.

There were 1302 values missing for Dep_Index in the community dataset only so I explored those a bit more understand what data we did have for these people. I took a look at what information was there for these 1302 communities and found that they also didn't have information for any community data but did have information for crime data so I visualized crime data.

This is a graph of crime incidence per capita per crime type in each area. I calculated the incidence rate by dividing # of incidences by population:

![Crime Rate By Type of Crime Per Area for NA dataset](/visuals/NANDataCrimeDataByTALB.png)

As compared to this same information for the remaining data without NA values. I also wanted to understand if the deprivation index followed the crime rate for when looking at the different TALB areas. And in fact, that is the case for many communities but not all. Overall, my conclusion was the crimes rates were quite low in most communities so wouldn't provide enough information to infer data for the 1302 rows of data so I decided to delete them from the dataset. You can see there is a general trend upward for both but I feel that a lot of information is lost when taking a broader approach. I think there is insight to be gained in looking in more detail at the individual communities.

![Crime Rate By Type of Crime Per Area](/visuals/CleanedCrimeDataByTALBandDepIndexMean.png)


# How does GMP and Deprication Index change over time and do they follow the same trend:

After running training a good predictive model, the most important feature was the AU codes so I wanted to look at changes in GMP and DI over time. I noticed that a lot of the time, the GMP and DI went in opposite directions for that time period (meaning the GMP went up with the DI went down and vice versa). Interestingly when looking at the changes using just the TALB which incorporates may AU codes, the trend was the same for both with DI increasing with GMP. This meant that a lot of information is being missed when looking at many regions at once. I wanted to look at the AU codes that showed different directions of change over time so looked at regions in which GMP increased WITH DI decreased for the same period and the other looking at DI increase WITH GMP decrease:

## AU Code Areas Showing An Increase in GMP WITH A Decrease in DI For Same Time Period

There were 283 different regions and 509 dates with these regions to graph. I broke up the graph into 12 subplots and labeled the AU codes. Several of the same labels indicate there are several dates for that code

![GMP increase WITH DI decrease over time for AU codes](/visuals/NegativeDIPositiveGMPOverTime.png)


## AU Code Areas Showing An Decrease in GMP WITH A Increase in DI For Same Time Period

There were 297 different regions and 604 dates with these regions to graph. I broke up the graph into 12 subplots and labeled the AU codes. Several of the same labels indicate there are several dates for that code

![GMP decrease WITH DI increase over time for AU codes](/visuals/PositiveDINegativeGMPOverTime.png)


## GMP v DI Over Time for 24 different TALB regions 

You can see it is quite messy so took a broader overview by lumping codes together into region with the TALB category.

![GMP versus DI over time for TALB](/visuals/GMPvDIOverTimePerTALB.png)


# Can we identify major indicators of GMP per capita with the data we have:

After cleaning the data which I detail below, I trained three different prediction models. The goal was to see if we can reliably predict GMP per capita with the data we had. If so, which predictors are identified as the most important? For all of these models I used libraries from SKlearn and python.

## Scoring:
I kept a holdout set of 20% of the data which didn't see the models. The sd, mean, max, and min for each feature was the same in the holdout set and the training set. I then did a train test split using an 80/20 training/test split.

## Cleaning the data:
-- Merged Datasets:
I merged the crime dataset with the community dataset as there may be some predictors in the crime that can be identified (however, there is very little crime data as per previous graphs but didn't want to lose any information). I changed the datatypes to categorical for the string datatypes and trained three models: Gradient boosted regressor, linear regression, and ridge regression.

-- Deleted Redundant Features:
The TA2018_name and TALB feature contained the same data but TALB having more information with 24 unique areas versus 4 for TA2018_name. I deleted the TA2018_name data.

-- Checked Correlation Between Numerical Features:

There is a bit of correlation between GMP and deprivation index but none between the other features so I kept all of the numerical features.

![correlation heatmap](/visuals/correlationPlot.png)

Each model was scored based on the R squared which is the coefficient of determination which is a measure of the variation in y (GMP in this case) that is explained by the data. A high R squared means you have a good model. I also validated independently with the holdout set and calculated the R squared for the holdout.

-- Categorical Variables:

I created categorical variables for the times for both crime occurences and date DPI was assessed. In addition, TALB, and

## Model Performance

Linear Regression using the default parameters: 

score = .19

holdout score = didn't do


Ridge Regression using default parameters:

score = .19

holdout score = didn't do


Gradient Boosted Regressor:

model score = .7554

holdout score = .7557


## Feature Importance

Based on the good predctions using the gradient boosted regressor, I looked at the feature importances relative to eachother with the model.feature_importance_ function in SKlearn.

The AU code is the biggest factor in predicting GMP which is very interesting, followed by the deprivation index.


![feature importance](/visuals/featureImportances.png)


# Conclusions and Future Analysis

While it is not surprising that Deprivation index and where you live (via the AU2017_code) are major indicators of GMP, crime rate doesn't seem to be as much of a factor. Or at least we can't draw conclusions from the crime data as there isn't enough of it. While, the DI and GMP change together over time when looking at larger areas, I think there is a lot to explore if we were to dig into the individual codes. Specifically identify areas in which the DI went up and GMP went (and visa versa). Is that a mistake in reporting or is there something real there?

Association is interesting and provides a place to start looking, however, the real challenge is to help identify causation (which I'm sure is everyone's goal). 

## Identify factors that are good predictors of GMP and/or DI, comparing them with areas that have gone up in GMP and/or DPI over time (either recently or in the past):

-- School data:
  - decile information
  - government funding for schools

-- Housing data:
  - mean value of houses in each code
  - percent ownership v rental

-- Industry data:
  - industry type
  - mean wage
  - percent employment of population

-- Health data:
  - how many people are getting sick
  - what are their illnesses
  - how serious
  - how much does each illness cost

-- Environmental data:
  - what is the topology (is the location on the ocean, mountainous?)
  - transport ease to and from

These are just a few thoughts that would be fun to explore. The data I used isn't available in this repo as they were too large.



