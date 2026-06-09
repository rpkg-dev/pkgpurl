# Run `*.nopurl.Rmd` files

**\[experimental\]**

Executes `.Rmd` files which are supposed to contain code not included in
the [source
package](https://r-pkgs.org/structure.html#sec-source-package), i.e.
usually outsourced to separate `.Rmd` files with the [`.nopurl` suffix
in their
filenames](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.html#-rmd-files-excluded-from-purling).
Those files are typically used to generate package data.

If an error is encountered saying `internal error -3 in R_decompress1`,
restart the R session and run again.

## Usage

``` r
run_nopurl_rmd(
  path = ".",
  path_rmd = fs::dir_ls(path = fs::path(path, "Rmd"), recurse = TRUE, type = "file", glob
    = "*.nopurl.Rmd"),
  env = NULL,
  document = TRUE,
  build_and_install = TRUE,
  restart_r_session = TRUE,
  quiet = TRUE,
  roclets = NULL,
  args = getOption("devtools.install.args"),
  dependencies = NA,
  upgrade = "never",
  keep_source = getOption("keep.source.pkgs")
)
```

## Arguments

- path:

  Path to the root of the package directory.

- path_rmd:

  Path(s) to the `.Rmd` files to be executed. A character vector.

- env:

  Environment to evaluate the `.Rmd` files in. If `NULL`, the [global
  environment](https://rdrr.io/r/base/environment.html) is used.

- document:

  Whether or not to re-build the package documentation after the last
  `.Rmd` file is executed.

- build_and_install:

  Whether or not to build and install the package after each `.Rmd` file
  execution.

- restart_r_session:

  Whether or not to restart the R session after the last `.Rmd` file is
  executed. Highly recommended if `build_and_install = TRUE`, but only
  possible when R is run within RStudio.

- quiet:

  if `TRUE` suppresses output from this function.

- roclets:

  Character vector of roclet names to use with package. The default,
  `NULL`, uses the roxygen `roclets` option, which defaults to
  `c("collate", "namespace", "rd")`.

- args:

  An optional character vector of additional command line arguments to
  be passed to `R CMD INSTALL`. This defaults to the value of the option
  `"devtools.install.args"`.

- dependencies:

  What kinds of dependencies to install. Most commonly one of the
  following values:

  - `NA`: only required (hard) dependencies,

  - `TRUE`: required dependencies plus optional and development
    dependencies,

  - `FALSE`: do not install any dependencies. (You might end up with a
    non-working package, and/or the installation might fail.) See
    [Package dependency
    types](https://pak.r-lib.org/reference/package-dependency-types.html)
    for other possible values and more information about package
    dependencies.

- upgrade:

  When `FALSE`, the default, pak does the minimum amount of work to give
  you the latest version(s) of `pkg`. It will only upgrade dependent
  packages if `pkg`, or one of their dependencies explicitly require a
  higher version than what you currently have. It will also prefer a
  binary package over to source package, even if the binary package is
  older.

  When `upgrade = TRUE`, pak will ensure that you have the latest
  version(s) of `pkg` and all their dependencies.

- keep_source:

  If `TRUE` will keep the srcrefs from an installed package. This is
  useful for debugging (especially inside of RStudio or Positron).
  Defaults to `getOption("keep.source.pkgs") || !build`, since srcrefs
  are most useful when the package is installed from its source
  directory, i.e. when `build = FALSE`.

## Value

`path_rmd`, invisibly.

## See also

Other high-level functions:
[`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md),
[`load_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/load_pkg.md),
[`process_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/process_pkg.md),
[`purl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.md)
