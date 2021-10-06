Week 4
================

``` r
library(nycflights13)
```

    ## Warning: package 'nycflights13' was built under R version 4.0.5

``` r
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 4.0.5

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.0.5

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

Q1: What time of day should you fly if you want to avoid delays as much
as possible?

``` r
flights %>%
  group_by(hour) %>%
  summarise(arr_delay = mean(arr_delay, na.rm = T),dep_delay=mean(dep_delay, na.rm = T)) 
```

    ## # A tibble: 20 x 3
    ##     hour arr_delay dep_delay
    ##    <dbl>     <dbl>     <dbl>
    ##  1     1   NaN       NaN    
    ##  2     5    -4.80      0.688
    ##  3     6    -3.38      1.64 
    ##  4     7    -5.30      1.91 
    ##  5     8    -1.11      4.13 
    ##  6     9    -1.45      4.58 
    ##  7    10     0.954     6.50 
    ##  8    11     1.48      7.19 
    ##  9    12     3.49      8.61 
    ## 10    13     6.54     11.4  
    ## 11    14     9.20     13.8  
    ## 12    15    12.3      16.9  
    ## 13    16    12.6      18.8  
    ## 14    17    16.0      21.1  
    ## 15    18    14.8      21.1  
    ## 16    19    16.7      24.8  
    ## 17    20    16.7      24.3  
    ## 18    21    18.4      24.2  
    ## 19    22    16.0      18.8  
    ## 20    23    11.8      14.0

5am-10am.

Q2: Does this choice depend on anything? Season? Weather? Airport?
Airline?

One: Season

``` r
Season_flights <- flights %>%
  mutate(season = ifelse(month >= 3 & month <= 5, 'spring', 
                         ifelse(month>=6 & month <= 8, "summer",
                                ifelse(month>=9&month <=11, "autumn","winter"))))%>%
  group_by(season)%>%
  summarise(dep_delay=mean(dep_delay, na.rm = T))%>%
  arrange(dep_delay)

ggplot(Season_flights, aes(x=season, y=dep_delay))+
  geom_bar(stat="identity")+
  labs(x="Season",
       y="Departure Delay")
```

![](Week-4_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

The four seasons of depature delay time from longest to shortest are
summer, spring, winter, and autumn.

Two: Airport

``` r
Airport_flights <- flights %>%
  group_by(origin) %>%
  mutate(dep_delay_lag = lag(dep_delay)) %>%
  arrange(dep_delay) 

ggplot(Airport_flights, aes(y = dep_delay, x = origin)) +
  geom_bar(stat = "identity") +
  labs(x = "Departure Delay",
       y = "Delay time")
```

    ## Warning: Removed 8255 rows containing missing values (position_stack).

![](Week-4_files/figure-gfm/unnamed-chunk-4-1.png)<!-- --> The EWR has
the most depature delay time, and the LGA has the least depature delay
time.

Three: Airlines

``` r
Airlines_flights <- flights %>%
  group_by(carrier) %>%
  summarise(dep_delay = mean(dep_delay,na.rm = T))%>%
  arrange(dep_delay)

ggplot(Airlines_flights, aes(x=carrier, y=dep_delay))+
  geom_bar(stat = "identity") +
  labs(x="Carrier",
       y="Departure Delay Time")
```

![](Week-4_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

I would choose AS, HA, and US airlines because their departure delay
time is shorter.

<https://github.com/JayXie99/433HW/blob/0fbafbb8107ae1407cfe76040a0bfefbbbb77aee/Week%204.Rmd>
