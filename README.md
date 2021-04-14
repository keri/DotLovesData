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

## Where were these null values:




# How does GMP change over time

The dataset was too large to visualize all regions (AU codes) at once so I calculated the CHANGE of deprivation score and GMP over the dates they were recorded

