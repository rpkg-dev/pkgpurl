# Determine a package's main `.Rmd` source file

Determines which R Markdown file under `Rmd/` in `path` is the package's
main source file. If multiple `.Rmd` files are present, the one whose
name matches the package name is selected.

## Usage

``` r
main_rmd(path = ".")
```

## Arguments

- path:

  Path to the root of the package directory.

## Value

A character vector, of length 1 if a main `.Rmd` is found, otherwise of
length 0.
