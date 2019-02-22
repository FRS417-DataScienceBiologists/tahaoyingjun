---
title: "Heming HW5"
output: 
  html_document: 
    keep_md: yes
---



1. Load the data.

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


2. Use your preferred function to have a look. Do you notice any problems?

```r
summary(life_history)
```

```
##     order              family             Genus          
##  Length:1440        Length:1440        Length:1440       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##    species               mass             gestation      
##  Length:1440        Min.   :     -999   Min.   :-999.00  
##  Class :character   1st Qu.:       50   1st Qu.:-999.00  
##  Mode  :character   Median :      403   Median :   1.05  
##                     Mean   :   383577   Mean   :-287.25  
##                     3rd Qu.:     7009   3rd Qu.:   4.50  
##                     Max.   :149000000   Max.   :  21.46  
##     newborn             weaning          wean mass       
##  Min.   :   -999.0   Min.   :-999.00   Min.   :    -999  
##  1st Qu.:   -999.0   1st Qu.:-999.00   1st Qu.:    -999  
##  Median :      2.6   Median :   0.73   Median :    -999  
##  Mean   :   6703.1   Mean   :-427.17   Mean   :   16049  
##  3rd Qu.:     98.0   3rd Qu.:   2.00   3rd Qu.:      10  
##  Max.   :2250000.0   Max.   :  48.00   Max.   :19075000  
##       AFR            max. life       litter size        litters/year     
##  Min.   :-999.00   Min.   :-999.0   Min.   :-999.000   Min.   :-999.000  
##  1st Qu.:-999.00   1st Qu.:-999.0   1st Qu.:   1.000   1st Qu.:-999.000  
##  Median :   2.50   Median :-999.0   Median :   2.270   Median :   0.375  
##  Mean   :-408.12   Mean   :-490.3   Mean   : -55.634   Mean   :-477.141  
##  3rd Qu.:  15.61   3rd Qu.: 147.2   3rd Qu.:   3.835   3rd Qu.:   1.155  
##  Max.   : 210.00   Max.   :1368.0   Max.   :  14.180   Max.   :   7.500
```
There are a lot of -999.

3. There are NA's. How are you going to deal with them?

I am going to replace the -999 with NA.

```r
life_history <- life_history%>%
  na_if("-999")
```


4. Where are the NA's? This is important to keep in mind as you build plots.

They are in the volumes of "mass", "gestation", "newbor", "weaning", "wean_mass", "AFR", "max_life", "litter_size", and "litters_yr".

5. Some of the variable names will be problematic. Let's rename them here as a final tidy step.

```r
life_history_tidy <- life_history %>%
  dplyr::rename(genus = Genus,
         wean_mass = `wean mass`,
         max_life = `max. life`,
         litter_size = `litter size`,
         litters_yr = `litters/year`) 
```
6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship.

```r
life_history_tidy %>% 
  select(newborn, gestation)
```

```
## # A tibble: 1,440 x 2
##    newborn gestation
##      <dbl>     <dbl>
##  1   3246.      8.13
##  2   5480       9.39
##  3   5093       6.35
##  4  10167.      7.9 
##  5     NA       6.8 
##  6   3810       5.08
##  7   3910       5.72
##  8   3846       5.5 
##  9  20000       8.93
## 10  23000.      9.14
## # ... with 1,430 more rows
```

```r
life_history_tidy
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti~ Antil~ Anti~ americ~ 4.54e4      8.13   3246.    3         8900
##  2 Arti~ Bovid~ Addax nasoma~ 1.82e5      9.39   5480     6.5         NA
##  3 Arti~ Bovid~ Aepy~ melamp~ 4.15e4      6.35   5093     5.63     15900
##  4 Arti~ Bovid~ Alce~ busela~ 1.50e5      7.9   10167.    6.5         NA
##  5 Arti~ Bovid~ Ammo~ clarkei 2.85e4      6.8      NA    NA           NA
##  6 Arti~ Bovid~ Ammo~ lervia  5.55e4      5.08   3810     4           NA
##  7 Arti~ Bovid~ Anti~ marsup~ 3.00e4      5.72   3910     4.04        NA
##  8 Arti~ Bovid~ Anti~ cervic~ 3.75e4      5.5    3846     2.13        NA
##  9 Arti~ Bovid~ Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti~ Bovid~ Bison bonasus 5.00e5      9.14  23000.    6.6         NA
## # ... with 1,430 more rows, and 4 more variables: AFR <dbl>,
## #   max_life <dbl>, litter_size <dbl>, litters_yr <dbl>
```

```r
life_history_tidy %>% 
  ggplot(aes(x=newborn, y=gestation))+
  geom_point()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Heming_HW5_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn't talk about this in lab, but you can use scale_x_log10() as a layer to correct for this issue. This will log transform the y-axis values.



```r
life_history_tidy %>%
  mutate(newborn_log=log10(newborn)) %>% 
  ggplot(aes(x=newborn_log, y=gestation))+
  geom_point()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Heming_HW5_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


8. Now that you have the basic plot, color the points by taxonomic order.

```r
life_history_tidy %>%
  mutate(newborn_log=log10(newborn)) %>% 
  ggplot(aes(x=newborn_log, y=gestation, color=order))+
  geom_point()
```

```
## Warning: Removed 673 rows containing missing values (geom_point).
```

![](Heming_HW5_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

9. Lastly, make the size of the points proportional to body mass.


```r
life_history_tidy %>%
  mutate(newborn_log=log10(newborn)) %>% 
  ggplot(aes(x=newborn_log, y=gestation, size=mass))+
  geom_point()
```

```
## Warning: Removed 691 rows containing missing values (geom_point).
```

![](Heming_HW5_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


10. Make a plot that shows the range of lifespan by order.


```r
life_history_tidy %>%
  ggplot(aes(x = reorder(order, -max_life), y = max_life))+
  geom_bar(stat="identity")+
  theme(axis.text.x = element_text(angle=60, hjust=1))
```

```
## Warning: Removed 841 rows containing missing values (position_stack).
```

![](Heming_HW5_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

