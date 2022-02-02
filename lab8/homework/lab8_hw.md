---
title: "Lab 8 Homework"
author: "Please Add Your Name Here"
date: "2022-02-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches<- read_csv("/Users/cristina/Desktop/BIS15W2022_cgonzalez/lab8/data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
sydneybeaches
```

```
## # A tibble: 3,690 × 8
##    BeachId Region        Council Site  Longitude Latitude Date  `Enterococci (…`
##      <dbl> <chr>         <chr>   <chr>     <dbl>    <dbl> <chr>            <dbl>
##  1      25 Sydney City … Randwi… Clov…      151.    -33.9 02/0…               19
##  2      25 Sydney City … Randwi… Clov…      151.    -33.9 06/0…                3
##  3      25 Sydney City … Randwi… Clov…      151.    -33.9 12/0…                2
##  4      25 Sydney City … Randwi… Clov…      151.    -33.9 18/0…               13
##  5      25 Sydney City … Randwi… Clov…      151.    -33.9 30/0…                8
##  6      25 Sydney City … Randwi… Clov…      151.    -33.9 05/0…                7
##  7      25 Sydney City … Randwi… Clov…      151.    -33.9 11/0…               11
##  8      25 Sydney City … Randwi… Clov…      151.    -33.9 23/0…               97
##  9      25 Sydney City … Randwi… Clov…      151.    -33.9 07/0…                3
## 10      25 Sydney City … Randwi… Clov…      151.    -33.9 25/0…                0
## # … with 3,680 more rows
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at /Users/cristina/Desktop/BIS15W2022_cgonzalez
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
sydneybeaches
```

```
## # A tibble: 3,690 × 8
##    beach_id region       council site  longitude latitude date  enterococci_cfu…
##       <dbl> <chr>        <chr>   <chr>     <dbl>    <dbl> <chr>            <dbl>
##  1       25 Sydney City… Randwi… Clov…      151.    -33.9 02/0…               19
##  2       25 Sydney City… Randwi… Clov…      151.    -33.9 06/0…                3
##  3       25 Sydney City… Randwi… Clov…      151.    -33.9 12/0…                2
##  4       25 Sydney City… Randwi… Clov…      151.    -33.9 18/0…               13
##  5       25 Sydney City… Randwi… Clov…      151.    -33.9 30/0…                8
##  6       25 Sydney City… Randwi… Clov…      151.    -33.9 05/0…                7
##  7       25 Sydney City… Randwi… Clov…      151.    -33.9 11/0…               11
##  8       25 Sydney City… Randwi… Clov…      151.    -33.9 23/0…               97
##  9       25 Sydney City… Randwi… Clov…      151.    -33.9 07/0…                3
## 10       25 Sydney City… Randwi… Clov…      151.    -33.9 25/0…                0
## # … with 3,680 more rows
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?
These data are in long format because there are multiple rows for each date. 

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`

```r
sydneybeaches_long <- sydneybeaches %>% 
  select(site, date, enterococci_cfu_100ml) %>% 
  arrange(date)
sydneybeaches_long
```

```
## # A tibble: 3,690 × 3
##    site                    date       enterococci_cfu_100ml
##    <chr>                   <chr>                      <dbl>
##  1 Clovelly Beach          01/02/2014                     8
##  2 Coogee Beach            01/02/2014                    29
##  3 Gordons Bay (East)      01/02/2014                     5
##  4 Little Bay Beach        01/02/2014                    13
##  5 Malabar Beach           01/02/2014                    35
##  6 Maroubra Beach          01/02/2014                     3
##  7 South Maroubra Beach    01/02/2014                     3
##  8 South Maroubra Rockpool 01/02/2014                     5
##  9 Bondi Beach             01/02/2014                    15
## 10 Bronte Beach            01/02/2014                    17
## # … with 3,680 more rows
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`

```r
sydneybeaches_wide <- sydneybeaches_long %>% 
  pivot_wider(names_from = "date",
              values_from = "enterococci_cfu_100ml")
sydneybeaches_wide
```

```
## # A tibble: 11 × 345
##    site         `01/02/2014` `01/02/2018` `01/03/2013` `01/04/2015` `01/05/2013`
##    <chr>               <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
##  1 Clovelly Be…            8            3           20           44            3
##  2 Coogee Beach           29           42          110           57            7
##  3 Gordons Bay…            5           24           NA           26            0
##  4 Little Bay …           13           19           56           19            4
##  5 Malabar Bea…           35           50           24           50           32
##  6 Maroubra Be…            3           50           10           12            3
##  7 South Marou…            3           46          450            5            0
##  8 South Marou…            5           52           22           14           10
##  9 Bondi Beach            15           48          360           25            2
## 10 Bronte Beach           17           30           32           21            4
## 11 Tamarama Be…           13           35          270           43            2
## # … with 339 more variables: `01/06/2017` <dbl>, `01/08/2014` <dbl>,
## #   `01/08/2018` <dbl>, `01/09/2016` <dbl>, `01/09/2017` <dbl>,
## #   `02/01/2013` <dbl>, `02/03/2018` <dbl>, `02/04/2013` <dbl>,
## #   `02/05/2018` <dbl>, `02/07/2015` <dbl>, `02/08/2016` <dbl>,
## #   `02/09/2013` <dbl>, `02/09/2014` <dbl>, `02/11/2014` <dbl>,
## #   `03/01/2018` <dbl>, `03/02/2016` <dbl>, `03/03/2017` <dbl>,
## #   `03/04/2017` <dbl>, `03/04/2018` <dbl>, `03/06/2014` <dbl>, …
```


5. Pivot the data back so that the dates are data and not column names.

```r
sydneybeaches_wide %>% 
  pivot_longer(-site,
               names_to = "date",
               values_to = "enterococci_cfu_100ml")
```

```
## # A tibble: 3,784 × 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 01/02/2014                     8
##  2 Clovelly Beach 01/02/2018                     3
##  3 Clovelly Beach 01/03/2013                    20
##  4 Clovelly Beach 01/04/2015                    44
##  5 Clovelly Beach 01/05/2013                     3
##  6 Clovelly Beach 01/06/2017                     4
##  7 Clovelly Beach 01/08/2014                     1
##  8 Clovelly Beach 01/08/2018                     3
##  9 Clovelly Beach 01/09/2016                     1
## 10 Clovelly Beach 01/09/2017                     3
## # … with 3,774 more rows
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.

```r
sydneybeaches_long %>% 
  separate(date, into= c("month", "day", "year"), sep = "/")
```

```
## # A tibble: 3,690 × 5
##    site                    month day   year  enterococci_cfu_100ml
##    <chr>                   <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach          01    02    2014                      8
##  2 Coogee Beach            01    02    2014                     29
##  3 Gordons Bay (East)      01    02    2014                      5
##  4 Little Bay Beach        01    02    2014                     13
##  5 Malabar Beach           01    02    2014                     35
##  6 Maroubra Beach          01    02    2014                      3
##  7 South Maroubra Beach    01    02    2014                      3
##  8 South Maroubra Rockpool 01    02    2014                      5
##  9 Bondi Beach             01    02    2014                     15
## 10 Bronte Beach            01    02    2014                     17
## # … with 3,680 more rows
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
sydneybeaches_long2 <- sydneybeaches_long %>% 
  separate(date, into= c("month", "day", "year"), sep = "/") %>% 
  group_by(year, site) %>% 
  summarize(average_enterococci_cfu=mean(enterococci_cfu_100ml, na.rm=T))
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.
```

```r
sydneybeaches_long2
```

```
## # A tibble: 66 × 3
## # Groups:   year [6]
##    year  site                    average_enterococci_cfu
##    <chr> <chr>                                     <dbl>
##  1 2013  Bondi Beach                               32.2 
##  2 2013  Bronte Beach                              26.8 
##  3 2013  Clovelly Beach                             9.28
##  4 2013  Coogee Beach                              39.7 
##  5 2013  Gordons Bay (East)                        24.8 
##  6 2013  Little Bay Beach                         122.  
##  7 2013  Malabar Beach                            101.  
##  8 2013  Maroubra Beach                            47.1 
##  9 2013  South Maroubra Beach                      39.3 
## 10 2013  South Maroubra Rockpool                   96.4 
## # … with 56 more rows
```


8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
sydneybeaches_wide2 <- sydneybeaches_long2 %>% 
  pivot_wider(names_from = year,
              values_from = average_enterococci_cfu)
sydneybeaches_wide2
```

```
## # A tibble: 11 × 7
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2   22.9 
##  2 Bronte Beach             26.8   17.5   23.6    61.3  16.8   43.4 
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93  10.6 
##  4 Coogee Beach             39.7   52.6   40.3    59.5  20.7   21.6 
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7   17.6 
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2   59.1 
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8   38.0 
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6    9.21
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26  12.5 
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9  112.  
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4   15.5
```


9. What was the most polluted beach in 2018?

```r
sydneybeaches_wide2 %>% 
  select(site, "2018") %>% 
  arrange(desc(`2018`))
```

```
## # A tibble: 11 × 2
##    site                    `2018`
##    <chr>                    <dbl>
##  1 South Maroubra Rockpool 112.  
##  2 Little Bay Beach         59.1 
##  3 Bronte Beach             43.4 
##  4 Malabar Beach            38.0 
##  5 Bondi Beach              22.9 
##  6 Coogee Beach             21.6 
##  7 Gordons Bay (East)       17.6 
##  8 Tamarama Beach           15.5 
##  9 South Maroubra Beach     12.5 
## 10 Clovelly Beach           10.6 
## 11 Maroubra Beach            9.21
```
South Maroubra Rockpool was the most polluted in 2018.

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
