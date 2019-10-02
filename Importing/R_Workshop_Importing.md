---
title: "Importing Data in R Studio"
output:
  slidy_presentation:
    keep_md: true
---



## Introduction

Hi everyone. By popular demand, we are going to focus our workshop today on importing datsets from various formats. 

I thought it would be fun to play with this data on college majors from Kieran Healy and the NCES: https://github.com/kjhealy/nces-degrees

Okay, now where did you download it to? And what's your working directory? 

I will show you the way that I manage R Studio Projects and working directories using R Studio's graphical user interface. While using the GUI will earn you snickers from hard-core R users, it's not a problem in my opinion as long as you record what you did in your script. That way going forward I can just run my script rather than use the GUI over and over again. More importantly this is crucial to making your work transparent and reproducible. 

R has its own data format called .Rdata. This is most similar to Stata's .dta. But most R users use the .csv format for their data. I will show you how you can open .csv files in R using R Studio's GUI.

## Reading MS Excel Files
I like using the GUI for this because it allows me to preview the data and make sure that it is going to import the way I expect. I pay particular attention to missing data formats to make sure those -99s, NAs, or NaNs are identified as missing and not numbers or text strings. R Studio also chooses a package that it best thinks can read the file format and translates my preferences into the appropriate commands for that package. It then generates the code and sends it to R. I copied and pasted the code below.


```r
tabn322_10_clean <-read.csv("tabn322_10_clean.csv")
```

If you are working in Excel and don't want to be bothered to export your worksheet as a .csv, you can also directly read in .xls files. Again, you can do this using the GUI or with code.


```r
library(readxl)
tabn322_10 <- read_excel("tabn322.10.xls", 
    na = "empty")
```

```
## New names:
## * `` -> ...2
## * `` -> ...3
## * `` -> ...4
## * `` -> ...5
## * `` -> ...6
## * … and 13 more problems
```
You can see that the result was not quite as clean. We might want to reassign the first row as the column names and knock out the rows that don't contain data.

##Data Cleaning

```r
names(tabn322_10) <- tabn322_10[1,]
```

```
## Warning: Must use a character vector as names.
## This warning is displayed once per session.
```

```r
tabn322_10<-tabn322_10[-c(1,2, 43, 44), ] 
tabn322_10<-tabn322_10[!is.na(tabn322_10$`Field of study`),]
tabn322_10$`Field of study`<-gsub("[\\.]","", tabn322_10$`Field of study`)
```

Couldn't I have done all of that in Excel? Yes, and maybe even more quickly the first time. But now I have a reproducible track record of what I did and can almost certainly do it faster next time than if I were tidying up manually in Excel.

## Exporting Data
You can also export data frames into a format of your choice. So, after cleaning up the Excel spreadsheet I imported, I can export it back out as a .csv or a .tab.


```r
write.csv(tabn322_10, "Clean_Data.csv")
write.table(tabn322_10, "Clean_Data.tab")
```

It is also possible to export directly to .xls or .xlsx, though it requires an external package and I can't think of a really good reason for doing so. Exporting as a .csv and opening it up in Excel is a more reliable option.


```r
#install.packages("writexl")
library(writexl)
write_xlsx(tabn322_10, "Clean_Data.xlsx")
```

##Interacting with Stata
You might, however, have a delinquent friend or colleague who uses Stata. You can export your data as a .dta file too!


```r
names(tabn322_10)<-c("fieldofstudy","y1970","y1975", "y1980", "y1985", "y1990", "y1995", "y2000", "y2005", "y2006", "y2007", "y2008", "y2009", "y2010", "y2011", "y2012", "y2013", "y2014", "y2015")
library(haven)
write_dta(tabn322_10, "Clean_Data.dta")
```

Oof, re-writing all of those column names to conform with Stata variable name requirement labels was a pain. Something to think about in advance if you know you are going to be jumping back and forth or if you think a Stata user might be using your dataset in the future.

A more frequent occurance may be that you want to import someone else's replication data that is in a Stata format. This looks very similar to all the other imports. Pay attention to Stata's NaN format for missing data!


```r
Clean_Data <- read_dta("Clean_Data.dta")
```



