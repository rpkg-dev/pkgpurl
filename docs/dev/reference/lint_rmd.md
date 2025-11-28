# Lint R Markdown package

This is a convenience wrapper around
[`lintr::lint_dir()`](https://lintr.r-lib.org/reference/lint.html) which
is tailored to a typical R Markdown package. To use this function, the
[lintr](https://github.com/jimhester/lintr/#readme) package is required.

## Usage

``` r
lint_rmd(
  path = ".",
  linters = default_linters,
  cache = FALSE,
  relative_path = TRUE,
  exclusions = default_exclusions(excl_vignettes = TRUE),
  pattern = "\\.[Rr]([Mm][Dd])?$",
  parse_settings = TRUE,
  show_progress = NULL
)
```

## Arguments

- path:

  Path to the root of the package directory.

- linters:

  A named list of linter functions to apply. See
  [linters](https://lintr.r-lib.org/reference/linters.html) for a full
  list of default and available linters.

- cache:

  When logical, toggle caching of lint results. If passed a character
  string, store the cache in this directory.

- relative_path:

  if `TRUE`, file paths are printed using their path relative to the
  base directory. If `FALSE`, use the full absolute path.

- exclusions:

  exclusions for
  [`exclude()`](https://lintr.r-lib.org/reference/exclude.html),
  relative to the package path.

- pattern:

  pattern for files, by default it will take files with any of the
  extensions .R, .Rmd, .qmd, .Rnw, .Rhtml, .Rrst, .Rtex, .Rtxt allowing
  for lowercase r (.r, ...).

- parse_settings:

  Logical. Whether to try and parse the
  [settings](https://lintr.r-lib.org/reference/read_settings.html).
  Otherwise, the
  [`default_settings()`](https://lintr.r-lib.org/reference/default_settings.html)
  are used. `TRUE` by default when linting files, as opposed to `text=`.

- show_progress:

  Logical controlling whether to show linting progress with
  [`cli::cli_progress_along()`](https://cli.r-lib.org/reference/cli_progress_along.html).
  The default behavior is to show progress in
  [`interactive()`](https://rdrr.io/r/base/interactive.html) sessions
  not running a testthat suite.

## Value

An object of class `c("lints", "list")`, each element of which is a
`"list"` object.

## Details

To avoid unnecessary noise, all the the generated `R/*.gen.R` files as
well as R Markdown vignettes under `vignettes/*.Rmd` are excluded from
linting.

This function is registered as an [RStudio
add-in](https://rstudio.github.io/rstudioaddins/), allowing RStudio
users to assign a [custom
shortcut](https://support.posit.co/hc/en-us/articles/206382178-Customizing-Keyboard-Shortcuts-in-the-RStudio-IDE)
to it and to invoke it from the [command
palette](https://posit.co/blog/rstudio-1-4-a-quick-tour/#command-palette-shortcuts).

## See also

[`default_linters`](https://pkgpurl.rpkg.dev/dev/reference/default_linters.md)
and
[`default_exclusions()`](https://pkgpurl.rpkg.dev/dev/reference/default_exclusions.md)

Other high-level functions:
[`load_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/load_pkg.md),
[`process_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/process_pkg.md),
[`purl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.md),
[`run_nopurl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/run_nopurl_rmd.md)
