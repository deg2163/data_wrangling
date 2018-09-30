Data Import
================
Diana Ballesteros
9/25/2018

Gather
------

PULSE Data

``` r
pulse_data = haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names()
pulse_datapulse_data = haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names()
pulse_data
```

    ## # A tibble: 1,087 x 7
    ##       id   age sex  bdi_score_bl bdi_score_01m bdi_score_06m bdi_score_12m
    ##    <dbl> <dbl> <ch>        <dbl>         <dbl>         <dbl>         <dbl>
    ##  1 10003  48.0 male            7             1             2             0
    ##  2 10015  72.5 male            6            NA            NA            NA
    ##  3 10022  58.5 male           14             3             8            NA
    ##  4 10026  72.7 male           20             6            18            16
    ##  5 10035  60.4 male            4             0             1             2
    ##  6 10050  84.7 male            2            10            12             8
    ##  7 10078  31.3 male            4             0            NA            NA
    ##  8 10088  56.9 male            5            NA             0             2
    ##  9 10091  76.0 male            0             3             4             0
    ## 10 10092  74.2 fem…           10             2            11             6
    ## # ... with 1,077 more rows

This isn't tidy yet...

``` r
## in  class 
pulse_tidy = pulse_data %>% 
  gather(key = "visit", value = "bdi_score", bdi_score_01m:bdi_score_12m)
```

    ## Warning: attributes are not identical across measure variables;
    ## they will be dropped

``` r
## to view the tidy version 
##pulse_tidy %>% View("tidy")
  ##gather(key = "visit", value = "bdi_score", bdi_score_01m:bdi_score_12m) %>% View("tidy")

## to view the non-tidy version 
##pulse_tidy %>% View ("nontidy")
```

``` r
## this is the same as the one before from the website 
pulse_tidy_data = gather(pulse_data, key = visit, value = bdi, bdi_score_bl:bdi_score_12m)
```

    ## Warning: attributes are not identical across measure variables;
    ## they will be dropped

``` r
pulse_tidy_data
```

    ## # A tibble: 4,348 x 5
    ##       id   age sex    visit          bdi
    ##    <dbl> <dbl> <chr>  <chr>        <dbl>
    ##  1 10003  48.0 male   bdi_score_bl     7
    ##  2 10015  72.5 male   bdi_score_bl     6
    ##  3 10022  58.5 male   bdi_score_bl    14
    ##  4 10026  72.7 male   bdi_score_bl    20
    ##  5 10035  60.4 male   bdi_score_bl     4
    ##  6 10050  84.7 male   bdi_score_bl     2
    ##  7 10078  31.3 male   bdi_score_bl     4
    ##  8 10088  56.9 male   bdi_score_bl     5
    ##  9 10091  76.0 male   bdi_score_bl     0
    ## 10 10092  74.2 female bdi_score_bl    10
    ## # ... with 4,338 more rows

Illustrate 'separate':

``` r
##?separate can help you pull the help 

pulse_tidy %>% 
  separate(visit, into = c("bdi_str","score_str","visit"), sep = "_") %>% 
  select(-bdi_str, -score_str) %>% 
  mutate(visit = replace(visit, visit == "b1", "00m"))
```

    ## # A tibble: 3,261 x 6
    ##       id   age sex    bdi_score_bl visit bdi_score
    ##    <dbl> <dbl> <chr>         <dbl> <chr>     <dbl>
    ##  1 10003  48.0 male              7 01m           1
    ##  2 10015  72.5 male              6 01m          NA
    ##  3 10022  58.5 male             14 01m           3
    ##  4 10026  72.7 male             20 01m           6
    ##  5 10035  60.4 male              4 01m           0
    ##  6 10050  84.7 male              2 01m          10
    ##  7 10078  31.3 male              4 01m           0
    ##  8 10088  56.9 male              5 01m          NA
    ##  9 10091  76.0 male              0 01m           3
    ## 10 10092  74.2 female           10 01m           2
    ## # ... with 3,251 more rows

All together, the data import/cleaning pipeline is

``` r
## creating a clean data set with everything from above 
pulse_df = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names() %>%
  gather(key = visit, value = bdi, bdi_score_bl:bdi_score_12m) %>%
  separate(visit, into = c("bdi_str","score_str","visit"), sep = "_") %>% 
  select(-bdi_str, -score_str) %>% 
  mutate(visit = replace(visit, visit == "b1", "00m"))
```

    ## Warning: attributes are not identical across measure variables;
    ## they will be dropped

Revisit FAS\_litters
--------------------

``` r
## in-class showing how to separate 2 variables in 1 column 
litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>%
  separate(group, into = c("dose", "day"), 3) %>%
  mutate(dose = tolower(dose),
         wt_gain = gd18_weight - gd0_weight) %>%
  arrange(litter_number)

## website 
litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3) %>%
  mutate(dose = tolower(dose),
         wt_gain = gd18_weight - gd0_weight) %>%
  arrange(litter_number)
```

Learning Assessment (Data Cleaning/Tidying)
-------------------------------------------

``` r
litter_data_practice = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>% 
  select(litter_number, ends_with("weight")) %>% 
  gather(key = gd, value = weight, gd0_weight:gd18_weight) %>% 
  mutate(gd = recode(gd, "gd0_weight" = 0, "gd18_weight" = 18))
```

Spread
------

Create 'analysis\_result'

``` r
analysis_result = tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)

analysis_result
```

    ## # A tibble: 4 x 3
    ##   group     time   mean
    ##   <chr>     <chr> <dbl>
    ## 1 treatment pre     4  
    ## 2 treatment post    8  
    ## 3 placebo   pre     3.5
    ## 4 placebo   post    4

Make it readable:

``` r
analysis_result %>% 
  spread(key = time, value = mean) %>% 
  knitr::kable()
```

| group     |  post|  pre|
|:----------|-----:|----:|
| placebo   |     4|  3.5|
| treatment |     8|  4.0|

``` r
##knitr:kable() is what makes the table presentable when you knit the data
```

Bind Rows
---------

Read in LotR Data

``` r
fellowship_ring = readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>%
  mutate(movie = "fellowship_ring")

two_towers = readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")
```

Create Final LotR data:

``` r
## in class, which helps you stack the rows on top of each other 
bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  gather(key = sex, value = word, female:male) %>% 
  mutate(race = tolower(race))
```

    ## # A tibble: 18 x 4
    ##    race   movie           sex     word
    ##    <chr>  <chr>           <chr>  <dbl>
    ##  1 elf    fellowship_ring female  1229
    ##  2 hobbit fellowship_ring female    14
    ##  3 man    fellowship_ring female     0
    ##  4 elf    two_towers      female   331
    ##  5 hobbit two_towers      female     0
    ##  6 man    two_towers      female   401
    ##  7 elf    return_king     female   183
    ##  8 hobbit return_king     female     2
    ##  9 man    return_king     female   268
    ## 10 elf    fellowship_ring male     971
    ## 11 hobbit fellowship_ring male    3644
    ## 12 man    fellowship_ring male    1995
    ## 13 elf    two_towers      male     513
    ## 14 hobbit two_towers      male    2463
    ## 15 man    two_towers      male    3589
    ## 16 elf    return_king     male     510
    ## 17 hobbit return_king     male    2673
    ## 18 man    return_king     male    2459

Join Data
---------

``` r
pup_data = read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 

litter_data = read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = tolower(group))
```

Create joined data:
-------------------

``` r
## left join, you don't have to put by, but it will be helpful
## pipes work if you are following a sequential thing, recommends that we create a new thing when you want to join the two data sets
fas_data = left_join(pup_data, litter_data, by = "litter_number")
fas_data
```

    ## # A tibble: 313 x 13
    ##    litter_number sex   pd_ears pd_eyes pd_pivot pd_walk group gd0_weight
    ##    <chr>         <chr>   <int>   <int>    <int>   <int> <chr>      <dbl>
    ##  1 #85           male        4      13        7      11 con7        19.7
    ##  2 #85           male        4      13        7      12 con7        19.7
    ##  3 #1/2/95/2     male        5      13        7       9 con7        27  
    ##  4 #1/2/95/2     male        5      13        8      10 con7        27  
    ##  5 #5/5/3/83/3-3 male        5      13        8      10 con7        26  
    ##  6 #5/5/3/83/3-3 male        5      14        6       9 con7        26  
    ##  7 #5/4/2/95/2   male       NA      14        5       9 con7        28.5
    ##  8 #4/2/95/3-3   male        4      13        6       8 con7        NA  
    ##  9 #4/2/95/3-3   male        4      13        7       9 con7        NA  
    ## 10 #2/2/95/3-2   male        4      NA        8      10 con7        NA  
    ## # ... with 303 more rows, and 5 more variables: gd18_weight <dbl>,
    ## #   gd_of_birth <int>, pups_born_alive <int>, pups_dead_birth <int>,
    ## #   wt_gain <dbl>
