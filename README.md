
<!-- README.md is generated from README.Rmd. Please edit that file -->
glue
====

[![Travis-CI Build Status](https://travis-ci.org/tidyverse/glue.svg?branch=master)](https://travis-ci.org/tidyverse/glue) [![Coverage Status](https://img.shields.io/codecov/c/github/tidyverse/glue/master.svg)](https://codecov.io/github/tidyverse/glue?branch=master)

Glue strings to data in R. Small, fast, dependency free interpreted string literals.

Installation
------------

``` r
# install.packages("devtools")
devtools::install_github("tidyverse/glue")
```

Usage
-----

###### Long strings can be broken by line and will be concatenated together

``` r
name <- "Fred"
age <- 50
anniversary <- as.Date("1991-10-12")
glue('My name is {name},',
  ' my age next year is {age + 1},',
  ' my anniversary is {format(anniversary, "%A, %B %d, %Y")}.')
#> My name is Fred, my age next year is 51, my anniversary is Saturday, October 12, 1991.
```

You can use named arguments to assign temporary variables

``` r
glue('My name is {name},',
  ' my age next year is {age + 1},',
  ' my anniversary is {format(anniversary, "%A, %B %d, %Y")}.',
  name = "Joe",
  age = 40,
  anniversary = as.Date("2001-10-12"))
#> My name is Joe, my age next year is 41, my anniversary is Friday, October 12, 2001.
```

`glue_data()` is useful in [magrittr](https://cran.r-project.org/package=magrittr) pipes.

``` r
`%>%` <- magrittr::`%>%`
head(mtcars) %>% glue_data("{rownames(.)} has {hp} hp")
#> Mazda RX4 has 110 hp
#> Mazda RX4 Wag has 110 hp
#> Datsun 710 has 93 hp
#> Hornet 4 Drive has 110 hp
#> Hornet Sportabout has 175 hp
#> Valiant has 105 hp
```

Leading whitespace and blank lines are automatically trimmed, which lets you indent the strings naturally. You can use `\\n` to explicitly keep a leading or trailing newline.

``` r
glue("
    A formatted string
    Can have multiple lines
      with additional indention preserved
    ")
#> A formatted string
#> Can have multiple lines
#>   with additional indention preserved

glue("
  \\ntrailing or leading newlines can be added explicitly\\n
  ")
#> 
#> trailing or leading newlines can be added explicitly
```

You can use `\\` at the end of a line to continue a line without adding a new line.

``` r
glue("
    A formatted string \\
    can also be on a \\
    single line
    ")
#> A formatted string can also be on a single line
```

A literal brace can be inserted by using doubled braces.

``` r
name <- "Fred"
glue("My name is {name}, not {{name}}.")
#> My name is Fred, not {name}.
```

All valid R code works in expressions, including braces and escaping. Backslashes do need to be doubled just like in all R strings.

``` r
  `foo}\`` <- "foo"
glue("{
      {
        '}\\'' # { and } in comments, single quotes
        \"}\\\"\" # or double quotes are ignored
        `foo}\\`` # as are { in backticks
      }
  }")
#> foo
```

Other implementations
=====================

Some other implementations of string interpolation in R (although not using identical syntax).

-   [stringr::str\_interp](http://stringr.tidyverse.org/reference/str_interp.html)
-   [pystr::pystr\_format](https://cran.r-project.org/package=pystr)
-   [R.utils::gstring](https://cran.r-project.org/package=R.utils)
