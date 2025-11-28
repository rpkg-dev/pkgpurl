# pkgpurl's default lintr exclusions

Returns an opinionated set of files and folders to be excluded from
linting, relative to the root of the package directory. To be used with
[`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md),
[`lintr::lint_dir()`](https://lintr.r-lib.org/reference/lint.html) or
[`lintr::lint_package()`](https://lintr.r-lib.org/reference/lint.html).

## Usage

``` r
default_exclusions(excl_inst = TRUE, excl_vignettes = TRUE)
```

## Arguments

- excl_inst:

  Whether or not to exclude all files under `inst/`. A logical scalar.

- excl_vignettes:

  Whether or not to exclude all files under `vignettes/`. A logical
  scalar.

## Value

A named list of
[lintr::linters](https://lintr.r-lib.org/reference/linters.html).

## See also

[`default_linters`](https://pkgpurl.rpkg.dev/dev/reference/default_linters.md)
and [`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md)

## Examples

``` r
pkgpurl::default_exclusions()
#>  [1] "docs"             "input"            "inst"             "output"           "packrat"          "pkgdown"          "renv"             "tests"           
#>  [9] "vignettes"        "R/*.gen.R"        "Rmd/*.nopurl.Rmd" "README.Rmd"      
```
