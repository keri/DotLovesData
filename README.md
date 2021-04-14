# DotLovesData

# Purpose

To gain insight into two datasets the well being of NZ communities in Auckland based on their location, deprivation score, and crime data. Can we use this data to identify major predictors for productivity in the region denoted as GMP (gross metropolitan product) per capita which is a measure of goods and services produced within a metropolitan statistical area per person over a period of 
time.

# Questions to ask:

1) How does the GMP and deprivation score change over time per area ?
2) Does this correlate with changes in crime rates?
3) Can we identify major indicators of GMP per capita with the data we have?

# What to do with NA values

There are several ways to deal with null values and I wanted to understand which null values there were and what communities they came from (to see if taking the mean was appropriate). A particular question I had in mind when looking at null values was there enough information available to infer a value and if I deleted those rows, what information would we lose.

There were 1302 values missing for Dep_Index in the community dataset only so I explored those a bit more understand what data we did have for these people. I took a look at what information was there for these 1302 communities and found that they also didn't have information for any community data but did have information for crime data so I visualized crime data.

This is a graph of crime incidence per capita per crime type in each area. I calculated the incidence rate by dividing # of incidences by population:

![Crime Rate By Type of Crime Per Area for NA dataset](/visuals/NANDataCrimeDataByTALB.png)

As compared to this same information for the remaining data without NA values. I also wanted to understand if the deprivation index followed the crime rate for when looking at the different TALB areas. And in fact, that is the case for many communities but not all. Overall, my conclusion was the crimes rates were quite low in most communities so wouldn't provide enough information to infer data for the 1302 rows of data so I decided to delete them from the dataset.

![Crime Rate By Type of Crime Per Area](/visuals/CleanedCrimeDataByTALBandDepIndexMean.png)


## Where were these null values:




# How does GMP change over time

The dataset was too large to visualize all regions (AU codes) at once so I calculated the CHANGE of deprivation score and GMP over the dates they were recorded

