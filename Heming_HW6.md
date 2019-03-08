---
title: "Heming HW6"
output: 
  html_document: 
    keep_md: yes
---



#Load the libraries

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.2.5
## v tibble  1.4.2     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.3.1
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(skimr)
library("RColorBrewer")
```
#Gapminder

```r
library("gapminder")
```
Load the data into a new object called gapminder.

```r
gapminder <- 
  gapminder::gapminder
```
1. Explore the data using the various function you have learned. Is it tidy, are there any NA's, what are its dimensions, what are the column names, etc.


```r
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```


```r
gapminder %>% 
  summarize(number_nas1=sum(is.na(country)),
            number_nas2=sum(is.na(continent)),
            number_nas3=sum(is.na(year)),
            number_nas4=sum(is.na(lifeExp)),
            number_nas5=sum(is.na(pop)),
            number_nas6=sum(is.na(gdpPercap)))
```

```
## # A tibble: 1 x 6
##   number_nas1 number_nas2 number_nas3 number_nas4 number_nas5 number_nas6
##         <int>       <int>       <int>       <int>       <int>       <int>
## 1           0           0           0           0           0           0
```
2. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer on average. Make a quick plot below to visualize this relationship.

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap,y=lifeExp))+
  geom_jitter()+
  labs(title = "per capita GDP vs. life expectancy",
       x= "per capita GDP",
       y= "life expectancy")
```

![](Heming_HW6_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

3. There is extreme disparity in per capita GDP. Rescale the x axis to make this easier to interpret. How would you characterize the relationship?

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap,y=lifeExp))+
  geom_jitter()+
  scale_x_log10()+
  labs(title = "per capita GDP vs. life expectancy",
       x= "per capita GDP",
       y= "life expectancy")
```

![](Heming_HW6_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
4. This should look pretty dense to you with significant overplotting. Try using a faceting approach to break this relationship down by year.

```r
gapminder %>% 
  ggplot(aes(x=gdpPercap,y=lifeExp))+
  geom_jitter()+
  scale_x_log10()+
  labs(title = "per capita GDP vs. life expectancy",
       x= "per capita GDP",
       y= "life expectancy")+
  facet_wrap(~year)
```

![](Heming_HW6_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
5. Simplify the comparison by comparing only 1952 and 2007. Can you come to any conclusions?

```r
gapminder %>% 
  filter(year=="1952"|year=="2007") %>%  
  ggplot(aes(x=gdpPercap,y=lifeExp))+
  geom_jitter()+
  scale_x_log10()+
  labs(title = "per capita GDP vs. life expectancy",
       x= "per capita GDP",
       y= "life expectancy")+
  facet_grid(~year)
```

![](Heming_HW6_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
6. Let's stick with the 1952 and 2007 comparison but make some aesthetic adjustments. First try to color by continent and adjust the size of the points by population. Add + scale_size(range = c(0.1, 10), guide = "none") as a layer to clean things up a bit.

```r
gapminder %>% 
  filter(year=="1952"|year=="2007") %>%  
  ggplot(aes(x=gdpPercap,y=lifeExp, color=continent, size=pop))+
  geom_jitter()+
  scale_x_log10()+
  scale_size(range = c(0.1, 10), guide = "none")+
  labs(title = "per capita GDP vs. life expectancy",
       x= "per capita GDP",
       y= "life expectancy")+
  facet_grid(~year)
```

![](Heming_HW6_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
7. Although we did not introduce them in lab, ggplot has a number of built-in themes that make things easier. I like the light theme for these data, but you can see lots of options. Apply one of these to your plot above.

```r
gapminder %>% 
  filter(year=="1952"|year=="2007") %>%  
  ggplot(aes(x=gdpPercap,y=lifeExp, color=continent, size=pop))+
  geom_jitter()+
  scale_x_log10()+
  scale_size(range = c(0.1, 10), guide = "none")+
  labs(title = "per capita GDP vs. life expectancy",
       x= "per capita GDP",
       y= "life expectancy")+
  scale_colour_brewer(palette = "Dark2")+
  facet_grid(~year)
```

![](Heming_HW6_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
8. What is the population for all countries on the Asian continent in 2007? Show this as a barplot.

```r
gapminder %>% 
  filter(year==2007, continent== "Asia") %>% 
  ggplot(aes(x=reorder(country, -pop), y=pop))+
  geom_bar(stat = "identity")+
  coord_flip()+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

![](Heming_HW6_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

9. You should see that China's population is the largest with India a close second. Let's focus on China only. Make a plot that shows how population has changed over the years.

```r
gapminder %>% 
  filter(country=="China") %>% 
  ggplot(aes(x=year,y=pop))+
  geom_bar(stat = "identity")+
  labs(title = "Population Growth of China",
       x = "Year",
       y = "Population")
```

![](Heming_HW6_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

10. Let's compare China and India. Make a barplot that shows population growth by year using position=dodge. Apply a custom color theme using RColorBrewer.

```r
gapminder %>% 
  filter(country=="China"|country=="India") %>% 
  ggplot(aes(x=year,y=pop, fill=country))+
  geom_bar(stat = "identity",position="dodge")+
  labs(title = "Population Growth of China",
       x = "Year",
       y = "Population")+
   scale_fill_brewer(palette = "Dark2")
```

![](Heming_HW6_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

