Data Manipulation
================
Diana Ballesteros
9/20/2018

Import my csv files as examples
-------------------------------

``` r
## loading the relative path 
litters_data = read_csv("./data/FAS_litters.csv",
  col_types = "ccddiiii")
litters_data = janitor::clean_names(litters_data)

pups_data = read_csv("./data/FAS_pups.csv",
  col_types = "ciiiii")
pups_data = janitor::clean_names(pups_data)
```

Select Variables
----------------

...by listing

``` r
##?select allows you to check what select does
## select(data frame, which group you want to select separated by commas)
select(litters_data, group, litter_number, gd0_weight, pups_survive)
```

    ## # A tibble: 49 x 4
    ##    group litter_number   gd0_weight pups_survive
    ##    <chr> <chr>                <dbl>        <int>
    ##  1 Con7  #85                   19.7            3
    ##  2 Con7  #1/2/95/2             27              7
    ##  3 Con7  #5/5/3/83/3-3         26              5
    ##  4 Con7  #5/4/2/95/2           28.5            4
    ##  5 Con7  #4/2/95/3-3           NA              6
    ##  6 Con7  #2/2/95/3-2           NA              4
    ##  7 Con7  #1/5/3/83/3-3/2       NA              9
    ##  8 Con8  #3/83/3-3             NA              8
    ##  9 Con8  #2/95/3               NA              8
    ## 10 Con8  #3/5/2/2/95           28.5            8
    ## # ... with 39 more rows

``` r
##selecting a different selection of variables where you extract the variables I am interested in
select(litters_data, litter_number, gd0_weight, pups_survive)
```

    ## # A tibble: 49 x 3
    ##    litter_number   gd0_weight pups_survive
    ##    <chr>                <dbl>        <int>
    ##  1 #85                   19.7            3
    ##  2 #1/2/95/2             27              7
    ##  3 #5/5/3/83/3-3         26              5
    ##  4 #5/4/2/95/2           28.5            4
    ##  5 #4/2/95/3-3           NA              6
    ##  6 #2/2/95/3-2           NA              4
    ##  7 #1/5/3/83/3-3/2       NA              9
    ##  8 #3/83/3-3             NA              8
    ##  9 #2/95/3               NA              8
    ## 10 #3/5/2/2/95           28.5            8
    ## # ... with 39 more rows

...by specifying a range:

``` r
##allows you to select a range 
select(litters_data, gd_of_birth:pups_survive)
```

    ## # A tibble: 49 x 4
    ##    gd_of_birth pups_born_alive pups_dead_birth pups_survive
    ##          <int>           <int>           <int>        <int>
    ##  1          20               3               4            3
    ##  2          19               8               0            7
    ##  3          19               6               0            5
    ##  4          19               5               1            4
    ##  5          20               6               0            6
    ##  6          20               6               0            4
    ##  7          20               9               0            9
    ##  8          20               9               1            8
    ##  9          20               8               0            8
    ## 10          20               8               0            8
    ## # ... with 39 more rows

``` r
##allows you to select a range and another list 
select(litters_data, group, gd_of_birth:pups_survive)
```

    ## # A tibble: 49 x 5
    ##    group gd_of_birth pups_born_alive pups_dead_birth pups_survive
    ##    <chr>       <int>           <int>           <int>        <int>
    ##  1 Con7           20               3               4            3
    ##  2 Con7           19               8               0            7
    ##  3 Con7           19               6               0            5
    ##  4 Con7           19               5               1            4
    ##  5 Con7           20               6               0            6
    ##  6 Con7           20               6               0            4
    ##  7 Con7           20               9               0            9
    ##  8 Con8           20               9               1            8
    ##  9 Con8           20               8               0            8
    ## 10 Con8           20               8               0            8
    ## # ... with 39 more rows

...by saying you want to remove (this will allow you to remove a column you don't want):

``` r
##allows you to keep everything else, except litter_number
select(litters_data, -litter_number)
```

    ## # A tibble: 49 x 7
    ##    group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_birth
    ##    <chr>      <dbl>       <dbl>       <int>           <int>           <int>
    ##  1 Con7        19.7        34.7          20               3               4
    ##  2 Con7        27          42            19               8               0
    ##  3 Con7        26          41.4          19               6               0
    ##  4 Con7        28.5        44.1          19               5               1
    ##  5 Con7        NA          NA            20               6               0
    ##  6 Con7        NA          NA            20               6               0
    ##  7 Con7        NA          NA            20               9               0
    ##  8 Con8        NA          NA            20               9               1
    ##  9 Con8        NA          NA            20               8               0
    ## 10 Con8        28.5        NA            20               8               0
    ## # ... with 39 more rows, and 1 more variable: pups_survive <int>

...Selecting gd\_day\_0\_weight and also renaming it as well. You can do either.

``` r
##here you are selecting gd0_weight and also renaming it 
select(litters_data, group, litter_number, gd_day_0_weight = gd0_weight)
```

    ## # A tibble: 49 x 3
    ##    group litter_number   gd_day_0_weight
    ##    <chr> <chr>                     <dbl>
    ##  1 Con7  #85                        19.7
    ##  2 Con7  #1/2/95/2                  27  
    ##  3 Con7  #5/5/3/83/3-3              26  
    ##  4 Con7  #5/4/2/95/2                28.5
    ##  5 Con7  #4/2/95/3-3                NA  
    ##  6 Con7  #2/2/95/3-2                NA  
    ##  7 Con7  #1/5/3/83/3-3/2            NA  
    ##  8 Con8  #3/83/3-3                  NA  
    ##  9 Con8  #2/95/3                    NA  
    ## 10 Con8  #3/5/2/2/95                28.5
    ## # ... with 39 more rows

``` r
##same way to rename a variable without selecting it at the same time 
rename(litters_data,  gd_day_0_weight = gd0_weight)
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd_day_0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                   <dbl>       <dbl>       <int>
    ##  1 Con7  #85                      19.7        34.7          20
    ##  2 Con7  #1/2/95/2                27          42            19
    ##  3 Con7  #5/5/3/83/3-3            26          41.4          19
    ##  4 Con7  #5/4/2/95/2              28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3              NA          NA            20
    ##  6 Con7  #2/2/95/3-2              NA          NA            20
    ##  7 Con7  #1/5/3/83/3-…            NA          NA            20
    ##  8 Con8  #3/83/3-3                NA          NA            20
    ##  9 Con8  #2/95/3                  NA          NA            20
    ## 10 Con8  #3/5/2/2/95              28.5        NA            20
    ## # ... with 39 more rows, and 3 more variables: pups_born_alive <int>,
    ## #   pups_dead_birth <int>, pups_survive <int>

....use select helpers

``` r
##?select_helpers allows you get help on how to do selec things

## allows you to select variables that start with "gd"
select(litters_data, starts_with("gd"))
```

    ## # A tibble: 49 x 3
    ##    gd0_weight gd18_weight gd_of_birth
    ##         <dbl>       <dbl>       <int>
    ##  1       19.7        34.7          20
    ##  2       27          42            19
    ##  3       26          41.4          19
    ##  4       28.5        44.1          19
    ##  5       NA          NA            20
    ##  6       NA          NA            20
    ##  7       NA          NA            20
    ##  8       NA          NA            20
    ##  9       NA          NA            20
    ## 10       28.5        NA            20
    ## # ... with 39 more rows

``` r
##allows you to select variables that start with "gd" and hte litter number 
select(litters_data, litter_number, starts_with("gd"))
```

    ## # A tibble: 49 x 4
    ##    litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr>                <dbl>       <dbl>       <int>
    ##  1 #85                   19.7        34.7          20
    ##  2 #1/2/95/2             27          42            19
    ##  3 #5/5/3/83/3-3         26          41.4          19
    ##  4 #5/4/2/95/2           28.5        44.1          19
    ##  5 #4/2/95/3-3           NA          NA            20
    ##  6 #2/2/95/3-2           NA          NA            20
    ##  7 #1/5/3/83/3-3/2       NA          NA            20
    ##  8 #3/83/3-3             NA          NA            20
    ##  9 #2/95/3               NA          NA            20
    ## 10 #3/5/2/2/95           28.5        NA            20
    ## # ... with 39 more rows

``` r
## allows you to select variables that start with "pup"
select(litters_data, starts_with("pup"))
```

    ## # A tibble: 49 x 3
    ##    pups_born_alive pups_dead_birth pups_survive
    ##              <int>           <int>        <int>
    ##  1               3               4            3
    ##  2               8               0            7
    ##  3               6               0            5
    ##  4               5               1            4
    ##  5               6               0            6
    ##  6               6               0            4
    ##  7               9               0            9
    ##  8               9               1            8
    ##  9               8               0            8
    ## 10               8               0            8
    ## # ... with 39 more rows

``` r
##helps your reorganize your dataset. In this case, you are pulling the litter_data, then telling it to bring in the litter_number as the first column, then bringing in everything else. 
select(litters_data, litter_number, everything())
```

    ## # A tibble: 49 x 8
    ##    litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>         <chr>      <dbl>       <dbl>       <int>           <int>
    ##  1 #85           Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2     Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2   Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3   Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2   Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-… Con7        NA          NA            20               9
    ##  8 #3/83/3-3     Con8        NA          NA            20               9
    ##  9 #2/95/3       Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95   Con8        28.5        NA            20               8
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

...select the columns containing litter number, sex, and PD ears.

``` r
select(pups_data, litter_number:pd_ears)
```

    ## # A tibble: 313 x 3
    ##    litter_number   sex pd_ears
    ##    <chr>         <int>   <int>
    ##  1 #85               1       4
    ##  2 #85               1       4
    ##  3 #1/2/95/2         1       5
    ##  4 #1/2/95/2         1       5
    ##  5 #5/5/3/83/3-3     1       5
    ##  6 #5/5/3/83/3-3     1       5
    ##  7 #5/4/2/95/2       1      NA
    ##  8 #4/2/95/3-3       1       4
    ##  9 #4/2/95/3-3       1       4
    ## 10 #2/2/95/3-2       1       4
    ## # ... with 303 more rows

Filter observations - eliminate some rows from my dataset
---------------------------------------------------------

gd\_of\_birth is exactly equal to 20
------------------------------------

gd\_of\_birth == 20
===================

greater than or equal to 2
--------------------------

pups\_born\_alive &gt;= 2
=========================

should be equal to each other; not equal to 4
---------------------------------------------

pups\_survive != 4
==================

!(pups\_survive == 4)
=====================

look at only the ones that are in group 7 or 8
----------------------------------------------

group %in% c("Con7", "Con8")
============================

similar to the one above
------------------------

group == "Con7" & gd\_of\_birth == 20
=====================================

are these thing NA or are these things not NA
---------------------------------------------

!is.na(wt\_increase)
====================

``` r
## gives you values less than 25 within the gd0_weight column 
filter(litters_data, gd0_weight < 25)
```

    ## # A tibble: 20 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Mod7  #59                 17          33.4          19               8
    ##  3 Mod7  #103                21.4        42.1          19               9
    ##  4 Mod7  #4/2/95/2           23.5        NA            19               9
    ##  5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  6 Mod7  #106                21.7        37.8          20               5
    ##  7 Mod7  #94/2               24.4        42.9          19               7
    ##  8 Mod7  #62                 19.5        35.9          19               7
    ##  9 Low7  #84/2               24.3        40.8          20               8
    ## 10 Low7  #107                22.6        42.4          20               9
    ## 11 Low7  #85/2               22.2        38.5          20               8
    ## 12 Low7  #98                 23.8        43.8          20               9
    ## 13 Low7  #102                22.6        43.3          20              11
    ## 14 Low7  #101                23.8        42.7          20               9
    ## 15 Low7  #112                23.9        40.5          19               6
    ## 16 Mod8  #97                 24.5        42.8          20               8
    ## 17 Low8  #53                 21.8        37.2          20               8
    ## 18 Low8  #100                20          39.2          20               8
    ## 19 Low8  #4/84               21.8        35.2          20               4
    ## 20 Low8  #99                 23.5        39            20               6
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data, gd0_weight >= 25) 
```

    ## # A tibble: 14 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  4 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  5 Con8  #5/4/3/83/3         28          NA            19               9
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Low7  #111                25.5        44.6          20               3
    ##  8 Mod8  #7/82-3-2           26.9        43.2          20               7
    ##  9 Mod8  #7/110/3-2          27.5        46            19               8
    ## 10 Mod8  #2/95/2             28.5        44.5          20               9
    ## 11 Mod8  #82/4               33.4        52.7          20               8
    ## 12 Low8  #79                 25.4        43.8          19               8
    ## 13 Low8  #108                25.6        47.5          20               8
    ## 14 Low8  #110                25.5        42.7          20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
## exactly equal to 8 
filter(litters_data, pups_born_alive == 8) 
```

    ## # A tibble: 16 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con8  #2/95/3             NA          NA            20               8
    ##  3 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  4 Con8  #3/5/3/83/3-…       NA          NA            20               8
    ##  5 Mod7  #59                 17          33.4          19               8
    ##  6 Mod7  #3/83/3-2           NA          NA            19               8
    ##  7 Low7  #84/2               24.3        40.8          20               8
    ##  8 Low7  #85/2               22.2        38.5          20               8
    ##  9 Mod8  #97                 24.5        42.8          20               8
    ## 10 Mod8  #5/93/2             NA          NA            19               8
    ## 11 Mod8  #7/110/3-2          27.5        46            19               8
    ## 12 Mod8  #82/4               33.4        52.7          20               8
    ## 13 Low8  #53                 21.8        37.2          20               8
    ## 14 Low8  #79                 25.4        43.8          19               8
    ## 15 Low8  #100                20          39.2          20               8
    ## 16 Low8  #108                25.6        47.5          20               8
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
## what it is saying: is it true that gd0_weight is missing? 
filter(litters_data, is.na(gd0_weight)) 
```

    ## # A tibble: 15 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #4/2/95/3-3           NA        NA            20               6
    ##  2 Con7  #2/2/95/3-2           NA        NA            20               6
    ##  3 Con7  #1/5/3/83/3-…         NA        NA            20               9
    ##  4 Con8  #3/83/3-3             NA        NA            20               9
    ##  5 Con8  #2/95/3               NA        NA            20               8
    ##  6 Con8  #1/6/2/2/95-2         NA        NA            20               7
    ##  7 Con8  #3/5/3/83/3-…         NA        NA            20               8
    ##  8 Con8  #2/2/95/2             NA        NA            19               5
    ##  9 Con8  #3/6/2/2/95-3         NA        NA            20               7
    ## 10 Mod7  #1/82/3-2             NA        NA            19               6
    ## 11 Mod7  #3/83/3-2             NA        NA            19               8
    ## 12 Mod7  #2/95/2-2             NA        NA            20               7
    ## 13 Mod7  #8/110/3-2            NA        NA            20               9
    ## 14 Mod8  #5/93                 NA        41.1          20              11
    ## 15 Mod8  #5/93/2               NA        NA            19               8
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
## by putting the exclamation point, you will exclude those that are NA
filter(litters_data, !is.na(gd0_weight)) 
```

    ## # A tibble: 34 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3         28          NA            19               9
    ##  7 Mod7  #59                 17          33.4          19               8
    ##  8 Mod7  #103                21.4        42.1          19               9
    ##  9 Mod7  #3/82/3-2           28          45.9          20               5
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## # ... with 24 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
## | means OR. This will give you Low8 OR Low 7 
filter(litters_data, group == "Low8" | group == "Low7")
```

    ## # A tibble: 15 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Low7  #84/2               24.3        40.8          20               8
    ##  2 Low7  #107                22.6        42.4          20               9
    ##  3 Low7  #85/2               22.2        38.5          20               8
    ##  4 Low7  #98                 23.8        43.8          20               9
    ##  5 Low7  #102                22.6        43.3          20              11
    ##  6 Low7  #101                23.8        42.7          20               9
    ##  7 Low7  #111                25.5        44.6          20               3
    ##  8 Low7  #112                23.9        40.5          19               6
    ##  9 Low8  #53                 21.8        37.2          20               8
    ## 10 Low8  #79                 25.4        43.8          19               8
    ## 11 Low8  #100                20          39.2          20               8
    ## 12 Low8  #4/84               21.8        35.2          20               4
    ## 13 Low8  #108                25.6        47.5          20               8
    ## 14 Low8  #99                 23.5        39            20               6
    ## 15 Low8  #110                25.5        42.7          20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
## does the exact same thing as above 
filter(litters_data, group %in% c("Low8","Low7")) 
```

    ## # A tibble: 15 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Low7  #84/2               24.3        40.8          20               8
    ##  2 Low7  #107                22.6        42.4          20               9
    ##  3 Low7  #85/2               22.2        38.5          20               8
    ##  4 Low7  #98                 23.8        43.8          20               9
    ##  5 Low7  #102                22.6        43.3          20              11
    ##  6 Low7  #101                23.8        42.7          20               9
    ##  7 Low7  #111                25.5        44.6          20               3
    ##  8 Low7  #112                23.9        40.5          19               6
    ##  9 Low8  #53                 21.8        37.2          20               8
    ## 10 Low8  #79                 25.4        43.8          19               8
    ## 11 Low8  #100                20          39.2          20               8
    ## 12 Low8  #4/84               21.8        35.2          20               4
    ## 13 Low8  #108                25.6        47.5          20               8
    ## 14 Low8  #99                 23.5        39            20               6
    ## 15 Low8  #110                25.5        42.7          20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
## will give you all the rows that have 4,5, or 6 within pups_born_alive 
filter(litters_data, pups_born_alive %in% 4:6)
```

    ## # A tibble: 12 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  4 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #1/82/3-2           NA          NA            19               6
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Low7  #112                23.9        40.5          19               6
    ## 11 Low8  #4/84               21.8        35.2          20               4
    ## 12 Low8  #99                 23.5        39            20               6
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
## you can also combine different things; either will do the same thing, but comma is easier to read 
filter(litters_data, pups_born_alive %in% 4:7, !is.na(gd0_weight))
```

    ## # A tibble: 12 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Mod7  #3/82/3-2           28          45.9          20               5
    ##  4 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  5 Mod7  #106                21.7        37.8          20               5
    ##  6 Mod7  #94/2               24.4        42.9          19               7
    ##  7 Mod7  #62                 19.5        35.9          19               7
    ##  8 Low7  #112                23.9        40.5          19               6
    ##  9 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 10 Low8  #4/84               21.8        35.2          20               4
    ## 11 Low8  #99                 23.5        39            20               6
    ## 12 Low8  #110                25.5        42.7          20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data, pups_born_alive %in% 4:7 & !is.na(gd0_weight))
```

    ## # A tibble: 12 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Mod7  #3/82/3-2           28          45.9          20               5
    ##  4 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  5 Mod7  #106                21.7        37.8          20               5
    ##  6 Mod7  #94/2               24.4        42.9          19               7
    ##  7 Mod7  #62                 19.5        35.9          19               7
    ##  8 Low7  #112                23.9        40.5          19               6
    ##  9 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 10 Low8  #4/84               21.8        35.2          20               4
    ## 11 Low8  #99                 23.5        39            20               6
    ## 12 Low8  #110                25.5        42.7          20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

Learning Assessment
-------------------

``` r
select(pups_data, litter_number:pd_walk)
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
filter(pups_data, sex == 1)
```

    ## # A tibble: 155 x 6
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
    ## # ... with 145 more rows

``` r
filter(pups_data, pd_walk < 11, sex == 2)
```

    ## # A tibble: 127 x 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <int>   <int>   <int>    <int>   <int>
    ##  1 #1/2/95/2         2       4      13        7       9
    ##  2 #1/2/95/2         2       4      13        7      10
    ##  3 #1/2/95/2         2       5      13        8      10
    ##  4 #1/2/95/2         2       5      13        8      10
    ##  5 #1/2/95/2         2       5      13        6      10
    ##  6 #5/5/3/83/3-3     2       5      13        8      10
    ##  7 #5/5/3/83/3-3     2       5      14        7      10
    ##  8 #5/5/3/83/3-3     2       5      14        8      10
    ##  9 #5/4/2/95/2       2      NA      14        7      10
    ## 10 #5/4/2/95/2       2      NA      14        7      10
    ## # ... with 117 more rows

Mutate: you can do multiple operations at the same time
-------------------------------------------------------

Create variable

``` r
## 1) creating a new variable based on the functions of other variable and add in the weight gain variable. 2) tolower allows you to do lower case of whatever is inside. You aren't necessary changing the table (only in the console); put them in separate lines so that it looks cleaner; new variables getting added in at the end of the dataset 
mutate(litters_data,
  wt_gain = gd18_weight - gd0_weight,
  group = tolower(group)
)
```

    ## # A tibble: 49 x 9
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 con8  #3/83/3-3           NA          NA            20               9
    ##  9 con8  #2/95/3             NA          NA            20               8
    ## 10 con8  #3/5/2/2/95         28.5        NA            20               8
    ## # ... with 39 more rows, and 3 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>, wt_gain <dbl>

``` r
## you are starting with the same info as above, but are now adding a new variable that is squaring it 
mutate(litters_data,
  wt_gain = gd18_weight - gd0_weight,
  wt_gain_squared = wt_gain^2
)
```

    ## # A tibble: 49 x 10
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
    ## # ... with 39 more rows, and 4 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>, wt_gain <dbl>, wt_gain_squared <dbl>

Learning Assessment
-------------------

``` r
##Create a variable that subtracts 7 from PD pivot
mutate(pups_data, pd_pivot_minus7 = pd_pivot - 7)
```

    ## # A tibble: 313 x 7
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pd_pivot_minus7
    ##    <chr>         <int>   <int>   <int>    <int>   <int>           <dbl>
    ##  1 #85               1       4      13        7      11               0
    ##  2 #85               1       4      13        7      12               0
    ##  3 #1/2/95/2         1       5      13        7       9               0
    ##  4 #1/2/95/2         1       5      13        8      10               1
    ##  5 #5/5/3/83/3-3     1       5      13        8      10               1
    ##  6 #5/5/3/83/3-3     1       5      14        6       9              -1
    ##  7 #5/4/2/95/2       1      NA      14        5       9              -2
    ##  8 #4/2/95/3-3       1       4      13        6       8              -1
    ##  9 #4/2/95/3-3       1       4      13        7       9               0
    ## 10 #2/2/95/3-2       1       4      NA        8      10               1
    ## # ... with 303 more rows

``` r
##Create a variable that is the sum of all the PD variables
mutate(pups_data, pd_sum_2 = pd_ears + pd_eyes + pd_pivot + pd_walk)
```

    ## # A tibble: 313 x 7
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pd_sum_2
    ##    <chr>         <int>   <int>   <int>    <int>   <int>    <int>
    ##  1 #85               1       4      13        7      11       35
    ##  2 #85               1       4      13        7      12       36
    ##  3 #1/2/95/2         1       5      13        7       9       34
    ##  4 #1/2/95/2         1       5      13        8      10       36
    ##  5 #5/5/3/83/3-3     1       5      13        8      10       36
    ##  6 #5/5/3/83/3-3     1       5      14        6       9       34
    ##  7 #5/4/2/95/2       1      NA      14        5       9       NA
    ##  8 #4/2/95/3-3       1       4      13        6       8       31
    ##  9 #4/2/95/3-3       1       4      13        7       9       33
    ## 10 #2/2/95/3-2       1       4      NA        8      10       NA
    ## # ... with 303 more rows

Arrange: you are just putting your data in a diff order
-------------------------------------------------------

...Arange the data

``` r
## ?arrange if you have any questions 

##arranges the gd0_weight from the smallest to the largest 
arrange(litters_data, gd0_weight)
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Mod7  #59                 17          33.4          19               8
    ##  2 Mod7  #62                 19.5        35.9          19               7
    ##  3 Con7  #85                 19.7        34.7          20               3
    ##  4 Low8  #100                20          39.2          20               8
    ##  5 Mod7  #103                21.4        42.1          19               9
    ##  6 Mod7  #106                21.7        37.8          20               5
    ##  7 Low8  #53                 21.8        37.2          20               8
    ##  8 Low8  #4/84               21.8        35.2          20               4
    ##  9 Low7  #85/2               22.2        38.5          20               8
    ## 10 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
##arranges by multiple things, first by pups_born_alive, then by gd0_weight
arrange(litters_data, pups_born_alive, gd0_weight)
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Low7  #111                25.5        44.6          20               3
    ##  3 Low8  #4/84               21.8        35.2          20               4
    ##  4 Mod7  #106                21.7        37.8          20               5
    ##  5 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  6 Mod7  #3/82/3-2           28          45.9          20               5
    ##  7 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  8 Con8  #2/2/95/2           NA          NA            19               5
    ##  9 Low8  #99                 23.5        39            20               6
    ## 10 Low7  #112                23.9        40.5          19               6
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
### desc makes it in descending order
arrange(litters_data, desc(gd0_weight), pups_born_alive)
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Mod8  #82/4               33.4        52.7          20               8
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  4 Mod8  #2/95/2             28.5        44.5          20               9
    ##  5 Mod7  #3/82/3-2           28          45.9          20               5
    ##  6 Con8  #5/4/3/83/3         28          NA            19               9
    ##  7 Mod8  #7/110/3-2          27.5        46            19               8
    ##  8 Con7  #1/2/95/2           27          42            19               8
    ##  9 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 10 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
##what does this mean? I think it gives you the first 10 at the top of the list 
head(arrange(litters_data, group, pups_born_alive), 10)
```

    ## # A tibble: 10 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  5 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  6 Con7  #1/2/95/2           27          42            19               8
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #2/2/95/2           NA          NA            19               5
    ##  9 Con8  #1/6/2/2/95-2       NA          NA            20               7
    ## 10 Con8  #3/6/2/2/95-3       NA          NA            20               7
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
tail(arrange(litters_data, group, pups_born_alive), 10)
```

    ## # A tibble: 10 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Mod7  #103                21.4        42.1          19               9
    ##  2 Mod7  #4/2/95/2           23.5        NA            19               9
    ##  3 Mod7  #8/110/3-2          NA          NA            20               9
    ##  4 Mod8  #7/82-3-2           26.9        43.2          20               7
    ##  5 Mod8  #97                 24.5        42.8          20               8
    ##  6 Mod8  #5/93/2             NA          NA            19               8
    ##  7 Mod8  #7/110/3-2          27.5        46            19               8
    ##  8 Mod8  #82/4               33.4        52.7          20               8
    ##  9 Mod8  #2/95/2             28.5        44.5          20               9
    ## 10 Mod8  #5/93               NA          41.1          20              11
    ## # ... with 2 more variables: pups_dead_birth <int>, pups_survive <int>

Piping
------

Look at intermediate object approach
------------------------------------

``` r
## creates a whole bunch of data sets in your environment, which can be a little messy 
litters_data_raw = read_csv("./data/FAS_litters.csv",
  col_types = "ccddiiii")
litters_data_clean_names = janitor::clean_names(litters_data_raw)
litters_data_selected_cols = select(litters_data_clean_names, -pups_survive)
litters_data_with_vars = mutate(litters_data_selected_cols, 
  wt_gain = gd18_weight - gd0_weight,
  group = tolower(group))
litters_data_with_vars
```

    ## # A tibble: 49 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 con8  #3/83/3-3           NA          NA            20               9
    ##  9 con8  #2/95/3             NA          NA            20               8
    ## 10 con8  #3/5/2/2/95         28.5        NA            20               8
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   wt_gain <dbl>

``` r
##use nested funstion calls: it is being saved as litter_data_clean, gives you exactly the same as the one above. Neither is very easy to read though... 
litters_data_clean = 
  mutate(
    select(
      janitor::clean_names(
        read_csv("./data/FAS_litters.csv", col_types = "ccddiiii")
        ), 
    -pups_survive
    ),
  wt_gain = gd18_weight - gd0_weight,
  group = tolower(group)
  )
```

Use piping

``` r
## much easier to read and you can add as much as you want to it
litters_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = tolower(group)
    ) %>% 
    filter(!is.na(gd0_weight))  
```

Illustrate placeholder use with "lm" The dot is just a placeholder for the code prior

``` r
## ?lm to help you with questions regarding a linear model 
## allows you fit in your linear mode (I'm still a little confused about this)
litters_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = tolower(group)
    ) %>% 
    filter(!is.na(gd0_weight)) %>%   
    lm(gd18_weight ~ gd0_weight, data = .)
    

## Another way to do the above

clean_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%  
  select(., -pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = tolower(group)
    ) %>% 
    filter(!is.na(gd0_weight)) %>%   
    lm(gd18_weight ~ gd0_weight, data = .)
## Another way to do the above if you are not starting off with a dataframe: 
litters_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names(dat = .) %>%
  select(.data = ., -pups_survive) %>%
  mutate(.data = .,
    wt_gain = gd18_weight - gd0_weight,
    group = tolower(group)) %>% 
  filter(.data = ., !is.na(gd0_weight)) %>% 
  lm(gd18_weight ~ gd0_weight, data = .)
```

Learning Assessment
-------------------

``` r
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names(.) %>%
  filter(sex == 1) %>% 
  select(-pd_ears) %>% 
  mutate(pd_pivot_gt7 = pd_pivot > 7)
```

    ## # A tibble: 155 x 6
    ##    litter_number   sex pd_eyes pd_pivot pd_walk pd_pivot_gt7
    ##    <chr>         <int>   <int>    <int>   <int> <lgl>       
    ##  1 #85               1      13        7      11 FALSE       
    ##  2 #85               1      13        7      12 FALSE       
    ##  3 #1/2/95/2         1      13        7       9 FALSE       
    ##  4 #1/2/95/2         1      13        8      10 TRUE        
    ##  5 #5/5/3/83/3-3     1      13        8      10 TRUE        
    ##  6 #5/5/3/83/3-3     1      14        6       9 FALSE       
    ##  7 #5/4/2/95/2       1      14        5       9 FALSE       
    ##  8 #4/2/95/3-3       1      13        6       8 FALSE       
    ##  9 #4/2/95/3-3       1      13        7       9 FALSE       
    ## 10 #2/2/95/3-2       1      NA        8      10 TRUE        
    ## # ... with 145 more rows
