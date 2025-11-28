# pkgpurl package configuration data

A [tibble](https://tibble.tidyverse.org/reference/tbl_df-class.html)
with data about all possible pkgpurl package configuration options. See
[`funky::config_val()`](https://funky.rpkg.dev/reference/config_val.html)
for more information.

## Usage

``` r
funky_config
```

## Format

A [tibble](https://tibble.tidyverse.org/reference/tbl_df-class.html)
with the columns `key`, `default_value`, `default_value_dynamic`,
`require`, and `description`.

## Examples

``` r
pkgpurl::funky_config
#> # A tibble: 3 × 5
#>   key                  default_value default_value_dynamic require description                                                                                  
#>   <chr>                <list>        <chr>                 <lgl>   <chr>                                                                                        
#> 1 add_copyright_notice <lgl [1]>     NA                    NA      Whether or not to add a **copyright notice** at the beginning of the generated `.R` files as…
#> 2 add_license_notice   <lgl [1]>     NA                    NA      Whether or not to add a **license notice** at the beginning of the generated `.R` files as r…
#> 3 gen_pkgdown_ref      <lgl [1]>     NA                    NA      Whether or not to overwrite [pkgdown](https://pkgdown.r-lib.org/)'s [reference index](https:…
```
