# Purl `Rmd/*.Rmd` to `R/*.gen.R`

This function strives to provide a standardized way to convert all
relevant `.Rmd` files in the `Rmd/` subdirectory to bare `.R` files in
the `R/` subdirectory using
[`knitr::purl()`](https://rdrr.io/pkg/knitr/man/knit.html). It is mainly
intended for authoring R packages in the [R Markdown file
format](https://rmarkdown.rstudio.com/).

## Usage

``` r
purl_rmd(
  path = ".",
  add_copyright_notice = funky::config_val("add_copyright_notice"),
  add_license_notice = funky::config_val("add_license_notice"),
  gen_pkgdown_ref = funky::config_val("gen_pkgdown_ref"),
  env = parent.frame()
)
```

## Arguments

- path:

  Path to the root of the package directory.

- add_copyright_notice:

  Whether or not to add a **copyright notice** at the beginning of the
  generated `.R` files as recommended by e.g. the [GNU
  licenses](https://www.gnu.org/licenses/gpl-howto.html). The notice
  consists of the name and description of the program and the word
  `Copyright (C)` followed by the release years and the name(s) of the
  copyright holder(s), or if not specified, the author(s). The year is
  always the current year. All the other information is extracted from
  the package's `DESCRIPTION` file. A logical scalar. Only applies if
  `path` [is actually an R package
  directory](https://pal.rpkg.dev/reference/is_pkg_dir.html).

- add_license_notice:

  Whether or not to add a **license notice** at the beginning of the
  generated `.R` files as recommended by e.g. the [GNU
  licenses](https://www.gnu.org/licenses/gpl-howto.html). The license is
  determined from the package's `DESCRIPTION` file and currently only
  the [`AGPL-3.0-or-later`
  license](https://spdx.org/licenses/AGPL-3.0-or-later.html) is
  supported. A logical scalar. Only applies if `path` [is actually an R
  package directory](https://pal.rpkg.dev/reference/is_pkg_dir.html).

- gen_pkgdown_ref:

  Whether or not to overwrite [pkgdown](https://pkgdown.r-lib.org/)'s
  [reference
  index](https://pkgdown.r-lib.org/reference/build_reference.html#reference-index)
  in the configuration file `_pkgdown.yml` with an auto-generated one
  based on the main input file as described in
  [`gen_pkgdown_ref()`](https://pkgpurl.rpkg.dev/dev/reference/gen_pkgdown_ref.md).
  A logical scalar. Only applies if `path` [is actually an R package
  directory](https://pal.rpkg.dev/reference/is_pkg_dir.html), [pkgdown
  is set up](https://pal.rpkg.dev/reference/is_pkgdown_dir.html) and a
  [main R Markdown
  file](https://pkgpurl.rpkg.dev/dev/reference/main_rmd.md) exists.

- env:

  Environment to evaluate R Markdown inline code expressions in when
  generating the pkgdown reference index. Only relevant if
  `gen_pkgdown_ref = TRUE`.

## Value

`path`, invisibly.

## Details

The generated `.R` files will be named the same as the `.Rmd` files plus
the suffix `.gen` to indicate the file was auto-generated. So the file
`Rmd/foo.Rmd` for example will be converted to `R/foo.gen.R`.

The R Markdown file format allows you to intermingle code with related
prose in [Markdown
syntax](https://bookdown.org/yihui/rmarkdown/markdown-syntax.html)
optimized for human readability. This facilitates (best) practices which
are commonly referred to as [*literate
programming*](https://en.wikipedia.org/wiki/Literate_programming).

In practice, the main advantage of writing R code in R Markdown is that
you don't have to rely on [`#`
comments](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Comments)
to explain, annotate or otherwise elaborate on your code. It also allows
you to easily compile your source code to beautifully looking HTML, PDF
etc. files using
[`rmarkdown::render()`](https://pkgs.rstudio.com/rmarkdown/reference/render.html).

This function is registered as an [RStudio
add-in](https://rstudio.github.io/rstudioaddins/), allowing RStudio
users to assign a [custom
shortcut](https://support.posit.co/hc/en-us/articles/206382178-Customizing-Keyboard-Shortcuts-in-the-RStudio-IDE)
to it and to invoke it from the [command
palette](https://posit.co/blog/rstudio-1-4-a-quick-tour/#command-palette-shortcuts).

## Files excluded from purling

`purl_rmd()` does not generate an `.R` file for each and every R
Markdown file in the `Rmd/` subdirectory. Two types of `.Rmd` files are
excluded from purling:

1.  Files having the suffix `.nopurl` in their name, e.g.
    `Rmd/playground.nopurl.Rmd`.

2.  Hidden files [as per Unix
    convention](https://en.wikipedia.org/wiki/Hidden_file_and_hidden_directory#Unix_and_Unix-like_environments)
    whose names start with a dot, e.g. `Rmd/.playground.Rmd`.

The above convention allows for easy exclusion of specific `.Rmd` files
from purling. A common case for this are scripts that generate
[package-internal data](https://r-pkgs.org/data.html#sec-data-sysdata)
from raw sources. Such a script could be stored as
`Rmd/data.nopurl.Rmd`, so that no corresponding file under `R/*.R` is
generated. For the sake of clarity, it's generally advised to prefer the
`.nopurl` suffix over hiding files.

## See also

Other high-level functions:
[`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md),
[`load_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/load_pkg.md),
[`process_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/process_pkg.md),
[`run_nopurl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/run_nopurl_rmd.md)
