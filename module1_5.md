rcode_memo
================
Yuki Joyama
2023-09-25

# Module 1

``` r
library(readr)

# read and name data
low_birth = read_csv("Module 01/lowbwt_Low.csv")
norm_birth = read_csv("Module 01/lowbwt_Normal.csv")
```

``` r
# Check for number of missing values
sum(is.na(low_birth))
```

    ## [1] 0

``` r
# Examine the classes of each column
str(low_birth)
```

    ## spc_tbl_ [59 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ id   : num [1:59] 31 76 44 68 23 45 51 49 71 83 ...
    ##  $ smoke: num [1:59] 0 0 1 1 1 1 1 0 0 0 ...
    ##  $ age  : num [1:59] 20 20 20 17 19 17 20 18 17 17 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   id = col_double(),
    ##   ..   smoke = col_double(),
    ##   ..   age = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

## Data manipulation using `dplyr`

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# Ordering data by variable/column 'id'
arrange(low_birth, id)
```

    ## # A tibble: 59 × 3
    ##       id smoke   age
    ##    <dbl> <dbl> <dbl>
    ##  1     4     1    28
    ##  2    10     0    29
    ##  3    11     1    34
    ##  4    13     0    25
    ##  5    15     0    25
    ##  6    16     0    27
    ##  7    17     0    23
    ##  8    18     0    24
    ##  9    19     0    24
    ## 10    20     1    21
    ## # ℹ 49 more rows

``` r
# Arrange by id in descending order
arrange(low_birth, desc(id))
```

    ## # A tibble: 59 × 3
    ##       id smoke   age
    ##    <dbl> <dbl> <dbl>
    ##  1    84     1    21
    ##  2    83     0    17
    ##  3    82     1    23
    ##  4    81     0    14
    ##  5    79     1    28
    ##  6    78     1    14
    ##  7    77     1    26
    ##  8    76     0    20
    ##  9    75     0    26
    ## 10    71     0    17
    ## # ℹ 49 more rows

`case_when` function

``` r
# Use case_when function to create new age categories
# Cat 1: Age < 25; Cat 2: 25 < Age < 30. Cat 3: Age > 30
mutate(low_birth, new_age = case_when(
  age < 25 ~ 1, 
  age >= 25 & age < 30 ~2,
  age > 30 ~ 3
))
```

    ## # A tibble: 59 × 4
    ##       id smoke   age new_age
    ##    <dbl> <dbl> <dbl>   <dbl>
    ##  1    31     0    20       1
    ##  2    76     0    20       1
    ##  3    44     1    20       1
    ##  4    68     1    17       1
    ##  5    23     1    19       1
    ##  6    45     1    17       1
    ##  7    51     1    20       1
    ##  8    49     0    18       1
    ##  9    71     0    17       1
    ## 10    83     0    17       1
    ## # ℹ 49 more rows

combine data sets

``` r
# stack low_birth & norm_birth
low_and_norm = rbind(low_birth, norm_birth)

# combine by specific variable
admin_birth = read_csv("Module 01/lowbwt_Admin.csv")
```

    ## Rows: 189 Columns: 2
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (2): id, visits
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
birth_final = full_join(admin_birth, low_and_norm, by = "id")
```

export data by `write.csv(birth_final, file = "./birth_final.csv")`
