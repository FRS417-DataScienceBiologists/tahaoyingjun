---
title: "Heming's HW1"
output: 
  html_document: 
    keep_md: yes
---




```r
5 - 3 * 2
```

```
## [1] -1
```

```r
8 / 2 ** 2
```

```
## [1] 2
```

```r
a<-5-3
a*2
```

```
## [1] 4
```

```r
a<-8/2
a**2
```

```
## [1] 16
```

```r
blackjack <- c(140, -20, 70, -120, 240)
roulette <- c(60, 50, 120, -300, 10)
days<-c("Monday", "Tuesday", "Wednesday", "Thrusday", "Friday")
names(blackjack) <-days
names(roulette) <- days
total_blackjack <- sum(blackjack)
total_blackjack
```

```
## [1] 310
```

```r
total_roulette <- sum(roulette)
total_roulette
```

```
## [1] -60
```

```r
total_week <- blackjack+roulette
total_week
```

```
##    Monday   Tuesday Wednesday  Thrusday    Friday 
##       200        30       190      -420       250
```

```r
max(total_week)
```

```
## [1] 250
```

```r
min(total_week)
```

```
## [1] -420
```

```r
sum(blackjack)
```

```
## [1] 310
```

```r
sum(roulette)
```

```
## [1] -60
```
