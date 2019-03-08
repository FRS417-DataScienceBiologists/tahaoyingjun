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
No, because the number is zero.

3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?

Yes. They are represented by -999 in the AFR, gestation, litter_size, litter_yr, mass, mass_life, newborn, wean_mass, weaning, and p75 colume. Maybe the scientist didn't get these data.


4. Compared to the msleep data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.

```r
life_history %>% 
  select(species, order)
```

```
## # A tibble: 1,440 x 2
##    species       order       
##    <chr>         <chr>       
##  1 americana     Artiodactyla
##  2 nasomaculatus Artiodactyla
##  3 melampus      Artiodactyla
##  4 buselaphus    Artiodactyla
##  5 clarkei       Artiodactyla
##  6 lervia        Artiodactyla
##  7 marsupialis   Artiodactyla
##  8 cervicapra    Artiodactyla
##  9 bison         Artiodactyla
## 10 bonasus       Artiodactyla
## # ... with 1,430 more rows
```

```r
life_history %>% 
  group_by(order) %>% 
  summarize(n_total=n())
```

```
## # A tibble: 17 x 2
##    order          n_total
##    <chr>            <int>
##  1 Artiodactyla       161
##  2 Carnivora          197
##  3 Cetacea             55
##  4 Dermoptera           2
##  5 Hyracoidea           4
##  6 Insectivora         91
##  7 Lagomorpha          42
##  8 Macroscelidea       10
##  9 Perissodactyla      15
## 10 Pholidota            7
## 11 Primates           156
## 12 Proboscidea          2
## 13 Rodentia           665
## 14 Scandentia           7
## 15 Sirenia              5
## 16 Tubulidentata        1
## 17 Xenarthra           20
```



5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.

```r
life_history <- 
  life_history %>% 
  na_if(-999.0)
```


```r
life_history %>%
  mutate(lifespan=max_life) %>% 
  group_by(order) %>%
  summarize(min=min(lifespan, na.rm=TRUE),
            max=max(lifespan, na.rm=TRUE),
            mean=mean(lifespan, na.rm=TRUE),
            sd=sd(lifespan, na.rm=TRUE),
            total=n())
```

```
## # A tibble: 17 x 6
##    order            min   max  mean    sd total
##    <chr>          <dbl> <dbl> <dbl> <dbl> <int>
##  1 Artiodactyla      81   732 248.   92.4   161
##  2 Carnivora         60   672 253.  113.    197
##  3 Cetacea          156  1368 586.  332.     55
##  4 Dermoptera       210   210 210   NaN       2
##  5 Hyracoidea       132   147 140.   10.6     4
##  6 Insectivora       14   156  46.2  34.8    91
##  7 Lagomorpha        72   216 108.   46.2    42
##  8 Macroscelidea     36   105  68.2  28.7    10
##  9 Perissodactyla   252   600 426.  122.     15
## 10 Pholidota        240   240 240   NaN       7
## 11 Primates         106   726 309.  132.    156
## 12 Proboscidea      840   960 900    84.9     2
## 13 Rodentia          12   420  83.8  63.6   665
## 14 Scandentia        32   149 106.   64.6     7
## 15 Sirenia          150   876 518   363.      5
## 16 Tubulidentata    288   288 288   NaN       1
## 17 Xenarthra        108   480 255.  119.     20
```

6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

```r
life_history %>% 
  group_by(order) %>% 
  summarise(mean_gestation=mean(gestation, na.rm = TRUE),
            mean_newborn=mean(newborn, na.rm = TRUE),
            mean_weaning=mean(wean_mass)) %>% 
  arrange(desc(mean_gestation))
```

```
## # A tibble: 17 x 4
##    order          mean_gestation mean_newborn mean_weaning
##    <chr>                   <dbl>        <dbl>        <dbl>
##  1 Proboscidea             21.3      99523.             NA
##  2 Perissodactyla          13.0      27015.             NA
##  3 Cetacea                 11.8     343077.             NA
##  4 Sirenia                 10.8      22878.             NA
##  5 Hyracoidea               7.4        231.             NA
##  6 Artiodactyla             7.26      7082.             NA
##  7 Tubulidentata            7.08      1734            6250
##  8 Primates                 5.47       287.             NA
##  9 Xenarthra                4.95       314.             NA
## 10 Carnivora                3.69      3657.             NA
## 11 Pholidota                3.63       276.             NA
## 12 Dermoptera               2.75        35.9            NA
## 13 Macroscelidea            1.91        24.5            NA
## 14 Scandentia               1.63        12.8            NA
## 15 Rodentia                 1.31        35.5            NA
## 16 Lagomorpha               1.18        57.0            NA
## 17 Insectivora              1.15         6.06           NA
```
Proboscidea has the longest mean gestation.


