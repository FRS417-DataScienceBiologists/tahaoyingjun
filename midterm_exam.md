---
title: "Midterm Exam"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc_float: no
  pdf_document:
    toc: yes
---

## Instructions
This exam is designed to show me what you have learned and where there are problems. You may use your notes and anything from the `class_files` folder, but please no internet searches. You have 35 minutes to complete as many of these exercises as possible on your own, and 10 minutes to work with a partner.  

At the end of the exam, upload the complete .Rmd file to your GitHub repository.  

1. Load the tidyverse.

```r
library(tidyverse)
```

```
## -- Attaching packages -------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.2.5
## v tibble  1.4.2     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.3.1
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ----------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


2. For these questions, we will use data about California colleges. Load the `ca_college_data.csv` as a new object called `colleges`.

```r
college<-
  readr::read_csv("data/ca_college_data.csv")
```

```
## Parsed with column specification:
## cols(
##   INSTNM = col_character(),
##   CITY = col_character(),
##   STABBR = col_character(),
##   ZIP = col_character(),
##   ADM_RATE = col_double(),
##   SAT_AVG = col_double(),
##   PCIP26 = col_double(),
##   COSTT4_A = col_double(),
##   C150_4_POOLED = col_double(),
##   PFTFTUG1_EF = col_double()
## )
```


3. Use your preferred function to have a look at the data and get an idea of its structure.

```r
summary(college)
```

```
##     INSTNM              CITY              STABBR         
##  Length:341         Length:341         Length:341        
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##      ZIP               ADM_RATE         SAT_AVG         PCIP26       
##  Length:341         Min.   :0.0807   Min.   : 870   Min.   :0.00000  
##  Class :character   1st Qu.:0.4581   1st Qu.: 985   1st Qu.:0.00000  
##  Mode  :character   Median :0.6370   Median :1078   Median :0.00000  
##                     Mean   :0.5901   Mean   :1112   Mean   :0.01981  
##                     3rd Qu.:0.7461   3rd Qu.:1237   3rd Qu.:0.02458  
##                     Max.   :1.0000   Max.   :1555   Max.   :0.21650  
##                     NA's   :240      NA's   :276    NA's   :35       
##     COSTT4_A     C150_4_POOLED     PFTFTUG1_EF    
##  Min.   : 7956   Min.   :0.0625   Min.   :0.0064  
##  1st Qu.:12578   1st Qu.:0.4265   1st Qu.:0.3212  
##  Median :16591   Median :0.5845   Median :0.5016  
##  Mean   :26685   Mean   :0.5705   Mean   :0.5577  
##  3rd Qu.:39289   3rd Qu.:0.7162   3rd Qu.:0.8117  
##  Max.   :69355   Max.   :0.9569   Max.   :1.0000  
##  NA's   :124     NA's   :221      NA's   :53
```


4. What are the column names?

```r
colnames(college)
```

```
##  [1] "INSTNM"        "CITY"          "STABBR"        "ZIP"          
##  [5] "ADM_RATE"      "SAT_AVG"       "PCIP26"        "COSTT4_A"     
##  [9] "C150_4_POOLED" "PFTFTUG1_EF"
```


5. Are there any NA's in the data? If so, how many are present and in which variables?
INSTNM              CITY              STABBR              ZIP               ADM_RATE     
 Length:341         Length:341         Length:341         Length:341         Min.   :0.0807  
 Class :character   Class :character   Class :character   Class :character   1st Qu.:0.4581  
 Mode  :character   Mode  :character   Mode  :character   Mode  :character   Median :0.6370  
                                                                             Mean   :0.5901  
                                                                             3rd Qu.:0.7461  
                                                                             Max.   :1.0000  
                                                                             NA's   :240     
    SAT_AVG         PCIP26           COSTT4_A     C150_4_POOLED     PFTFTUG1_EF    
 Min.   : 870   Min.   :0.00000   Min.   : 7956   Min.   :0.0625   Min.   :0.0064  
 1st Qu.: 985   1st Qu.:0.00000   1st Qu.:12578   1st Qu.:0.4265   1st Qu.:0.3212  
 Median :1078   Median :0.00000   Median :16591   Median :0.5845   Median :0.5016  
 Mean   :1112   Mean   :0.01981   Mean   :26685   Mean   :0.5705   Mean   :0.5577  
 3rd Qu.:1237   3rd Qu.:0.02458   3rd Qu.:39289   3rd Qu.:0.7162   3rd Qu.:0.8117  
 Max.   :1555   Max.   :0.21650   Max.   :69355   Max.   :0.9569   Max.   :1.0000  
 NA's   :276    NA's   :35        NA's   :124     NA's   :221      NA's   :53      


6. Which cities in California have the highest number of colleges?

```r
college %>% 
  ggplot(aes(x=CITY))+
  geom_bar()+
   theme(axis.text.x = element_text(angle=90, hjust=1))
```

![](midterm_exam_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


7. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest cost?


```r
ggplot(data=college, mapping=aes(x=COSTT4_A, y=CITY))+
 geom_boxplot()
```

```
## Warning: Removed 124 rows containing missing values (stat_boxplot).
```

![](midterm_exam_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


8. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What does this mean?

```r
ggplot(data=college, mapping=aes(x=ADM_RATE, y=C150_4_POOLED))+
 geom_point()
```

```
## Warning: Removed 251 rows containing missing values (geom_point).
```

![](midterm_exam_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

9. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Run the code below and look at the output. Are all of the columns tidy? Why or why not?

```r
univ_calif<-
  college %>%
  filter_all(any_vars(str_detect(.,pattern = "University of California")))
univ_calif
```

```
## # A tibble: 10 x 10
##    INSTNM CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
##    <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Unive~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
##  2 Unive~ Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
##  3 Unive~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
##  4 Unive~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
##  5 Unive~ Davis CA     9561~    0.423    1218  0.198    33904         0.850
##  6 Unive~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
##  7 Unive~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
##  8 Unive~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
##  9 Unive~ San ~ CA     9410~   NA          NA NA           NA        NA    
## 10 Unive~ San ~ CA     9414~   NA          NA NA           NA        NA    
## # ... with 1 more variable: PFTFTUG1_EF <dbl>
```
They are tidy, because each colume has a category.


10. Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".


11. As a final step, remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.


12. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Please use a barplot.


## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
