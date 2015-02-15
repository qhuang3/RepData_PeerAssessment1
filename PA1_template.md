# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
raw_activity <- read.csv("activity.csv",header=TRUE)
activity <- raw_activity[complete.cases(raw_activity),]
```


## What is mean total number of steps taken per day?

```r
daily_total <- aggregate(x=activity$steps, by=list(date=activity$date), FUN="sum")
summary(daily_total)
```

```
##          date          x        
##  2012-10-02: 1   Min.   :   41  
##  2012-10-03: 1   1st Qu.: 8841  
##  2012-10-04: 1   Median :10765  
##  2012-10-05: 1   Mean   :10766  
##  2012-10-06: 1   3rd Qu.:13294  
##  2012-10-07: 1   Max.   :21194  
##  (Other)   :47
```

```r
daily_total$date <- strptime(daily_total$date,"%Y-%m-%d ")
plot(daily_total$date, daily_total$x,type="h",main="Total steps by date", xlab="Date",ylab="Total Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
cat("Mean: ",mean(daily_total$x))
```

```
## Mean:  10766.19
```

```r
cat("Median: ",median(daily_total$x))
```

```
## Median:  10765
```

## What is the average daily activity pattern?

```r
daily_avg <- aggregate(x=activity$steps, by=list(interval=activity$interval), FUN="sum")
plot(daily_avg$interval, daily_avg$x,type="l",main="Average steps by 5 minutes interval", xlab="Interval",ylab="Avg Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

```r
cat("Maximum number of step ",daily_avg[order(-daily_avg$x),][1,2])
```

```
## Maximum number of step  10927
```

```r
cat("at interval ",daily_avg[order(-daily_avg$x),][1,1])
```

```
## at interval  835
```

## Imputing missing values

```r
for(i in 1:nrow(daily_avg)){
row <- daily_avg[i,]
interval <- row$interval
steps <- row$x/24
raw_activity[raw_activity$interval==interval & is.na(raw_activity$steps),1] <- steps
}

new_daily_total <- aggregate(x=raw_activity$steps, by=list(date=raw_activity$date), FUN="sum")
new_daily_total$date <- strptime(new_daily_total$date,"%Y-%m-%d ")
plot(new_daily_total$date, new_daily_total$x,type="h",main="Total steps by date with Imputing values", xlab="Date",ylab="Total Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 
