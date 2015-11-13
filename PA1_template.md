# Reproducible Research: Peer Assessment 1
Martin HEIN (m#)  
2. November 2015  

*****
# PROLOGUE
It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the "quantified self" movement -- a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

## Data
The data for this assignment can be downloaded from the course web site:

Dataset: [Activity monitoring data [52K]][dataset]


The variables included in this dataset are:

| variable | description |
|:---------|:------------|
| steps | Number of steps taking in a 5-minute interval (missing values are coded as NA) |
| date | The date on which the measurement was taken in YYYY-MM-DD format |
| interval | Identifier for the 5-minute interval in which measurement was taken |

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

## Assignment
This assignment will be described in multiple parts. You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a single R markdown document that can be processed by knitr and be transformed into an HTML file.

Throughout your report make sure you always include the code that you used to generate the output you present. When writing code chunks in the R markdown document, always use _echo = TRUE_ so that someone else will be able to read the code. This assignment will be evaluated via peer assessment so it is essential that your peer evaluators be able to review the code for your analysis.

For the plotting aspects of this assignment, feel free to use any plotting system in R (i.e., base, lattice, ggplot2).

Fork/clone the [GitHub repository created for this assignment][github]. You will submit this assignment by pushing your completed files into your forked repository on GitHub. The assignment submission will consist of the URL to your GitHub repository and the SHA-1 commit ID for your repository state.

_**NOTE: The GitHub repository also contains the dataset for the assignment so you do not have to download the data separately.**_

### Loading and preprocessing the data
Show any code that is needed to

#. Load the data (i.e. read.csv())
#. Process/transform the data (if necessary) into a format suitable for your analysis

### What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

#. Calculate the total number of steps taken per day
#. If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day
#. Calculate and report the mean and median of the total number of steps taken per day

### What is the average daily activity pattern?
#. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
#. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

### Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

#. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)
#. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
#. Create a new dataset that is equal to the original dataset but with the missing data filled in.
#. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

### Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

#. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.
#. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

## Submitting the Assignment
To submit the assignment:

#. Commit the your completed PA1_template.Rmd file to the master branch of your git repository (you should already be on the master branch unless you created new ones)
#. Commit your PA1_template.md and PA1_template.html files produced by processing your R markdown file with knit2html() function in R (from the knitr package) by running the function from the console.
#. If your document has figures included (it should) then they should have been placed in the figure/ directory by default (unless you overrided the default). Add and commit the figure/directory to yoru git repository so that the figures appear in the markdown file when it displays on github.
#. Push your master branch to GitHub.
#. Submit the URL to your GitHub repository for this assignment on the course web site.

In addition to submitting the URL for your GitHub repository, you will need to submit the 40 character SHA-1 hash (as string of numbers from 0-9 and letters from a-f) that identifies the repository commit that contains the version of the files you want to submit. You can do this in GitHub by doing the following

#. Going to your GitHub repository web page for this assignment
#. Click on the "?? commits" link where ?? is the number of commits you have in the repository. For example, if you made a total of 10 commits to this repository, the link should say "10 commits".
#. You will see a list of commits that you have made to this repository. The most recent commit is at the very top. If this represents the version of the files you want to submit, then just click the ?copy to clipboard? button on the right hand side that should appear when you hover over the SHA-1 hash. Paste this SHA-1 hash into the course web site when you submit your assignment. If you don't want to use the most recent commit, then go down and find the commit you want and copy the SHA-1 hash.

A valid submission will look something like (this is just an example!)

    https://github.com/rdpeng/RepData_PeerAssessment1
    7c376cc5447f11537f8740af8e07d6facc3d9645

# PREPARE FOR WORK
## Setup the environment
Before displaying any data or plots, we have to setup the analysis environment by loading all the required libraries, and defining any global constants or objects.


```r
## load required libraries
library(R.utils) 
library(RCurl)
library(httr)
library(XML)
library(rvest)
library(gdata)
library(data.table)
library(dplyr)
library(tidyr)
library(lubridate)
library(zoo)
library(grid)
library(ggplot2)

## global constants/objects
dlmeth <- (if(.Platform$OS.type == "windows") "auto" else "curl")
dldir <- "."

inDatRmtZip <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
inDatLocZip <- file.path(dldir, "repdata%2Fdata%2Factivity.zip")
inDatLoc <- file.path(dldir, "activity.csv")

## save current working directory
odir <- getwd()
```

## Download and unpack the source data
Next we will obtain the source data from the corresponding internet site, and unpack the zip archive, but only in the event of lacking a local copy of the unpacked source data (file ./activity.csv).


```r
## download source data only in case not already available locally
if (!file.exists(inDatLoc)) {
    
    ## download source data
    if (!file.exists(inDatLocZip)) {
        try(download.file(inDatRmtZip, inDatLocZip, method=dlmeth), silent=TRUE)
        
        if (file.exists(inDatLoc)) unlink(inDatLoc)
    } # if
    
    ## unpack source data
    if (file.exists(inDatLocZip) && (!file.exists(inDatLoc)))
        unzip(inDatLocZip, exdir=dldir)
} # if
```

_**Note**_  
_Once the source data have been downloaded and unpacked, the local copy will be used for any subsequent _knitting_. In order to re-unpack the data, remove file ./activity.csv, and in order to re-download the source data from the internet (including the subsequent unpacking), delete file ./repdata%2Fdata%2Factivity.zip._

# GET FAMILIAR WITH THE SOURCE DATA
## Load the source data
After successfully downloading and unpacking the source date, it is getting loaded into a data.table.


```r
## load source data into data.table
if (file.exists(inDatLoc)) {
    dtAct <- fread(inDatLoc)
} else {
    dtAct <- NULL
} # if/else
```

## Explore the source data
As a final step during initalisation, let's have a quick peek at the source data.


```r
## explore first few observations of the source data
if (!is.null(dtAct)) head(dtAct)
```

```
##    steps       date interval
## 1:    NA 2012-10-01        0
## 2:    NA 2012-10-01        5
## 3:    NA 2012-10-01       10
## 4:    NA 2012-10-01       15
## 5:    NA 2012-10-01       20
## 6:    NA 2012-10-01       25
```


```r
## explore last few observations of the source data
if (!is.null(dtAct)) tail(dtAct)
```

```
##    steps       date interval
## 1:    NA 2012-11-30     2330
## 2:    NA 2012-11-30     2335
## 3:    NA 2012-11-30     2340
## 4:    NA 2012-11-30     2345
## 5:    NA 2012-11-30     2350
## 6:    NA 2012-11-30     2355
```

Ideally the first few observations of the source date should match with regards to the structure the last few observations. Talking structure, let's have a closer look at the source data's structure.


```r
## explore the structure source data
if (!is.null(dtAct)) str(dtAct)
```

```
## Classes 'data.table' and 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  - attr(*, ".internal.selfref")=<externalptr>
```

In order to get some idea of the value ranges of the various variables, let us draw up a quick summary.


```r
## quick summary of the source data
if (!is.null(dtAct)) summary(dtAct)
```

```
##      steps            date              interval     
##  Min.   :  0.00   Length:17568       Min.   :   0.0  
##  1st Qu.:  0.00   Class :character   1st Qu.: 588.8  
##  Median :  0.00   Mode  :character   Median :1177.5  
##  Mean   : 37.38                      Mean   :1177.5  
##  3rd Qu.: 12.00                      3rd Qu.:1766.2  
##  Max.   :806.00                      Max.   :2355.0  
##  NA's   :2304
```

Having accomplished this, we are now set and ready for any further processing, and/or to start answering potential questions at hand.

# PRE-PROCESSING OF THE DATA SET
For convenience purposes, and before answering the first question, we will add a new variable _**date.time**_ to the data set, combining the variables _**date**_ and _**interval**_. This is feasible due to the fact that each individual interval represents a specific time of day, starting at _0_ (= _00:00_), ranging all the way up to _2355_ (= _23:55_).

We also will create a second new variable _**weekday**_ pointing to the respective day of the week, associated with each date, ranging from _1_ (= _Sunday_) up to _7_ (= _Saturday_).

Thirdly, derived from the _**weekday**_ variable, a new variable _**daytype**_ will be created, indicating whether a particular day is a day of the _**weekend**_ (ie. _Saturday_, _Sunday_) or a "regular" _**weekday**_ (_Monday_ to _Friday_).


```r
## create date/time, weekday, and daytype variable
if (!is.null(dtAct)) {
    dtAct[, date.time := ymd_hm(paste(date, sprintf("%04d", interval)))]
    dtAct[, weekday := as.factor(wday(date))]
    dtAct[which(weekday %in% c(1, 7)), daytype := "weekend"]
    dtAct[which(!(weekday %in% c(1, 7))), daytype := "weekday"]
    dtAct[, daytype := as.factor(daytype)]
    print(dtAct)
} # if
```

```
##        steps       date interval           date.time weekday daytype
##     1:    NA 2012-10-01        0 2012-10-01 00:00:00       2 weekday
##     2:    NA 2012-10-01        5 2012-10-01 00:05:00       2 weekday
##     3:    NA 2012-10-01       10 2012-10-01 00:10:00       2 weekday
##     4:    NA 2012-10-01       15 2012-10-01 00:15:00       2 weekday
##     5:    NA 2012-10-01       20 2012-10-01 00:20:00       2 weekday
##    ---                                                              
## 17564:    NA 2012-11-30     2335 2012-11-30 23:35:00       6 weekday
## 17565:    NA 2012-11-30     2340 2012-11-30 23:40:00       6 weekday
## 17566:    NA 2012-11-30     2345 2012-11-30 23:45:00       6 weekday
## 17567:    NA 2012-11-30     2350 2012-11-30 23:50:00       6 weekday
## 17568:    NA 2012-11-30     2355 2012-11-30 23:55:00       6 weekday
```

# QUESTIONS TO ADDRESS
## What is mean total number of steps taken per day?
As a first step we will calculate the total number of steps taken per day (any missing values will be ignored).


```r
## determine the total number of steps taken each day
stpsDay <- NULL

if (!is.null(dtAct)) {
    stpsDay <- dtAct[, sum(steps, na.rm=TRUE), by=.(date)]
    setnames(stpsDay, c("date", "total.steps.per.day"))
} # if
```

In order to translate the textual data into a chart, a corresponding plot is getting created (albeit not printed at this point in time).


```r
## create the plot
stpsPlt <- NULL

if (!is.null(stpsDay)) {
    stpsRng <- with(stpsDay, range(total.steps.per.day, na.rm=TRUE))
    binWdth <- 2000
    stpsPlt <- ggplot() +
               aes(x=stpsDay[, total.steps.per.day], fill=..count..) +
               geom_histogram(binwidth=binWdth) #+
               #geom_density(na.rm=TRUE) #+
               #geom_rug(aes(stpsDay$total.steps.per.day), sides="b")
} # if
```

As a next step the mean and median values are getting determined and added to the plot.


```r
## calculate the mean and median value
stpsMn <- with(stpsDay, mean(total.steps.per.day, na.rm=TRUE))
stpsMd <- with(stpsDay, median(total.steps.per.day, na.rm=TRUE))

## add these two to the plot
if (!is.null(stpsPlt)) {
    stpsPlt <- stpsPlt +
               geom_vline(aes(xintercept=stpsMn, shape="mean"), linetype=2, show_guide=TRUE) +
               geom_vline(aes(xintercept=stpsMd, shape="median"), linetype=3, show_guide=TRUE) +
               scale_shape_manual(name="", values=c("x", "x")) +
               guides(shape=guide_legend(override.aes=list(linetype=c(2, 3), shape = c(NA, NA))))
} # if
```

Another important topic to keep in mind is setting the corresponding title and labels.


```r
## set/add labels & text
if (!is.null(stpsPlt)) {
    stpsPlt <- stpsPlt +
               scale_x_continuous(name="steps per day") +
               scale_y_continuous(name="frequency") +
               scale_fill_continuous(name="frequency") +
               ggtitle("Total number of steps taken per day")
} # if
```

After having added all these individual components to our plot, we are now ready to finally print the plot created.

![](PA1_template_files/figure-html/total_steps_plot_output-1.png) 

_(Mean value: 9354.23, median: 1.0395\times 10^{4})_

## What is the average daily activity pattern?
Let's start by calculating the average number of steps taken per interval, averaged across all days (any missing values will be ignored).


```r
## determine the average number of steps taken each day, averaged across all days
stpsInt <- NULL

if (!is.null(dtAct)) {
    stpsInt <- dtAct[, mean(steps, na.rm=TRUE), by=.(interval)]
    setnames(stpsInt, c("interval", "avg.steps.per.interval"))
} # if
```

In order to translate the textual data into a chart, a corresponding plot is getting created (albeit not printed at this point in time).


```r
## create the plot
tsPlt <- NULL

if (!is.null(stpsInt)) {
    tsPlt <- ggplot(stpsInt) +
             aes(x=interval, y=avg.steps.per.interval) +
             geom_line()
} # if
```

As a next step the maximum average number of steps along with the corresponding interval are getting determined and added to the plot.


```r
## determine the maximum average number of steps
stpsAvgMax <- stpsInt[which.max(avg.steps.per.interval), avg.steps.per.interval]
intAvgMax <- stpsInt[which.max(avg.steps.per.interval), interval]

## add it to the plot
if (!is.null(tsPlt)) {
    tsPlt <- tsPlt +
             geom_vline(aes(xintercept=intAvgMax, shape="maxavg"), linetype=2, show_guide=TRUE) +
             scale_shape_manual(name="", values=c("x")) +
             guides(shape=guide_legend(override.aes=list(linetype=c(2), shape = c(NA))))
} # if
```

Another important topic to keep in mind is setting the corresponding title and labels.


```r
## set/add labels & text
if (!is.null(tsPlt)) {
    tsPlt <- tsPlt +
             scale_x_continuous(name="interval") +
             scale_y_continuous(name="steps per interval") +
             ggtitle("Average number of steps taken per interval")
} # if
```

After having added all these individual components to our plot, we are now ready to finally print the plot created.

![](PA1_template_files/figure-html/average_steps_plot_output-1.png) 

_(Maximum average number of steps per day 206.17 at interval 835)_

## Imputing missing values
Looking at the given data set, one can observe that there are _**17568**_ row(s) or observations in total, whereof _**15264**_ observations are considered to be complete, ie. no value is missing in any of these rows, and _**2304**_ observations where at least one value is missing.

These numbers can be determined by facilitating the _**```complete.case()```**_ function of the _**stats**_ package as shown below.


```r
## determine the number of rows/observations (total, complete, incomplete)
obsTotal <- NULL
obsComplete <- NULL
obsIncomplete <- NULL

if (!is.null(dtAct)) {
    obsTotal <- nrow(dtAct)
    obsComplete <- sum(complete.cases(dtAct))
    obsIncomplete <- sum(!complete.cases(dtAct))
} # if
```

So the number of incomplete cases is amounting to 13.11% of all observations available.

Another interesting aspect of the data set is the number of steps greater than zero (4250), as well as the number of steps equal to zero (11014).


```r
## determine the number of rows/observations with steps equal to zero, or greater then zero
obsNonzero <- NULL
obsZero <- NULL

if (!is.null(dtAct)) {
    obsNonzero <- length(which(dtAct$steps > 0))
    obsZero <- length(which(dtAct$steps == 0))
} # if
```

So the number of non-zero values for steps is amounting to 24.19% of all observations available, and the number of zero value to 62.69%.

There are various approaches viable, when considering filling the gaps, and replacing the missing values with approximated/interpolated ones. The R package _**zoo**_ provides two functions for this purpose: _**```na.approx()```**_, and _**```na.spline()```**_.


```r
## replace missing values with approximated/interpolated ones
dtActNew <- NULL

if (!is.null(dtAct)) {
    # create a copy of the original data set
    dtActNew <- copy(dtAct)
    
    # replace the missing values by linear approximation
    #dtActNew[, steps := na.approx(steps, na.rm=TRUE, rule=2)]
    
    # replace the missing values by cubic spline interpolation
    dtActNew[, steps := na.spline(steps, na.rm=TRUE)]
    
    # set any negative number of steps to 0
    dtActNew[which(dtActNew$steps < 0), steps := 0]
} # if
```



Let us take a look at the number of steps greater than zero (6574 or 37.42%), as well as the number of steps equal to zero (10994 or 62.58%).


```r
stpsNewDay <- NULL
stpsNewPlt <- NULL

if (!is.null(dtActNew)) {
    # determine the total number of steps taken each day
    stpsNewDay <- dtActNew[, sum(steps, na.rm=TRUE), by=.(date)]
    setnames(stpsNewDay, c("date", "total.steps.per.day"))

    # calculate the mean and median value
    stpsNewMn <- with(stpsNewDay, mean(total.steps.per.day, na.rm=TRUE))
    stpsNewMd <- with(stpsNewDay, median(total.steps.per.day, na.rm=TRUE))
    
    # create the plot
    stpsNewRng <- with(stpsNewDay, range(total.steps.per.day, na.rm=TRUE))
    binWdth <- 2000
    stpsNewPlt <- ggplot() +
                  aes(x=stpsNewDay[, total.steps.per.day], fill=..count..) +
                  geom_histogram(binwidth=binWdth) +
                  #geom_density(na.rm=TRUE) #+
                  #geom_rug(aes(stpsNewDay$total.steps.per.day), sides="b")
                  geom_vline(aes(xintercept=stpsNewMn, shape="mean"), linetype=2, show_guide=TRUE) +
                  geom_vline(aes(xintercept=stpsNewMd, shape="median"), linetype=3, show_guide=TRUE) +
                  scale_shape_manual(name="", values=c("x", "x")) +
                  guides(shape=guide_legend(override.aes=list(linetype=c(2, 3), shape = c(NA, NA)))) +
                  scale_x_continuous(name="steps per day") +
                  scale_y_continuous(name="frequency") +
                  scale_fill_continuous(name="frequency") +
                  ggtitle("Total number of steps taken per day")
    stpsNewPlt
} # if
```

![](PA1_template_files/figure-html/total_new_steps_per_day-1.png) 

_(Mean value: 9354.23, median: 1.0395\times 10^{4})_

Comparing the original values of steps taken with the imputed ones, one can observe that the approximation model used yields almost identical numbers of steps taken.


```
##     value original  imputed   difference differenceP
## 1:   mean  9354.23  9354.23 8.264869e-07           0
## 2: median 10395.00 10395.00 0.000000e+00           0
```

A working hypothesis as why this is the case might lie with the fact that there are significantly more zero and missing values than non-zero ones.

## Are there differences in activity patterns between weekdays and weekends?
In the previous pre-processing section above, the variables (_**weekday**_, _**daytype**_) have already created which will get facilitated in order to address this question.

Again we will start by calculating the average number of steps taken per day and interval, but this time, we will also group it by _**daytype**_.

Having done this, let's determine the maximum average values and the corresponding intervals within each of the _**daytype**_ groups before actually plotting the data.


```r
stpsNewInt <- NULL
tsNewPlt <- NULL

if (!is.null(dtActNew)) {
    # determine the average number of steps taken each day, averaged across all days
    stpsNewInt <- dtActNew[, mean(steps, na.rm=TRUE), by=.(interval, daytype)]
    setnames(stpsNewInt, c("interval", "daytype", "avg.steps.per.interval"))

    # determine the maximum average number of steps
    stpsNewAvgMax <- stpsNewInt[which.max(avg.steps.per.interval), avg.steps.per.interval]
    intNewAvgMax <- stpsNewInt[which.max(avg.steps.per.interval), interval]
    
    stpsNewAvgMaxWD <- stpsNewInt[daytype == "weekday"][which.max(avg.steps.per.interval),
                                                        avg.steps.per.interval]
    intNewAvgMaxWD <- stpsNewInt[daytype == "weekday"][which.max(avg.steps.per.interval),
                                                       interval]
    
    stpsNewAvgMaxWE <- stpsNewInt[daytype == "weekend"][which.max(avg.steps.per.interval),
                                                        avg.steps.per.interval]
    intNewAvgMaxWE <- stpsNewInt[daytype == "weekend"][which.max(avg.steps.per.interval),
                                                       interval]
    
    # create a data.table for the average max indicator per daytype
    stpsNewIntDT <- data.table(daytype=c("weekday", "weekend"), 
                               interval=c(intNewAvgMaxWD, intNewAvgMaxWE),
                               avg.steps.per.interval=c(stpsNewAvgMaxWD, stpsNewAvgMaxWE))
    
    # create the plot
    tsNewPlt <- ggplot(stpsNewInt) +
                aes(x=interval, y=avg.steps.per.interval, colour=daytype) +
                geom_line() +
                facet_grid(daytype ~ .) +
                geom_vline(data=stpsNewIntDT, 
                           aes(xintercept=interval, shape="maxavg", colour=daytype), 
                           linetype=2, show_guide=TRUE) +
                scale_shape_manual(name="", values=c("x")) +
                guides(shape=guide_legend(override.aes=list(linetype=c(2), shape = c(NA)))) +
                guides(colour=FALSE) +
                scale_x_continuous(name="interval") +
                scale_y_continuous(name="steps per interval") +
                ggtitle("Average number of steps taken per interval")
    tsNewPlt
} # if
```

![](PA1_template_files/figure-html/average_new_steps_per_interval-1.png) 

_(Maximum average number of steps per day 202.89 (total)/202.89 (weekday)/153.13 (weekend)) at interval 835 (total)/835 (weekday)/915 (weekend))_

As can be derived from the charts above, daily patterns at the weekend deviate from the ones during the week eg. on weekends people appear to get up at a later time, and also retire to bed later that night. Activities are distributed more evenly across the day, and these activities are also more intensen than during the rest of the week (one hypothesis might point to people sitting more during the day on "regular" weekdays).

# EPILOGUE
In order to support reproducibility of this analysis, let's retrieve some information on the system the analysis was carried out.


```r
## retrieve session/system information
sessionInfo()
```

```
## R version 3.2.1 (2015-06-18)
## Platform: x86_64-apple-darwin13.4.0 (64-bit)
## Running under: OS X 10.11.2 (unknown)
## 
## locale:
## [1] C
## 
## attached base packages:
##  [1] stats4    splines   grid      grDevices datasets  compiler  graphics 
##  [8] stats     utils     methods   base     
## 
## other attached packages:
##  [1] tidyr_0.3.1                   dplyr_0.4.3                  
##  [3] gdata_2.17.0                  rvest_0.3.1                  
##  [5] xml2_0.1.2                    XML_3.98-1.3                 
##  [7] httr_1.0.0                    RCurl_1.95-4.7               
##  [9] bitops_1.0-6                  knitr_1.11                   
## [11] TSA_1.01                      tseries_0.10-34              
## [13] survival_2.38-3               strucchange_1.5-1            
## [15] sandwich_2.3-4                spatial_7.3-11               
## [17] shiny_0.12.2                  rstudio_0.98.1103            
## [19] rpart_4.1-10                  R.utils_2.1.0                
## [21] R.oo_1.19.0                   R.methodsS3_1.7.0            
## [23] quadprog_1.5-5                performanceEstimation_1.0.2  
## [25] PerformanceAnalytics_1.4.3541 xts_0.9-7                    
## [27] zoo_1.7-12                    mvtnorm_1.0-3                
## [29] microbenchmark_1.4-2          mgcv_1.8-9                   
## [31] nlme_3.1-122                  Matrix_1.2-2                 
## [33] manipulate_0.98.1103          lubridate_1.3.3              
## [35] locfit_1.5-9.1                leaps_2.9                    
## [37] lattice_0.20-33               ISOweek_0.6-2                
## [39] ISOcodes_2015.04.04           highr_0.5.1                  
## [41] ggplot2_1.0.1                 foreign_0.8-66               
## [43] evaluate_0.8                  devtools_1.9.1               
## [45] data.table_1.9.6              compare_0.2-6                
## [47] codetools_0.2-14              class_7.3-14                 
## [49] boot_1.3-17                   bit64_0.9-5                  
## [51] bit_1.1-12                    batch_1.1-4                  
## 
## loaded via a namespace (and not attached):
##  [1] gtools_3.5.0     assertthat_0.1   yaml_2.1.13      chron_2.3-47    
##  [5] digest_0.6.8     colorspace_1.2-6 htmltools_0.2.6  httpuv_1.3.3    
##  [9] plyr_1.8.3       xtable_1.8-0     scales_0.3.0     proto_0.3-10    
## [13] magrittr_1.5     mime_0.4         memoise_0.2.1    MASS_7.3-44     
## [17] tools_3.2.1      formatR_1.2.1    stringr_1.0.0    munsell_0.4.2   
## [21] labeling_0.3     rmarkdown_0.8.1  gtable_0.1.2     DBI_0.3.1       
## [25] reshape2_1.4.1   R6_2.1.1         stringi_1.0-1    parallel_3.2.1  
## [29] Rcpp_0.12.1
```

As a final step, all the temporary files and data along the way will be removed.


```r
## restore saved working directory
setwd(odir)

## remove any objects created
rm(list=ls())
```

*****
[dataset]: <https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip>
[github]: <http://github.com/rdpeng/RepData_PeerAssessment1>
