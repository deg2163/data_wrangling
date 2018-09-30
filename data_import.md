Data Import
================
Diana Ballesteros
9/18/2018

Import FAS csv filed
--------------------

Import my first csv ('FAS\_litters.csv)

``` r
litters_data = read_csv(file = "./data/FAS_litters.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_integer(),
    ##   `Pups born alive` = col_integer(),
    ##   `Pups dead @ birth` = col_integer(),
    ##   `Pups survive` = col_integer()
    ## )

``` r
## Convert to lower snake case 
litters_data = janitor::clean_names(litters_data)
```

``` r
##Learning Assessment
pups_data = read_csv(file = "./data/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_integer(),
    ##   `PD ears` = col_integer(),
    ##   `PD eyes` = col_integer(),
    ##   `PD pivot` = col_integer(),
    ##   `PD walk` = col_integer()
    ## )

``` r
## Convert to lower snake case 
pups_data = janitor::clean_names(pups_data)
```

``` r
##Allows you to view the data 
litters_data
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #3/83/3-3           NA          NA            20               9
    ##  9 Con8  #2/95/3             NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
##allows you to see the top 10 rows of the data 
head(litters_data)
```

    ## # A tibble: 6 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
##allows you to see the bottom rows of the data, often good to check the first and last rows to make sure you don't have empty rows 
tail(litters_data, 5)
```

    ## # A tibble: 5 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Low8  #100                20          39.2          20               8
    ## 2 Low8  #4/84               21.8        35.2          20               4
    ## 3 Low8  #108                25.6        47.5          20               8
    ## 4 Low8  #99                 23.5        39            20               6
    ## 5 Low8  #110                25.5        42.7          20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
##allows you to take a look at the data briefly skmir is another package
skimr::skim(litters_data)
```

    ## Skim summary statistics
    ##  n obs: 49 
    ##  n variables: 8 
    ## 
    ## ── Variable type:character ──────────────────────────────────────────────────────────────────────────────────────────
    ##       variable missing complete  n min max empty n_unique
    ##          group       0       49 49   4   4     0        6
    ##  litter_number       0       49 49   3  15     0       49
    ## 
    ## ── Variable type:integer ────────────────────────────────────────────────────────────────────────────────────────────
    ##         variable missing complete  n  mean   sd p0 p25 p50 p75 p100
    ##      gd_of_birth       0       49 49 19.65 0.48 19  19  20  20   20
    ##  pups_born_alive       0       49 49  7.35 1.76  3   6   8   8   11
    ##  pups_dead_birth       0       49 49  0.33 0.75  0   0   0   0    4
    ##     pups_survive       0       49 49  6.41 2.05  1   5   7   8    9
    ##      hist
    ##  ▅▁▁▁▁▁▁▇
    ##  ▂▂▃▃▇▅▁▁
    ##  ▇▂▁▁▁▁▁▁
    ##  ▂▂▃▃▅▇▇▅
    ## 
    ## ── Variable type:numeric ────────────────────────────────────────────────────────────────────────────────────────────
    ##     variable missing complete  n  mean   sd   p0   p25   p50   p75 p100
    ##   gd0_weight      15       34 49 24.38 3.28 17   22.3  24.1  26.67 33.4
    ##  gd18_weight      17       32 49 41.52 4.05 33.4 38.88 42.25 43.8  52.7
    ##      hist
    ##  ▁▃▇▇▇▆▁▁
    ##  ▂▃▃▇▆▂▁▁

You can use View(litters\_data), but it won't allow you to knit it. It is just useful to use when you want to take a look at the entire dataset.

Skip some rows; omit variable names

``` r
litters_data = read_csv(file = "./data/FAS_litters.csv",
  skip = 10, col_names = FALSE)
```

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_character(),
    ##   X2 = col_character(),
    ##   X3 = col_double(),
    ##   X4 = col_double(),
    ##   X5 = col_integer(),
    ##   X6 = col_integer(),
    ##   X7 = col_integer(),
    ##   X8 = col_integer()
    ## )

``` r
##allows you to tell the data set what data types each column is 
litters_data = read_csv(file = "./data/FAS_litters.csv",
  col_types = cols(
    Group = col_character(),
    `Litter Number` = col_character(),
    `GD0 weight` = col_double(),
    `GD12 weight` = col_double(),
    `GD of Birth` = col_integer(),
    `Pups born alive` = col_integer(),
    `Pups dead @ birth` = col_integer(),
    `Pups survive` = col_integer()
  )
)
```

    ## Warning: The following named parsers don't match the column names: GD12
    ## weight

``` r
##allows you to tell the data set what data tupe each is (short-hand)
litters_data = read_csv(file = "./data/FAS_litters.csv",
  col_types = "ccddiiii"
)
```

``` r
##Learning Assessment 

pups_data
```

    ## # A tibble: 313 x 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <int>   <int>   <int>    <int>   <int>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # ... with 303 more rows

``` r
pups_data = read_csv("./data/FAS_pups.csv", col_types = "ciiiii")
pups_data = janitor::clean_names(pups_data)
skimr::skim(pups_data)
```

    ## Skim summary statistics
    ##  n obs: 313 
    ##  n variables: 6 
    ## 
    ## ── Variable type:character ──────────────────────────────────────────────────────────────────────────────────────────
    ##       variable missing complete   n min max empty n_unique
    ##  litter_number       0      313 313   3  15     0       49
    ## 
    ## ── Variable type:integer ────────────────────────────────────────────────────────────────────────────────────────────
    ##  variable missing complete   n  mean   sd p0 p25 p50 p75 p100     hist
    ##   pd_ears      18      295 313  3.68 0.59  2   3   4   4    5 ▁▁▅▁▁▇▁▁
    ##   pd_eyes      13      300 313 12.99 0.62 12  13  13  13   15 ▂▁▇▁▁▂▁▁
    ##  pd_pivot      13      300 313  7.09 1.51  4   6   7   8   12 ▃▆▇▃▂▁▁▁
    ##   pd_walk       0      313 313  9.5  1.34  7   9   9  10   14 ▁▅▇▅▃▂▁▁
    ##       sex       0      313 313  1.5  0.5   1   1   2   2    2 ▇▁▁▁▁▁▁▇

Other Formats
-------------

Read in mlb data

``` r
library(readxl)

## you can either load the library or do it this way
mlb_data = read_excel (path = "./data/mlb11.xlsx", n_max = 20)
head(mlb_data, 5)
```

    ## # A tibble: 5 x 12
    ##   team   runs at_bats  hits homeruns bat_avg strikeouts stolen_bases  wins
    ##   <chr> <dbl>   <dbl> <dbl>    <dbl>   <dbl>      <dbl>        <dbl> <dbl>
    ## 1 Texa…   855    5659  1599      210   0.283        930          143    96
    ## 2 Bost…   875    5710  1600      203   0.28        1108          102    90
    ## 3 Detr…   787    5563  1540      169   0.277       1143           49    95
    ## 4 Kans…   730    5672  1560      129   0.275       1006          153    71
    ## 5 St. …   762    5532  1513      162   0.273        978           57    90
    ## # ... with 3 more variables: new_onbase <dbl>, new_slug <dbl>,
    ## #   new_obs <dbl>

``` r
##if you want to take a subset 
mlb_subset = read_excel(path = "./data/mlb11.xlsx", 
                        range = "A1:E17")
```

``` r
## Another way to do what I did: 
  
#### you can either load the library or do it this way
mlb_data = readxl::read_excel(path = "./data/mlb11.xlsx")

##if you want to take a subset 
mlb_subset = readxl::read_excel(path = "./data/mlb11.xlsx", range = "A1:E17")
```

Read in Pulse Data Set
----------------------

``` r
##helpful when you get data from sas/spss/stats 
library(haven)
pulse_data = read_sas("./data/public_pulse_data.sas7bdat")

##another way to do it 
pulse_data = haven::read_sas("./data/public_pulse_data.sas7bdat")
```

Compare ith base R
------------------

``` r
## preferred 
pups_readr = read_csv("./data/Fas_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_integer(),
    ##   `PD ears` = col_integer(),
    ##   `PD eyes` = col_integer(),
    ##   `PD pivot` = col_integer(),
    ##   `PD walk` = col_integer()
    ## )

``` r
##won't work, so it's a good thing sinc eyou don't want it guessing 
pups_readr$S
```

    ## Warning: Unknown or uninitialised column: 'S'.

    ## NULL

``` r
## don't use - shows a long list of ouput
pups_baser = read.csv("./data/Fas_pups.csv")

##it guesses which column starts with an and assumes it since there is only one 
pups_baser$S
```

    ##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
    ##  [36] 2 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2
    ##  [71] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1
    ## [106] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
    ## [141] 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## [176] 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
    ## [211] 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2
    ## [246] 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## [281] 1 1 1 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
