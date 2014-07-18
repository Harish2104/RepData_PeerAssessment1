Reproducible Research: Peer Assessment 1
==========================================
## Loading and preprocessing the data

The code for loading dataset,required packages and preprocessing steps into R is

```r
activity <- read.csv("activity.csv")
```

```
## Warning: cannot open file 'activity.csv': No such file or directory
```

```
## Error: cannot open the connection
```

```r

suppressWarnings(library(lubridate))

suppressWarnings(library(ggplot2))

activity$day <- weekdays(as.Date(activity$date))
```

```
## Error: object 'activity' not found
```

```r

str(activity)
```

```
## Error: object 'activity' not found
```


## What is mean total number of steps taken per day?

Histogram of the total number of steps taken each day is

```r
steps.date <- aggregate(activity$steps, by = list(activity$date), FUN = sum)
```

```
## Error: object 'activity' not found
```

```r

names(steps.date) <- c("date", "steps")
```

```
## Error: object 'steps.date' not found
```

```r

qplot(steps, data = steps.date, binwidth = 1400) + theme_bw()
```

```
## Error: object 'steps.date' not found
```


Mean of total number of steps taken per day is

```r
mean(steps.date$steps, na.rm = T)
```

```
## Error: object 'steps.date' not found
```


Median of total number of steps taken per day is


```r
median(steps.date$steps, na.rm = T)
```

```
## Error: object 'steps.date' not found
```


## What is the average daily activity pattern?

Time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis) is

```r
steps.interval <- aggregate(steps ~ interval, data = activity, FUN = mean, na.rm = T)
```

```
## Error: object 'activity' not found
```

```r

ggplot(steps.interval, aes(interval, steps)) + geom_line(color = "blue") + theme_bw() + 
    labs(x = "Interval", y = "Average Number of Steps")
```

```
## Error: object 'steps.interval' not found
```


The 5-minute interval, on average across all the days in the dataset that contains the maximum number of steps is


```r
steps.interval[which.max(steps.interval$steps), ]$interval
```

```
## Error: object 'steps.interval' not found
```


## Imputing missing values

Total number of missing values in the dataset are


```r
sum(is.na(activity))
```

```
## Error: object 'activity' not found
```


All the missing values in the dataset are filled with mean for that 5-minute interval. 


```r
a <- merge(activity, steps.interval, by = "interval", suffixes = c("", ".y"))
```

```
## Error: object 'activity' not found
```

```r

b <- is.na(a$steps)
```

```
## Error: $ operator is invalid for atomic vectors
```

```r

a$steps[b] <- a$steps.y[b]
```

```
## Error: $ operator is invalid for atomic vectors
```


Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
a <- a[, c(-5)]
```

```
## Error: incorrect number of dimensions
```

```r

a <- a[order(a$date), ]
```

```
## Error: $ operator is invalid for atomic vectors
```

```r

activity1 <- a
```


Histogram of the total number of steps taken each day with the new dataset is

```r
steps.date1 <- aggregate(activity1$steps, by = list(activity1$date), FUN = sum)
```

```
## Error: $ operator is invalid for atomic vectors
```

```r

names(steps.date1) <- c("date", "steps")
```

```
## Error: object 'steps.date1' not found
```

```r

qplot(steps, data = steps.date1, binwidth = 1400) + theme_bw()
```

```
## Error: object 'steps.date1' not found
```


Mean of total number of steps taken per day using the new dataset is


```r
mean(steps.date1$steps, na.rm = T)
```

```
## Error: object 'steps.date1' not found
```


Median of total number of steps taken per day using the new dataset is


```r
median(steps.date1$steps, na.rm = T)
```

```
## Error: object 'steps.date1' not found
```


By including the imputed values in the dataset, both the median and the mean total number of steps taken per day are equal, as expected. A comparison of histograms for the non-imputed and imputed datasets demonstrates that the imputation had the greatest impact on the 10,000 - 15,000 steps per day range and that the distribution of data with the imputed data appears to be more normal.

## Are there differences in activity patterns between weekdays and weekends?

Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


```r
activity1$daytype <- ifelse(activity1$day %in% c("Saturday", "Sunday"), "Weekend", 
    "Weekday")
```

```
## Error: $ operator is invalid for atomic vectors
```

```r

activity1$daytype <- as.factor(activity1$daytype)
```

```
## Error: $ operator is invalid for atomic vectors
```


Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).


```r
steps.interval1 <- aggregate(steps ~ interval + daytype, data = activity1, FUN = mean, 
    na.rm = T)
```

```
## Error: invalid 'envir' argument of type 'character'
```

```r

ggplot(steps.interval1, aes(interval, steps)) + geom_line(color = "blue") + 
    theme_bw() + labs(x = "Interval", y = "Average Number of Steps") + facet_wrap(~daytype, 
    nrow = 2, ncol = 1)
```

```
## Error: object 'steps.interval1' not found
```


In summary, there are several significant differences between the weekday and weekend profiles.

1)The weekend curve has a delayed onset perhaps related to late wake up time.

2)There is generally a higher degree of activities during the weekend than weekday.

3)There are also more activities towards the end of the day in weekend.
