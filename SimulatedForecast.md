Student Forecast Simulation
================

## Describe a model:

``` r
library(tidyverse)
```

    ## -- Attaching packages -------------------------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.1     v purrr   0.3.4
    ## v tibble  3.0.1     v dplyr   1.0.0
    ## v tidyr   1.1.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ----------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rsample)
library(markovchain)
```

    ## Package:  markovchain
    ## Version:  0.8.5
    ## Date:     2020-05-21
    ## BugReport: http://github.com/spedygiorgio/markovchain/issues

``` r
Simulated = tibble(Id = seq(1, 100, 1),
                   Semester = rep(c("F", "W"),50),
                   NumberSemester = rep(1:4, 25)) %>% 
  expand(Id, Semester, NumberSemester) %>% 
  arrange(Id, NumberSemester, Semester) 

Simulated2 = Simulated %>% 
  mutate(ProbOfMovingOn = runif(dim(Simulated)[1], 0,1),
         MovedOn = case_when(ProbOfMovingOn > .1 ~ 1, TRUE ~ 0) ,
         CurrentSemester= case_when( MovedOn == 1 ~ paste0(Semester, NumberSemester), TRUE ~ "Leave")) %>% 
  group_by(Id) %>% 
  mutate(FutureSemester = lead(CurrentSemester)) %>% 
  ungroup() 
```

The next step is to simulate the chances of moving on.

``` r
knitr::kable(head(Simulated2))
```

| Id | Semester | NumberSemester | ProbOfMovingOn | MovedOn | CurrentSemester | FutureSemester |
| -: | :------- | -------------: | -------------: | ------: | :-------------- | :------------- |
|  1 | F        |              1 |      0.8502838 |       1 | F1              | W1             |
|  1 | W        |              1 |      0.6897115 |       1 | W1              | Leave          |
|  1 | F        |              2 |      0.0307998 |       0 | Leave           | W2             |
|  1 | W        |              2 |      0.8503098 |       1 | W2              | F3             |
|  1 | F        |              3 |      0.2289828 |       1 | F3              | W3             |
|  1 | W        |              3 |      0.5899675 |       1 | W3              | F4             |

``` r
knitr::kable( Simulated2 %>% 
  count(CurrentSemester, FutureSemester) %>% 
  tidyr::spread(FutureSemester, n, fill = 0) ) 
```

| CurrentSemester | F2 | F3 | F4 | Leave | W1 | W2 | W3 | W4 | <NA> |
| :-------------- | -: | -: | -: | ----: | -: | -: | -: | -: | ---: |
| F1              |  0 |  0 |  0 |    11 | 75 |  0 |  0 |  0 |    0 |
| F2              |  0 |  0 |  0 |     6 |  0 | 84 |  0 |  0 |    0 |
| F3              |  0 |  0 |  0 |    10 |  0 |  0 | 82 |  0 |    0 |
| F4              |  0 |  0 |  0 |     8 |  0 |  0 |  0 | 87 |    0 |
| Leave           | 11 |  6 |  9 |     4 | 14 | 10 |  7 |  4 |    9 |
| W1              | 79 |  0 |  0 |    10 |  0 |  0 |  0 |  0 |    0 |
| W2              |  0 | 86 |  0 |     8 |  0 |  0 |  0 |  0 |    0 |
| W3              |  0 |  0 | 86 |     3 |  0 |  0 |  0 |  0 |    0 |
| W4              |  0 |  0 |  0 |     0 |  0 |  0 |  0 |  0 |   91 |
