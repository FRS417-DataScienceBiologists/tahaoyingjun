---
title: "Heming HW2"
output: 
  html_document: 
    keep_md: yes
---



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
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee~ Acin~ carni Carn~ lc                  12.1      NA        NA    
##  2 Owl ~ Aotus omni  Prim~ <NA>                17         1.8      NA    
##  3 Moun~ Aplo~ herbi Rode~ nt                  14.4       2.4      NA    
##  4 Grea~ Blar~ omni  Sori~ lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667
##  6 Thre~ Brad~ herbi Pilo~ <NA>                14.4       2.2       0.767
##  7 Nort~ Call~ carni Carn~ vu                   8.7       1.4       0.383
##  8 Vesp~ Calo~ <NA>  Rode~ <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn~ domesticated        10.1       2.9       0.333
## 10 Roe ~ Capr~ herbi Arti~ lc                   3        NA        NA    
## # ... with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```
1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.

```r
?msleep
```

```
## starting httpd help server ... done
```
2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.

```r
summary(msleep)
```

```
##      name              genus               vore          
##  Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##     order           conservation        sleep_total      sleep_rem    
##  Length:83          Length:83          Min.   : 1.90   Min.   :0.100  
##  Class :character   Class :character   1st Qu.: 7.85   1st Qu.:0.900  
##  Mode  :character   Mode  :character   Median :10.10   Median :1.500  
##                                        Mean   :10.43   Mean   :1.875  
##                                        3rd Qu.:13.75   3rd Qu.:2.400  
##                                        Max.   :19.90   Max.   :6.600  
##                                                        NA's   :22     
##   sleep_cycle         awake          brainwt            bodywt        
##  Min.   :0.1167   Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:0.1833   1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :0.3333   Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :0.4396   Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:0.5792   3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :1.5000   Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##  NA's   :51                       NA's   :27
```

```r
str(msleep)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	83 obs. of  11 variables:
##  $ name        : chr  "Cheetah" "Owl monkey" "Mountain beaver" "Greater short-tailed shrew" ...
##  $ genus       : chr  "Acinonyx" "Aotus" "Aplodontia" "Blarina" ...
##  $ vore        : chr  "carni" "omni" "herbi" "omni" ...
##  $ order       : chr  "Carnivora" "Primates" "Rodentia" "Soricomorpha" ...
##  $ conservation: chr  "lc" NA "nt" "lc" ...
##  $ sleep_total : num  12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
##  $ sleep_rem   : num  NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
##  $ sleep_cycle : num  NA NA NA 0.133 0.667 ...
##  $ awake       : num  11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
##  $ brainwt     : num  NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
##  $ bodywt      : num  50 0.48 1.35 0.019 600 ...
```
3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal.

```r
select(msleep, name, genus, bodywt) 
```

```
## # A tibble: 83 x 3
##    name                       genus        bodywt
##    <chr>                      <chr>         <dbl>
##  1 Cheetah                    Acinonyx     50    
##  2 Owl monkey                 Aotus         0.48 
##  3 Mountain beaver            Aplodontia    1.35 
##  4 Greater short-tailed shrew Blarina       0.019
##  5 Cow                        Bos         600    
##  6 Three-toed sloth           Bradypus      3.85 
##  7 Northern fur seal          Callorhinus  20.5  
##  8 Vesper mouse               Calomys       0.045
##  9 Dog                        Canis        14    
## 10 Roe deer                   Capreolus    14.8  
## # ... with 73 more rows
```
4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total Make two new dataframes (large and small) based on these parameters.

```r
small <- msleep%>% 
  select(name, bodywt, sleep_total)%>% 
  filter(bodywt <= 1)
  small
```

```
## # A tibble: 36 x 3
##    name                       bodywt sleep_total
##    <chr>                       <dbl>       <dbl>
##  1 Owl monkey                  0.48         17  
##  2 Greater short-tailed shrew  0.019        14.9
##  3 Vesper mouse                0.045         7  
##  4 Guinea pig                  0.728         9.4
##  5 Chinchilla                  0.42         12.5
##  6 Star-nosed mole             0.06         10.3
##  7 African giant pouched rat   1             8.3
##  8 Lesser short-tailed shrew   0.005         9.1
##  9 Big brown bat               0.023        19.7
## 10 European hedgehog           0.77         10.1
## # ... with 26 more rows
```

```r
large <- msleep%>% 
  select(name, bodywt, sleep_total)%>% 
  filter(bodywt >= 200)
  large
```

```
## # A tibble: 7 x 3
##   name             bodywt sleep_total
##   <chr>             <dbl>       <dbl>
## 1 Cow                600          4  
## 2 Asian elephant    2547          3.9
## 3 Horse              521          2.9
## 4 Giraffe            900.         1.9
## 5 Pilot whale        800          2.7
## 6 African elephant  6654          3.3
## 7 Brazilian tapir    208.         4.4
```
5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?

```r
mean(large$sleep_total)
```

```
## [1] 3.3
```
6. What is the average sleep duration for small mammals as we have defined them?

```r
mean(small$sleep_total)
```

```
## [1] 12.65833
```
7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total.

```r
msleep %>% 
  select(name, genus, order, sleep_total)%>% 
  filter(sleep_total >= 18)
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 North American Opossum Didelphis  Didelphimorphia        18  
## 2 Big brown bat          Eptesicus  Chiroptera             19.7
## 3 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
## 4 Little brown bat       Myotis     Chiroptera             19.9
## 5 Giant armadillo        Priodontes Cingulata              18.1
```
