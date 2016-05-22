# Coursera: Practical Machine Learning Assignment: Prediction Assignment Writeup
Jeff Carter [GitHub](https://github.com/jmacarter)  


```
## Run time: 2016-05-21 17:56:16
## R version: R version 3.3.0 (2016-05-03)
```


This document stepwise describes the analysis performed for the prediction assignment of the Coursera's Practical Machine Learning course. This project uses data from the accelerometers of fitness devices of six participants to determine the manner in which they performed a particular exercise. 


Libraries used in the analysis will be loaded first. A seed value is also set at this time.


```r
options(warn=-1)
library(caret)
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```r
library(randomForest)
```

```
## randomForest 4.6-12
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

```r
library(Hmisc)
```

```
## Loading required package: survival
```

```
## 
## Attaching package: 'survival'
```

```
## The following object is masked from 'package:caret':
## 
##     cluster
```

```
## Loading required package: Formula
```

```
## 
## Attaching package: 'Hmisc'
```

```
## The following object is masked from 'package:randomForest':
## 
##     combine
```

```
## The following objects are masked from 'package:base':
## 
##     format.pval, round.POSIXt, trunc.POSIXt, units
```

```r
library(foreach)
library(doParallel)
```

```
## Loading required package: iterators
```

```
## Loading required package: parallel
```

```r
set.seed(2016)
```

The first step is to download and preprocess the training and test data for this project. As noted in Reproducible Research lectures, the download date will also be recorded.

analyze the type & the completion rate of the data. 

```r
# Make a data directory for data files for this assignment, if not present
if (!file.exists("./data"))
{
     dir.create("./data")
}

# Download the training and test data files, if necessary, and record the download date
if (!file.exists("./data/data.zip"))
{
     URL <- "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"
     download.file(URL,"./data/pml-training.csv", mode = "wb")
   
     URL <- "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"
     download.file(URL,"./data/pml-testing.csv", mode = "wb")
   
     # record the download date, as mentioned in lectures
     DownloadDate <- date()
     sink("./data/download_date.txt")
     cat("Date data downloaded: ")
     cat(DownloadDate)
     sink()
}
```
