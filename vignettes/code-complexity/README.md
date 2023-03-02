
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Cyclomatic Complexity of R (or any) Code

Cyclomatic complexity is a measurement developed by Thomas McCabe in
1976 to determine the stability and level of confidence in a program. It
measures the number of linearly-independent paths through a program
module. Programs with lower Cyclomatic complexity are easier to
understand and less risky to modify.

The way Cyclomatic complexity is computed is explained
[here](https://www.bbau.ac.in/dept/CS/TM/Cyclomatic.pdf)

The recommended complexity value is 10 or below, though in some
circumstances, complexity as high as 15 is considered acceptable.

# cyclocomp R package

Functions that do not have any conditions or branches has complexity of
1

``` r
library(cyclocomp)
```

    Warning: package 'cyclocomp' was built under R version 4.1.3

``` r
cyclocomp(
  
  
  my_summary <- function(x, y){
    ave.x <- mean(x, na.rm=T)
    ave.y <- mean(y, na.rm=T)
    xy.cor <- cor(x,y)
    c(ave_x = ave.x, ave_y = ave.y, xy_cor=xy.cor)
  }
  
)
```

    [1] 1

An example with one if() statement

``` r
cyclocomp(
  
  
  my_if <- function(x){
    if (any(is.na(x))) 0 else mean(x)
  }
  
)
```

    [1] 2

More complex example with multiple conditions

``` r
cyclocomp(
  
  
  my_if2 <- function(x){
    
    if (any(is.na(x)) | length(x) < 1){
      print("Error message")
      return(NA)
    } else {
      res <- mean(x)
    }
    
    if ( res == 0 ) print("warning") else return(res)
  }
  
)
```

    [1] 4

Every loop adds additional complexity to the code as there is a
condition that needs to be checked while loop is executing

``` r
cyclocomp(
  
  my_for <- function(x){
    
    for (i in seq_along(x)){
      x[i] <- x[i] * i + i
    }
    
  }
  
)
```

    [1] 3

# Using cyclocomp R package for R packages

``` r
cyclocomp_package("cyclocomp")
```

                        name cyclocomp
    6              flowgraph        60
    11          post_process        10
    12   report_system_error         4
    13           what_atomic         3
    1          build_package         2
    10              is_child         2
    14             what_expr         2
    2              cyclocomp         1
    3      cyclocomp_package         1
    4  cyclocomp_package_dir         1
    5            cyclocomp_q         1
    7                   id.1         1
    8                   id.2         1
    9                   id.3         1

If you are in the R project where you develop an R package, you can
compute the project complexity using `cyclocomp_package_dir()` function:

``` r
cyclocomp_package_dir(path = ".")
```
