---
title: "RChandna_Week5_HW"
author: "Rajat Chandna"
date: "June 11, 2018"
output: 
  html_document:
    keep_md: true
---




```r
library(dplyr)
```

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

```r
sessionInfo()
```

```
## R version 3.5.0 (2018-04-23)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 10 x64 (build 17134)
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=English_United States.1252 
## [2] LC_CTYPE=English_United States.1252   
## [3] LC_MONETARY=English_United States.1252
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] dplyr_0.7.4
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.16     digest_0.6.15    rprojroot_1.3-2  assertthat_0.2.0
##  [5] R6_2.2.2         backports_1.1.2  magrittr_1.5     evaluate_0.10.1 
##  [9] pillar_1.2.2     rlang_0.2.0      stringi_1.1.7    bindrcpp_0.2.2  
## [13] rmarkdown_1.9    tools_3.5.0      stringr_1.3.1    glue_1.2.0      
## [17] yaml_2.1.19      compiler_3.5.0   pkgconfig_2.0.1  htmltools_0.3.6 
## [21] bindr_0.1.1      knitr_1.20       tibble_1.4.2
```

# Questions  

\   
**Backstory**: Your client is expecting a baby soon.  However, he is not sure what to name the child.  Being out of the loop, he hires you to help him figure out popular names.  He provides for you raw data in order to help you make a decision.

\    

#### 1.	Data Munging (30 points): Utilize yob2016.txt for this question. This file is a series of popular children's names born in the year 2016 in the United States.  It consists of three columns with a first name, a gender, and the amount of children given that name.  However, the data is raw and will need cleaning to make it tidy and usable.

\     

\    

##### a.	First, import the .txt file into R so you can process it.  Keep in mind this is not a CSV file.  You might have to open the file to see what you're dealing with.  Assign the resulting data frame to an object, df, that consists of three columns with human-readable column names for each.
\    


```r
df <- read.delim2("Data/yob2016.txt", sep=";", stringsAsFactors = FALSE, header = FALSE, col.names = c("Name", "Gender", "No_Of_Children_Given_This_Name_2016"))
dim(df)
```

```
## [1] 32869     3
```

\     

##### b.	Display the summary and structure of df.
\     


```r
str(df)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ Name                               : chr  "Emma" "Olivia" "Ava" "Sophia" ...
##  $ Gender                             : chr  "F" "F" "F" "F" ...
##  $ No_Of_Children_Given_This_Name_2016: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
summary(df)
```

```
##      Name              Gender          No_Of_Children_Given_This_Name_2016
##  Length:32869       Length:32869       Min.   :    5.0                    
##  Class :character   Class :character   1st Qu.:    7.0                    
##  Mode  :character   Mode  :character   Median :   12.0                    
##                                        Mean   :  110.7                    
##                                        3rd Qu.:   30.0                    
##                                        Max.   :19414.0
```
\    

##### c.	Your client tells you that there is a problem with the raw file.  One name was entered twice and misspelled.  The client cannot remember which name it is; there are thousands he saw! But he did mention he accidentally put three y's at the end of the name.  Write an R command to figure out which name it is and display it.
\     


```r
# Find yyy at the end of the Name
df[grep("yyy$", df$Name), "Name"]
```

```
## [1] "Fionayyy"
```

\  

##### d.	Upon finding the misspelled name, please remove this particular observation, as the client says it's redundant.  Save the remaining dataset as an object: y2016 
\    


```r
index <- grep("yyy$", df$Name)
y2016 <- df[-index,]
str(y2016)
```

```
## 'data.frame':	32868 obs. of  3 variables:
##  $ Name                               : chr  "Emma" "Olivia" "Ava" "Sophia" ...
##  $ Gender                             : chr  "F" "F" "F" "F" ...
##  $ No_Of_Children_Given_This_Name_2016: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
# Saving the Object( if the intent is to actually save it in file). Subset DF is already placed in y2016 object
saveRDS(y2016, file = "Data/y2016_DF.rds")
```

\     


#### 2.	Data Merging (30 points): Utilize yob2015.txt for this question.  This file is similar to yob2016, but contains names, gender, and total children given that name for the year 2015.  

\  

##### a.	Like 1a, please import the .txt file into R.  Look at the file before you do.  You might have to change some options to import it properly.  Again, please give the dataframe human-readable column names.  Assign the dataframe to y2015.  

\    


```r
y2015 <- read.delim2("Data/yob2015.txt", sep=",", stringsAsFactors = FALSE, header = FALSE, col.names = c("Name", "Gender", "No_Of_Children_Given_This_Name_2015"))
dim(y2015)
```

```
## [1] 33063     3
```

\   

##### b.	Display the last ten rows in the dataframe.  Describe something you find interesting about these 10 rows.
\     


```r
tail(y2015, 10)
```

```
##         Name Gender No_Of_Children_Given_This_Name_2015
## 33054   Ziyu      M                                   5
## 33055   Zoel      M                                   5
## 33056  Zohar      M                                   5
## 33057 Zolton      M                                   5
## 33058   Zyah      M                                   5
## 33059 Zykell      M                                   5
## 33060 Zyking      M                                   5
## 33061  Zykir      M                                   5
## 33062  Zyrus      M                                   5
## 33063   Zyus      M                                   5
```

```r
# Extra Exploration of Data
str(y2015)
```

```
## 'data.frame':	33063 obs. of  3 variables:
##  $ Name                               : chr  "Emma" "Olivia" "Sophia" "Ava" ...
##  $ Gender                             : chr  "F" "F" "F" "F" ...
##  $ No_Of_Children_Given_This_Name_2015: int  20415 19638 17381 16340 15574 14871 12371 11766 11381 10283 ...
```

```r
summary(y2015)
```

```
##      Name              Gender          No_Of_Children_Given_This_Name_2015
##  Length:33063       Length:33063       Min.   :    5.0                    
##  Class :character   Class :character   1st Qu.:    7.0                    
##  Mode  :character   Mode  :character   Median :   11.0                    
##                                        Mean   :  111.4                    
##                                        3rd Qu.:   30.0                    
##                                        Max.   :20415.0
```
\    

**The interesting thing about last 10 rows is that all last 10 names belong to gender male and each name(of last 10) is given exactly to 5 children in year 2015.**
\    

##### c.	Merge y2016 and y2015 by your Name column; assign it to final.  The client only cares about names that have data for both 2016 and 2015; there should be no NA values in either of your amount of children rows after merging.

\    


```r
# Since NAs are to be avoided, Not Using all = true Option 
final <- merge(x=y2015, y=y2016, by = union("Name", "Gender"))
#Shortening Names of Columns
colnames(final) <- c("Name", "Gender", "GivenIn2015", "GivenIn2016")
summary(final)
```

```
##      Name              Gender           GivenIn2015       GivenIn2016     
##  Length:26550       Length:26550       Min.   :    5.0   Min.   :    5.0  
##  Class :character   Class :character   1st Qu.:    8.0   1st Qu.:    8.0  
##  Mode  :character   Mode  :character   Median :   16.0   Median :   15.0  
##                                        Mean   :  137.2   Mean   :  135.5  
##                                        3rd Qu.:   41.0   3rd Qu.:   41.0  
##                                        Max.   :20415.0   Max.   :19414.0
```

```r
# Make Sure No NAs in dataset
if (any(is.na(final)) == TRUE){
  print("The dataset contains NA values")
}else{
  print("No NA values are found in result of is.na function ")
}
```

```
## [1] "No NA values are found in result of is.na function "
```

```r
print(paste("Are All Observations complete: ", as.character(all(complete.cases(final)), sep="")))
```

```
## [1] "Are All Observations complete:  TRUE"
```

\    

#### 3.	Data Summary (30 points): Utilize your data frame object final for this part.

\     

\    

##### a.	Create a new column called "Total" in final that adds the amount of children in 2015 and 2016 together.  In those two years combined, how many people were given popular names?
\    


```r
final$Total <- final$GivenIn2015 + final$GivenIn2016
final <- dplyr::arrange(final, desc(Total))
print(paste("In the Two Years Combined, the number of people that were given popular names are: ", as.character(sum(final$Total)), sep=""))
```

```
## [1] "In the Two Years Combined, the number of people that were given popular names are: 7239231"
```

\     

##### b.	Sort the data by Total.  What are the top 10 most popular names?
\    


```r
final <- dplyr::arrange(final, desc(Total))
print("10 Most popularNames, in order are: ")
```

```
## [1] "10 Most popularNames, in order are: "
```

```r
for(i in 1:10){
  print(paste("At No.", as.character(i), "is", final[i,"Name"], sep=" "))
}
```

```
## [1] "At No. 1 is Emma"
## [1] "At No. 2 is Olivia"
## [1] "At No. 3 is Noah"
## [1] "At No. 4 is Liam"
## [1] "At No. 5 is Sophia"
## [1] "At No. 6 is Ava"
## [1] "At No. 7 is Mason"
## [1] "At No. 8 is William"
## [1] "At No. 9 is Jacob"
## [1] "At No. 10 is Isabella"
```
\    


##### c.	The client is expecting a girl!  Omit boys and give the top 10 most popular girl's names.
\    


```r
final_fem <- final[final$Gender == "F", ]
final_fem <- dplyr::arrange(final_fem, desc(Total))
str(final_fem)
```

```
## 'data.frame':	15267 obs. of  5 variables:
##  $ Name       : chr  "Emma" "Olivia" "Sophia" "Ava" ...
##  $ Gender     : chr  "F" "F" "F" "F" ...
##  $ GivenIn2015: int  20415 19638 17381 16340 15574 14871 11381 12371 11766 10283 ...
##  $ GivenIn2016: int  19414 19246 16070 16237 14722 14366 13030 11699 10926 10733 ...
##  $ Total      : int  39829 38884 33451 32577 30296 29237 24411 24070 22692 21016 ...
```

```r
print("10 Most popular Girl's Names, in order are: ")
```

```
## [1] "10 Most popular Girl's Names, in order are: "
```

```r
for(i in 1:10){
  print(paste("At No.", as.character(i), "is", final_fem[i,"Name"], sep=" "))
}
```

```
## [1] "At No. 1 is Emma"
## [1] "At No. 2 is Olivia"
## [1] "At No. 3 is Sophia"
## [1] "At No. 4 is Ava"
## [1] "At No. 5 is Isabella"
## [1] "At No. 6 is Mia"
## [1] "At No. 7 is Charlotte"
## [1] "At No. 8 is Abigail"
## [1] "At No. 9 is Emily"
## [1] "At No. 10 is Harper"
```
\    

##### d.	Write these top 10 girl names and their Totals to a CSV file.  Leave out the other columns entirely.
\    


```r
final_fem <- dplyr::arrange(final_fem, desc(Total))
write.csv2(head(final_fem,10)[,c("Name", "Total")], file = "Data/Top10GirlsNamesFor2015-16.csv")
```
\    


#### 4.	Upload to GitHub (10 points): Push at minimum your RMarkdown for this homework assignment and a Codebook to one of your GitHub repositories (you might place this in a Homework repo like last week).  The Codebook should contain a short definition of each object you create, and if creating multiple files, which file it is contained in.  You are welcome and encouraged to add other files-just make sure you have a description and directions that are helpful for the grader.

\  

Raw Link
https://github.com/R-Chandna/SMU_MSDS_HomeWork/tree/master/MSDS%206306_Doing_Data_Science/Week5

[Click Here To Follow the Link to My SMU HomeWork Repo Week 5 HW Folder](https://github.com/R-Chandna/SMU_MSDS_HomeWork/tree/master/MSDS%206306_Doing_Data_Science/Week5)

\    






