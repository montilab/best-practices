
<!-- README.md is generated from README.Rmd. Please edit that file -->

# The Essentials

This is meant to serve as a very brief introduction to adopting
tidyverse packages for data manipulation in R. It is just to give you a
sense of the functionality of commonly used packages and hopefully
convince you to take a deep dive in all that tidyverse has to offer. He
we cover some essential functions that you will use on a daily basis as
well as some useful R tricks and tips.

``` r
library(magrittr)
library(dplyr)
library(tidyr)
library(purrr)
library(janitor)
library(glue)
library(reshape2)
```

Or just use `tidyverse`

``` r
library(tidyverse)
```

A fun penguins dataset for examples

``` r
library(palmerpenguins)
df <- penguins
```

## Data Cleaning

``` r
# What if the names were ugly?
colnames(df) <- c("Species", "ISLAND", "Bill_Length$(MM)", "bill.depth.mm", "flipper.length.mm", "body.mass.(g)", "SEX", "YEAR")
head(df)
```

    # A tibble: 6 x 8
      Species ISLAND `Bill_Length$(M… bill.depth.mm flipper.length.… `body.mass.(g)`
      <fct>   <fct>             <dbl>         <dbl>            <int>           <int>
    1 Adelie  Torge…             39.1          18.7              181            3750
    2 Adelie  Torge…             39.5          17.4              186            3800
    3 Adelie  Torge…             40.3          18                195            3250
    4 Adelie  Torge…             NA            NA                 NA              NA
    5 Adelie  Torge…             36.7          19.3              193            3450
    6 Adelie  Torge…             39.3          20.6              190            3650
    # … with 2 more variables: SEX <fct>, YEAR <int>

``` r
df <- janitor::clean_names(df)
head(df)
```

    # A tibble: 6 x 8
      species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
      <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    1 Adelie  Torge…           39.1          18.7              181        3750 male 
    2 Adelie  Torge…           39.5          17.4              186        3800 fema…
    3 Adelie  Torge…           40.3          18                195        3250 fema…
    4 Adelie  Torge…           NA            NA                 NA          NA <NA> 
    5 Adelie  Torge…           36.7          19.3              193        3450 fema…
    6 Adelie  Torge…           39.3          20.6              190        3650 male 
    # … with 1 more variable: year <int>

``` r
summary(penguins)
```

``` 
      species          island    bill_length_mm  bill_depth_mm  
 Adelie   :152   Biscoe   :168   Min.   :32.10   Min.   :13.10  
 Chinstrap: 68   Dream    :124   1st Qu.:39.23   1st Qu.:15.60  
 Gentoo   :124   Torgersen: 52   Median :44.45   Median :17.30  
                                 Mean   :43.92   Mean   :17.15  
                                 3rd Qu.:48.50   3rd Qu.:18.70  
                                 Max.   :59.60   Max.   :21.50  
                                 NA's   :2       NA's   :2      
 flipper_length_mm  body_mass_g       sex           year     
 Min.   :172.0     Min.   :2700   female:165   Min.   :2007  
 1st Qu.:190.0     1st Qu.:3550   male  :168   1st Qu.:2007  
 Median :197.0     Median :4050   NA's  : 11   Median :2008  
 Mean   :200.9     Mean   :4202                Mean   :2008  
 3rd Qu.:213.0     3rd Qu.:4750                3rd Qu.:2009  
 Max.   :231.0     Max.   :6300                Max.   :2009  
 NA's   :2         NA's   :2                                 
```

## `magrittr`

**`%>%`**

Here we want to find the average bill length in inches.

``` r
# Fine
mean_bill_length_in <- mean(df$bill_length_mm * 0.0393701, na.rm=TRUE)
print(mean_bill_length_in)
```

    [1] 1.729211

``` r
# Better
mean_bill_length_in <- df %>%
  dplyr::pull(bill_length_mm) %>%
  magrittr::multiply_by(0.0393701) %>%
  mean(na.rm=TRUE)

print(mean_bill_length_in)
```

    [1] 1.729211

**`set_colnames()`**

``` r
df %>%
magrittr::set_colnames(paste0("Column_", seq(ncol(.)))) %>%
head()
```

    # A tibble: 6 x 8
      Column_1 Column_2  Column_3 Column_4 Column_5 Column_6 Column_7 Column_8
      <fct>    <fct>        <dbl>    <dbl>    <int>    <int> <fct>       <int>
    1 Adelie   Torgersen     39.1     18.7      181     3750 male         2007
    2 Adelie   Torgersen     39.5     17.4      186     3800 female       2007
    3 Adelie   Torgersen     40.3     18        195     3250 female       2007
    4 Adelie   Torgersen     NA       NA         NA       NA <NA>         2007
    5 Adelie   Torgersen     36.7     19.3      193     3450 female       2007
    6 Adelie   Torgersen     39.3     20.6      190     3650 male         2007

You can also insert short hand functions\!

``` r
df %>%
dplyr::pull(bill_length_mm) %>%
(function(x) {
  return(x * 0.0393701)
}) %>%
mean(na.rm=TRUE)
```

    [1] 1.729211

## `dplyr`

Please check out the [full
reference](https://dplyr.tidyverse.org/reference/index.html) for more
functions

**`mutate()`**

Modify a new or existing column

``` r
df %>%
dplyr::mutate(bill_length_in = bill_length_mm * 0.0393701) %>%
head()
```

    # A tibble: 6 x 9
      species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
      <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    1 Adelie  Torge…           39.1          18.7              181        3750 male 
    2 Adelie  Torge…           39.5          17.4              186        3800 fema…
    3 Adelie  Torge…           40.3          18                195        3250 fema…
    4 Adelie  Torge…           NA            NA                 NA          NA <NA> 
    5 Adelie  Torge…           36.7          19.3              193        3450 fema…
    6 Adelie  Torge…           39.3          20.6              190        3650 male 
    # … with 2 more variables: year <int>, bill_length_in <dbl>

**`filter()`**

Filter rows

``` r
df %>%
dplyr::filter(species == "Chinstrap") %>%
head()
```

    # A tibble: 6 x 8
      species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
      <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    1 Chinst… Dream            46.5          17.9              192        3500 fema…
    2 Chinst… Dream            50            19.5              196        3900 male 
    3 Chinst… Dream            51.3          19.2              193        3650 male 
    4 Chinst… Dream            45.4          18.7              188        3525 fema…
    5 Chinst… Dream            52.7          19.8              197        3725 male 
    6 Chinst… Dream            45.2          17.8              198        3950 fema…
    # … with 1 more variable: year <int>

**`select()`**

Select columns

``` r
df %>%
dplyr::select(species, body_mass_g) %>%
head()
```

    # A tibble: 6 x 2
      species body_mass_g
      <fct>         <int>
    1 Adelie         3750
    2 Adelie         3800
    3 Adelie         3250
    4 Adelie           NA
    5 Adelie         3450
    6 Adelie         3650

**`pull()`**

Select columns and flatten to vector

``` r
df %>%
dplyr::pull(body_mass_g) %>%
head()
```

    [1] 3750 3800 3250   NA 3450 3650

**`arrange()`**

Order rows

``` r
df %>%
dplyr::arrange(desc(body_mass_g)) %>%
head()
```

    # A tibble: 6 x 8
      species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
      <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    1 Gentoo  Biscoe           49.2          15.2              221        6300 male 
    2 Gentoo  Biscoe           59.6          17                230        6050 male 
    3 Gentoo  Biscoe           51.1          16.3              220        6000 male 
    4 Gentoo  Biscoe           48.8          16.2              222        6000 male 
    5 Gentoo  Biscoe           45.2          16.4              223        5950 male 
    6 Gentoo  Biscoe           49.8          15.9              229        5950 male 
    # … with 1 more variable: year <int>

**`across()`**

Apply function across columns

``` r
df %>%
dplyr::mutate(dplyr::across(c(bill_length_mm, bill_depth_mm), round)) %>%
head()
```

    # A tibble: 6 x 8
      species island bill_length_mm bill_depth_mm flipper_length_… body_mass_g sex  
      <fct>   <fct>           <dbl>         <dbl>            <int>       <int> <fct>
    1 Adelie  Torge…             39            19              181        3750 male 
    2 Adelie  Torge…             40            17              186        3800 fema…
    3 Adelie  Torge…             40            18              195        3250 fema…
    4 Adelie  Torge…             NA            NA               NA          NA <NA> 
    5 Adelie  Torge…             37            19              193        3450 fema…
    6 Adelie  Torge…             39            21              190        3650 male 
    # … with 1 more variable: year <int>

**`group_by()` + `summarise()`** Group by columns and compute summary
statistics

``` r
df %>%
dplyr::group_by(species) %>%
dplyr::summarise(n=n(), mass=mean(body_mass_g, na.rm=TRUE))
```

    # A tibble: 3 x 3
      species       n  mass
      <fct>     <int> <dbl>
    1 Adelie      152 3701.
    2 Chinstrap    68 3733.
    3 Gentoo      124 5076.

**Conditional mutate**

``` r
df %>%
dplyr::mutate(body_size=
      dplyr::case_when(body_mass_g > 5000 ~ "Massive",
                       body_mass_g > 4000 ~ "Big",
                       TRUE ~ "Small")) %>%
dplyr::pull(body_size) %>%
table()
```

    .
        Big Massive   Small 
        111      61     172 

## `purrr`

**Mutate with two or more columns**

``` r
df.xyz <- data.frame(x=c(1,3), y=c(1,3), z=c(1,3))
head(df.xyz)
```

``` 
  x y z
1 1 1 1
2 3 3 3
```

``` r
df.xyz <- df.xyz %>%
      dplyr::mutate(xyz = purrr::pmap(list(x, y, z), function(a, b, c) {
          a + b + c
      }))

head(df.xyz)
```

``` 
  x y z xyz
1 1 1 1   3
2 3 3 3   9
```

## `glue`

String manipulation and formatting

``` r
name <- "Fred"
glue('My name is {name}.')
```

    My name is Fred.

``` r
df %>%
dplyr::select(species, bill_length_mm, sex) %>%
tidyr::drop_na() %>%
dplyr::mutate(desc = glue("This {sex} penguin ({species}) has a bill length of {bill_length_mm} mm")) %>%
head()
```

    # A tibble: 6 x 4
      species bill_length_mm sex    desc                                            
      <fct>            <dbl> <fct>  <glue>                                          
    1 Adelie            39.1 male   This male penguin (Adelie) has a bill length of…
    2 Adelie            39.5 female This female penguin (Adelie) has a bill length …
    3 Adelie            40.3 female This female penguin (Adelie) has a bill length …
    4 Adelie            36.7 female This female penguin (Adelie) has a bill length …
    5 Adelie            39.3 male   This male penguin (Adelie) has a bill length of…
    6 Adelie            38.9 female This female penguin (Adelie) has a bill length …

## `reshape2`

**`melt()`**

Melt a data frame for visualization

``` r
df %>%
dplyr::select(species, bill_length_mm, bill_depth_mm) %>%
reshape2::melt(id=c("species")) %>%
head(5)
```

``` 
  species       variable value
1  Adelie bill_length_mm  39.1
2  Adelie bill_length_mm  39.5
3  Adelie bill_length_mm  40.3
4  Adelie bill_length_mm    NA
5  Adelie bill_length_mm  36.7
```

## Extra helpers

**Not in**

``` r
`%not_in%` <- Negate(`%in%`)
```

**Not null**

``` r
not_null <- Negate(is.null)
```

**Not NA**

``` r
not_na <- Negate(is.na)
```

**Remove nulls from a vector**

``` r
drop_nulls <- function(x){
  x[!sapply(x, is.null)]
}
```

**Iterate through all files in directory**

``` r
files <- list.files(path=path.to.dir, pattern="*.pattern", full.names=TRUE, recursive=FALSE)
lapply(files, function(fn) {
  do.something(fn)
})
```

**Apply function to a list**

``` r
is.this <- function(x) {
  if (x == this) {
    return(F)
  } else {
    return(T)
  }
}
lapply(alist, function(x) is.this(x))
```
