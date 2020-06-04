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
  arrange(Id, NumberSemester, Semester) %>% 
  mutate(CurrentSemester= paste0(Semester, NumberSemester)) %>% 
  group_by(Id) %>% 
  mutate(FutureSemester = lead(CurrentSemester)) %>% 
  ungroup() 
```

``` r
knitr::kable(head(Simulated))
```

| Id | Semester | NumberSemester | CurrentSemester | FutureSemester |
| -: | :------- | -------------: | :-------------- | :------------- |
|  1 | F        |              1 | F1              | W1             |
|  1 | W        |              1 | W1              | F2             |
|  1 | F        |              2 | F2              | W2             |
|  1 | W        |              2 | W2              | F3             |
|  1 | F        |              3 | F3              | W3             |
|  1 | W        |              3 | W3              | F4             |
