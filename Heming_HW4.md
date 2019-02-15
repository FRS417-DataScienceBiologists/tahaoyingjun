---
title: "Heming HW4"
output: 
  html_document: 
    keep_md: yes
---



#Load the tidyverse

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

```r
life_history <- readr::read_csv("mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```
Rename some of the variables. Notice that I am replacing the old life_history data.

```r
life_history <- life_history %>% 
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
life_history
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti~ Antil~ Anti~ americ~ 4.54e4      8.13   3246.    3         8900
##  2 Arti~ Bovid~ Addax nasoma~ 1.82e5      9.39   5480     6.5       -999
##  3 Arti~ Bovid~ Aepy~ melamp~ 4.15e4      6.35   5093     5.63     15900
##  4 Arti~ Bovid~ Alce~ busela~ 1.50e5      7.9   10167.    6.5       -999
##  5 Arti~ Bovid~ Ammo~ clarkei 2.85e4      6.8    -999  -999         -999
##  6 Arti~ Bovid~ Ammo~ lervia  5.55e4      5.08   3810     4         -999
##  7 Arti~ Bovid~ Anti~ marsup~ 3.00e4      5.72   3910     4.04      -999
##  8 Arti~ Bovid~ Anti~ cervic~ 3.75e4      5.5    3846     2.13      -999
##  9 Arti~ Bovid~ Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti~ Bovid~ Bison bonasus 5.00e5      9.14  23000.    6.6       -999
## # ... with 1,430 more rows, and 4 more variables: AFR <dbl>,
## #   max_life <dbl>, litter_size <dbl>, litters_yr <dbl>
```
1. Explore the data using the method that you prefer. Below, I am using a new package called skimr. If you want to use this, make sure that it is installed.


```r
library("skimr")
```

```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## -- Variable type:character ----------------------
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## -- Variable type:numeric ------------------------
##     variable missing complete    n      mean         sd   p0  p25     p50
##          AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##    gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##  litter_size       0     1440 1440    -55.63     234.88 -999    1    2.27
##   litters_yr       0     1440 1440   -477.14     500.03 -999 -999    0.38
##         mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max_life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##      newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##    wean_mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##      weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
##      p75          p100     hist
##    15.61     210       <U+2586><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587><U+2581>
##     4.5       21.46    <U+2583><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##     3.83      14.18    <U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##     1.15       7.5     <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##  7009.17       1.5e+08 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##   147.25    1368       <U+2587><U+2581><U+2581><U+2583><U+2582><U+2581><U+2581><U+2581>
##    98    2250000       <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##    10          1.9e+07 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##     2         48       <U+2586><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
```

2. Run the code below. Are there any NA's in the data? Does this seem likely?

```r
msleep %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```
3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?

4. Compared to the msleep data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.

5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.

6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?


