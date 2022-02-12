---
title: "Lab 10 Homework"
author: "Cristina Gonzalez"
date: "2022-02-11"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  

```r
names(deserts)
```

```
##  [1] "record_id"       "month"           "day"             "year"           
##  [5] "plot_id"         "species_id"      "sex"             "hindfoot_length"
##  [9] "weight"          "genus"           "species"         "taxa"           
## [13] "plot_type"
```


```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, …
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, …
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", …
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod…
```

```r
summary(deserts)
```

```
##    record_id         month             day            year         plot_id     
##  Min.   :    1   Min.   : 1.000   Min.   : 1.0   Min.   :1977   Min.   : 1.00  
##  1st Qu.: 8964   1st Qu.: 4.000   1st Qu.: 9.0   1st Qu.:1984   1st Qu.: 5.00  
##  Median :17762   Median : 6.000   Median :16.0   Median :1990   Median :11.00  
##  Mean   :17804   Mean   : 6.474   Mean   :16.1   Mean   :1990   Mean   :11.34  
##  3rd Qu.:26655   3rd Qu.:10.000   3rd Qu.:23.0   3rd Qu.:1997   3rd Qu.:17.00  
##  Max.   :35548   Max.   :12.000   Max.   :31.0   Max.   :2002   Max.   :24.00  
##                                                                                
##   species_id            sex            hindfoot_length     weight      
##  Length:34786       Length:34786       Min.   : 2.00   Min.   :  4.00  
##  Class :character   Class :character   1st Qu.:21.00   1st Qu.: 20.00  
##  Mode  :character   Mode  :character   Median :32.00   Median : 37.00  
##                                        Mean   :29.29   Mean   : 42.67  
##                                        3rd Qu.:36.00   3rd Qu.: 48.00  
##                                        Max.   :70.00   Max.   :280.00  
##                                        NA's   :3348    NA's   :2503    
##     genus             species              taxa            plot_type        
##  Length:34786       Length:34786       Length:34786       Length:34786      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
## 
```
The data appear tidy.

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts %>% 
  summarize(genera=n_distinct(genus),
            species=n_distinct(species))
```

```
## # A tibble: 1 × 2
##   genera species
##    <int>   <int>
## 1     26      40
```

```r
deserts %>% 
  summarise(observations=n())
```

```
## # A tibble: 1 × 1
##   observations
##          <int>
## 1        34786
```


```r
deserts %>% 
  count(species) %>% 
  arrange(desc(n))
```

```
## # A tibble: 40 × 2
##    species          n
##    <chr>        <int>
##  1 merriami     10596
##  2 penicillatus  3123
##  3 ordii         3027
##  4 baileyi       2891
##  5 megalotis     2609
##  6 spectabilis   2504
##  7 torridus      2249
##  8 flavus        1597
##  9 eremicus      1299
## 10 albigula      1252
## # … with 30 more rows
```
26 genera and 40 species represented, with a total of 34786 observations. Clarki, scutalatus, tereticaudus, tigris, uniparens, viridis were all only sampled 1 time. Merriami was sampled the most.

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.

```r
deserts %>% 
  count(taxa) %>% 
  arrange(n) 
```

```
## # A tibble: 4 × 2
##   taxa        n
##   <chr>   <int>
## 1 Reptile    14
## 2 Rabbit     75
## 3 Bird      450
## 4 Rodent  34247
```

```r
deserts %>% 
  ggplot(aes(x= taxa, fill=taxa))+
  geom_bar()+
  scale_y_log10()+
  labs(title="Observations by Taxa")
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`

```r
deserts %>% 
  ggplot(aes(x= taxa, fill=plot_type))+
  geom_bar(position="dodge")+
  scale_y_log10() +
  labs(title="Observed Taxa by Plot Type")
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts %>% 
  group_by(species) %>% 
  summarize(min_weight=min(weight, na.rm=T),
            max_weight=max(weight, na.rm=T)) %>% 
  arrange(min_weight)
```

```
## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf

## Warning in min(weight, na.rm = T): no non-missing arguments to min; returning
## Inf
```

```
## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf

## Warning in max(weight, na.rm = T): no non-missing arguments to max; returning
## -Inf
```

```
## # A tibble: 40 × 3
##    species      min_weight max_weight
##    <chr>             <dbl>      <dbl>
##  1 flavus                4         25
##  2 megalotis             4         29
##  3 penicillatus          4         74
##  4 torridus              5         46
##  5 taylori               6         18
##  6 maniculatus           7         49
##  7 eremicus              8         40
##  8 leucopus              8         27
##  9 montanus              8         13
## 10 fulvescens            9         20
## # … with 30 more rows
```

```r
deserts %>% 
  na.omit() %>% 
  ggplot(aes(x= species, y=weight))+
  geom_boxplot()+
  coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

6. Add another layer to your answer from #5 using `geom_point` to get an idea of how many measurements were taken for each species.

```r
deserts %>% 
  na.omit() %>% 
  ggplot(aes(x= species, y=weight))+
  geom_point(alpha=0.5, color="royalblue2", position="jitter")+
  geom_boxplot(alpha=0.5)+
  coord_flip()
```

![](lab10_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
deserts %>% 
  filter(species=="merriami") %>% 
  group_by(year) %>% 
  summarise(n=n())
```

```
## # A tibble: 26 × 2
##     year     n
##    <dbl> <int>
##  1  1977   264
##  2  1978   389
##  3  1979   209
##  4  1980   493
##  5  1981   559
##  6  1982   609
##  7  1983   528
##  8  1984   396
##  9  1985   667
## 10  1986   406
## # … with 16 more rows
```

```r
deserts %>% 
  filter(species=="merriami") %>% 
  ggplot(aes(x=as.factor(year)))+
  geom_bar()+
  theme(axis.text.x = element_text(angle = 60))+
  labs(title="Dipodomys merriami Observations Over Time",
       x="Year",
       y="Observations")
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.

```r
deserts %>% 
  ggplot(aes(x=weight, y=hindfoot_length))+
  geom_point(color="palevioletred3", alpha=0.2)+
  labs(title = "Hindfoot Length vs. Height",
       x="Weight",
       y="Hindfoot Length")
```

```
## Warning: Removed 4048 rows containing missing values (geom_point).
```

![](lab10_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts %>% 
  select(genus, species, weight) %>% 
  group_by(species) %>% 
  summarize(mean_weight=mean(weight, na.rm=T)) %>% 
  arrange(desc(mean_weight))
```

```
## # A tibble: 40 × 2
##    species      mean_weight
##    <chr>              <dbl>
##  1 albigula           159. 
##  2 spectabilis        120. 
##  3 spilosoma           93.5
##  4 hispidus            65.6
##  5 fulviventer         58.9
##  6 ochrognathus        55.4
##  7 ordii               48.9
##  8 merriami            43.2
##  9 baileyi             31.7
## 10 leucogaster         31.6
## # … with 30 more rows
```

```r
deserts %>% 
  filter(species=="spectabilis" | species=="albigula") %>%
  filter(weight!="NA" & hindfoot_length!="NA") %>% 
  select(sex, genus, species, weight, hindfoot_length) %>% 
  mutate(ratio=weight/hindfoot_length) %>% 
  arrange(desc(ratio))
```

```
## # A tibble: 3,072 × 6
##    sex   genus   species  weight hindfoot_length ratio
##    <chr> <chr>   <chr>     <dbl>           <dbl> <dbl>
##  1 M     Neotoma albigula    278              32  8.69
##  2 M     Neotoma albigula    280              33  8.48
##  3 M     Neotoma albigula    270              32  8.44
##  4 F     Neotoma albigula    274              33  8.30
##  5 M     Neotoma albigula    253              32  7.91
##  6 M     Neotoma albigula    260              33  7.88
##  7 F     Neotoma albigula    252              32  7.88
##  8 M     Neotoma albigula    259              33  7.85
##  9 M     Neotoma albigula    259              33  7.85
## 10 M     Neotoma albigula    265              34  7.79
## # … with 3,062 more rows
```

```r
deserts %>% 
  filter(species=="spectabilis" | species=="albigula") %>% 
  filter(weight!="NA" & hindfoot_length!="NA" & sex!="NA") %>% 
  select(sex, genus, species, weight, hindfoot_length) %>% 
  mutate(ratio=weight/hindfoot_length) %>% 
  ggplot(aes(x=species, y=ratio, fill=sex))+
  geom_boxplot()
```

![](lab10_hw_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.

```r
deserts %>% 
  filter(species=="albigula") %>% 
  na.omit(weight) %>% 
  group_by(year) %>% 
  summarize(mean_weight=mean(weight))
```

```
## # A tibble: 25 × 2
##     year mean_weight
##    <dbl>       <dbl>
##  1  1978        181.
##  2  1979        138 
##  3  1980        160.
##  4  1981        161.
##  5  1982        164.
##  6  1983        156.
##  7  1984        150.
##  8  1985        149.
##  9  1986        160.
## 10  1987        160.
## # … with 15 more rows
```


```r
deserts %>% 
  filter(species=="albigula") %>% 
  na.omit(weight) %>% 
  group_by(year) %>% 
  summarize(mean_weight=mean(weight)) %>% 
  ggplot(aes(x=as.factor(year), y=mean_weight))+
  geom_col(fill="skyblue4")+
  theme(axis.text.x = element_text(angle = 60))+
  labs(title="Neotoma albigula Weight Through Time",
       y="Average Weight",
       x="Year")
```

![](lab10_hw_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
