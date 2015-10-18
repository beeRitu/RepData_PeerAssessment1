---
title: "Reproducible Research: Peer Assignment 1"
author: "Ritu"
date: "October 18, 2015"
output: html_document
---

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

Libraries "dplyr" and "ggplot2" have been used for this assignment. They were included in the R program as follows:


```r
#---Reading the libraries----
library(dplyr)
library(ggplot2)
```

##Loading and preprocessing the data

Data file was located in the current working directory for R. Data was read as a csv file as below:

```r
#----Reading the data----
data<-read.table("activity.csv",header=TRUE,sep=',')
```

##Calculating the mean total number of steps taken per day

For this part of the assignment, NA values have been ignored. Therefore data was filtered to remove NA values.


```r
#Removing the NA values in steps
data_valid<-filter(data,!is.na(data$steps))
```

**Total number of steps taken per day**

To calculate the total number of steps taken per day *group_by* function from dplyr package was used to group the data using the column data and calculate the sum of steps for each date. Mutate function creates the column *daily_step_count* which is filled with the same daily sum value for each row corresponding to the same date.


```r
#calculating total number of steps each day
step_sum_byDay<-data_valid%>%
    group_by(date)%>%
    mutate(daily_step_count=sum(steps))
```

**Histogram of the total number of steps taken each day**

Histogram was plotted using the base plotting system in R.


```r
#plotting the histogram
with(step_sum_byDay,hist(daily_step_count))
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 

**Calculating the mean and median of total number of steps taken per day**

For calculating the mean and median we take the daily sum value for each day and use *mean* and *median* function to perform the computation. Only interval 0 is read for each day because the daily sum value is the same for each row corresponding to the same date.


```r
for_mean_median<-step_sum_byDay%>%
  filter(interval==0)
mean_steps<-mean(for_mean_median$daily_step_count)
median_steps<-median(for_mean_median$daily_step_count)
```
The calculated value of mean of daily step count is 1.0766189 &times; 10<sup>4</sup> and median of daily step count is 10765.

##Average daily activity pattern

**Time series plot** 

Dplyr package is again used to compute the average number of steps taken per interval average across all days as followS:


```r
#average daily activity time series plot
data_valid2<-data_valid%>%
  group_by(interval)%>%
  mutate(step_average=mean(steps))
```
Time series plot is made using the base plotting system in R.

```r
with(data_valid2, plot(interval,step_average,type="l"))
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png) 

**Time interval with maximum number of steps across all days**
Interval with maximum steps is calculating by looking at the corresponding average daily step count maximum.

```r
#5-minute interval with max activity
max_steps<-max(data_valid2$step_average)
max_interval<-filter(data_valid2,step_average==max_steps)
max_interval<-unique(max_interval$interval)
```
The interval found to have maximum daily step count is 835.