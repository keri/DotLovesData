# DotLovesData

# Purpose

To gain insight into two datasets the well being of NZ communities in Auckland based on their location, deprivation score, and crime data. Can we use this data to identify major predictors for productivity in the region denoted as GMP (gross metropolitan product) per capita which is a measure of goods and services produced within a metropolitan statistical area per person over a period of 
time.

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

The dataset was too large to visualize all regions (AU codes) at once and the final visual still needs work to be more interpretable. I calculated the CHANGE of deprivation score and GMP from the first and last date for each code and it was a bit of a mess as there are 489 codes. I also calculcated for just the TALB regions, of which there were 24. I wanted to see how the changes correlated with eacother as I noticed that some deprivation indices actually increased while the GMP went down, so I am looking for general trends here:

## GMP v Deprivation Index (DI) Over Time for all 498 different AU codes: GMP in blue and DI in red

![GMP versus DI over time for AU codes](/visuals/GMPvDIOverTimePerAUCode.png)

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


