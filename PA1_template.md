# Reproducible Research: Peer Assessment 1


```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

## Loading and preprocessing the data

The first step is to load and preprocess the data.  Unzip the file to begin:


```r
unzip("activity.zip")
```

Then, load the data into a data frame call `activity`:


```r
activity <- read.csv("activity.csv")
```

Check the data frame to see if we need any processing or transformation:


```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

It looks like the date column is stored as a factor, not a date.  Let's convert that to a date and group the data by date in a new data frame.  This used the `dplyr` package that is loaded without displaying to the screen.


```r
activity$date <- as.Date(activity$date)

activity_by_date <- group_by(activity, date)
```

## What is mean total number of steps taken per day?

First, let's create a variable, `total_steps`,  holding the total number of steps taken on each day.  We also rename the column header on the second column.


```r
total_steps <- summarize(activity_by_date, sum(steps, na.rm = TRUE))
names(total_steps)[2] <- "sum"

total_steps
```

```
## # A tibble: 61 x 2
##          date   sum
##        <date> <int>
## 1  2012-10-01     0
## 2  2012-10-02   126
## 3  2012-10-03 11352
## 4  2012-10-04 12116
## 5  2012-10-05 13294
## 6  2012-10-06 15420
## 7  2012-10-07 11015
## 8  2012-10-08     0
## 9  2012-10-09 12811
## 10 2012-10-10  9900
## # ... with 51 more rows
```

Here is a histogram of the total steps each day:


```r
hist(total_steps$sum, xlab = "Total Steps on Each Day", main = "Histogram of Total Steps on Each Day")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

The mean number of steps each day is:


```r
mean(total_steps$sum)
```

```
## [1] 9354.23
```

The median number of steps each day is:


```r
median(total_steps$sum)
```

```
## [1] 10395
```

## What is the average daily activity pattern?

Let's return to the original data frame and group on 5-minute interval this time.  Again, this uses the `dplyr` package.


```r
activity_by_interval <- group_by(activity, interval)
```

Now, put the mean for each interval into a new variable, `mean_steps` and rename the mean column:


```r
mean_steps <- summarize(activity_by_interval, mean(steps, na.rm = TRUE))
names(mean_steps)[2] <- "mean"
```

Let's plot this variable in a time series to see how activity fluctuates throughout the day:


```r
plot(mean_steps$interval, mean_steps$mean, type = "l", xlab = "5-Minute Interval", ylab = "Mean Number of Steps Across All Days", main = "Mean Number of Steps at 5-Minute Intervals")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
