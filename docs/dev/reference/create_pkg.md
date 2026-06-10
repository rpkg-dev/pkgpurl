# Create new R Markdown package

Populates the directory specified via `path` with all the necessary
files for a new R Markdown package.

The `DESCRIPTION` file is created using
[`usethis::use_description()`](https://usethis.r-lib.org/reference/use_description.html)
and all fields except `Package`, `URL` and `BugReports` are sourced from
the [`usethis.description` R
option](https://usethis.r-lib.org/reference/use_description.html) if
defined.

## Usage

``` r
create_pkg(
  name,
  id_netlify = NULL,
  path = ".",
  incl_roxygen2_meta = TRUE,
  incl_reexports = FALSE,
  incl_sysdata_rmd = FALSE,
  incl_data_rmd = FALSE,
  incl_asciicasts_rmd = FALSE,
  incl_pkgdown_config = TRUE,
  incl_ripgrep_config = TRUE,
  incl_ack_config = FALSE,
  incl_air_config = FALSE
)
```

## Arguments

- name:

  Package name.

- id_netlify:

  Netlify site identifier.

- path:

  Path to the new package directory.

- incl_roxygen2_meta:

  Whether or not to create a `man/roxygen/meta.R` file that i.a. stores
  the metadata for [roxygen2's `@family`
  tags](https://roxygen2.r-lib.org/articles/index-crossref.html#family).

- incl_reexports:

  Whether or not to create an `R/reexports.R` file prefilled with an
  opinionated set of
  [magrittr](https://magrittr.tidyverse.org/reference/index.html#pipes)
  and [rlang](https://rlang.r-lib.org/reference/index.html#operators)
  operator exports.

- incl_sysdata_rmd:

  Whether or not to create an `Rmd/sysdata.nopurl.Rmd` stub file.

- incl_data_rmd:

  Whether or not to create an `Rmd/data.nopurl.Rmd` stub file.

- incl_asciicasts_rmd:

  Whether or not to create an `Rmd/asciicasts.nopurl.Rmd` stub file.

- incl_pkgdown_config:

  Whether or not to create a minimal `pkgdown/_pkgdown.yml`
  configuration file.

- incl_ripgrep_config:

  Whether or not to create a custom `.rgignore`
  [ripgrep](https://github.com/burntsushi/ripgrep) ignore file.

- incl_ack_config:

  Whether or not to create an [`.ackrc` configuration
  file](https://beyondgrep.com/documentation/).

- incl_air_config:

  Whether or not to create an [`air.toml` configuration
  file](https://posit-dev.github.io/air/configuration.html). Note that
  air is not of much use yet for R Markdown packages.

## Value

`path`, invisibly.
