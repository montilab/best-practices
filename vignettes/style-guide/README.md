
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Style Guide

Here are some basic tips for maintaining consistent coding style and
formatting across lab projects. These are specifically for the R
programming language but generally apply across languages and
environments. Some of these tips will depend on your preference, but
always be consistent in whatever style you choose. If you’re
contributing to an existing project **always look at the existing style
and adhere accordingly**.

-----

  - Write in UTF-8
  - Always use 4 spaces for indentation (don’t use tabs)
  - Try to limit line length to 120 characters
  - Use modular architecture to group similar functions etc.
  - Always document your functions and objects with `roxygen2`
  - Instead of `@import dplyr` just import what the function needs
    `@importFrom dplyr filter`
  - Object definitions should always be uppercase and use `snake_case`
  - Functions and variables should always be lowercase and use
    `snake_case`
  - Use `return` statements in your functions, don’t depend on R to
    infer what should be returned.
  - Private functions should begin with a `.` like `.private_function()`
  - Explicit bools should use `TRUE`/`FALSE` instead of `T`/`F`
  - Use `<-` for assignment instead of `=`
  - Omit trailing semicolons from code lines
  - Avoid `sapply()` - it is not type safe. Use `vapply()` or `mapply()`
  - Avoid using `1:10` - use `seq_len()` or `seq_along()`
  - Try to use `tidyverse` packages to manipulate data - particularly
    `magrittr`, `dplyr`, and `purrr`
  - Use the pipe function `%>%` instead of nested functions
  - If you need to refer to absolute paths on your device (e.g. private
    data) use `Sys.getenv(envar="")`
