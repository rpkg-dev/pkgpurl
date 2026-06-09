# Process R Markdown package from source to installation

Executes all steps to process an R package written in R Markdown format
from source to installation in one go:

1.  Purl all relevant `Rmd/*.Rmd` files to `R/*.gen.R` files using
    [`purl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.md).

2.  Re-generate the [pkgdown reference
    index](https://pkgdown.r-lib.org/reference/build_reference.html#reference-index)
    based on the package's [main R Markdown
    file](https://pkgpurl.rpkg.dev/dev/reference/main_rmd.md) using
    [`gen_pkgdown_ref()`](https://pkgpurl.rpkg.dev/dev/reference/gen_pkgdown_ref.md)
    (if `gen_pkgdown_ref = TRUE`).

3.  Re-build the package documentation using
    [`devtools::document()`](https://devtools.r-lib.org/reference/document.html)
    (if `document = TRUE`).

4.  Build and install the package (if `build_and_install = TRUE`). This
    is done either using
    [`rstudioapi::executeCommand(commandId = "buildFull")`](https://rstudio.github.io/rstudioapi/reference/executeCommand.html)
    (if `use_rstudio_api = TRUE`) or using
    [`devtools::install()`](https://devtools.r-lib.org/reference/install.html)
    (if `use_rstudio_api = FALSE`).

5.  Restarts the R session using
    [`rstudioapi::restartSession()`](https://rstudio.github.io/rstudioapi/reference/restartSession.html)
    (if either `restart_r_session = TRUE` or `use_rstudio_api = TRUE`).

## Usage

``` r
process_pkg(
  path = ".",
  add_copyright_notice = funky::config_val("add_copyright_notice"),
  add_license_notice = funky::config_val("add_license_notice"),
  gen_pkgdown_ref = funky::config_val("gen_pkgdown_ref"),
  env = parent.frame(),
  document = TRUE,
  build_and_install = TRUE,
  restart_r_session = build_and_install,
  use_rstudio_api = NULL,
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

- document:

  Whether or not to re-build the package documentation after purling
  `Rmd/*.Rmd` to `R/*.gen.R`.

- build_and_install:

  Whether or not to build and install the package after purling
  `Rmd/*.Rmd` to `R/*.gen.R`.

- restart_r_session:

  Whether or not to restart the R session. Highly recommended if
  `build_and_install = TRUE`, but only possible when R is run within
  RStudio. Note that the R session is always restarted if
  `use_rstudio_api = TRUE`.

- use_rstudio_api:

  Whether or not to rely on the RStudio API to install the built package
  (which always triggers an R session restart regardless of
  `restart_r_session`). If `NULL`, the RStudio API is automatically used
  if possible, i.e. RStudio is running. Note that installation without
  the RStudio API has known issues, see section *Details* below for
  further information.

- quiet:

  Whether or not to suppress printing status output from internal
  processing.

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

`path`, invisibly.

## Details

Note that the installation via
[`devtools::install()`](https://devtools.r-lib.org/reference/install.html)
(i.e. `use_rstudio_api = FALSE`) is known to fail in certain situations
(lazy-load database corruption) due to [unresolved
deficiencies](https://bugs.r-project.org/show_bug.cgi?id=16644) in R's
namespace unloading. If you encounter an error, simply restart the R
session and try again.

This function is registered as an [RStudio
add-in](https://rstudio.github.io/rstudioaddins/), allowing RStudio
users to assign a [custom
shortcut](https://support.posit.co/hc/en-us/articles/206382178-Customizing-Keyboard-Shortcuts-in-the-RStudio-IDE)
to it and to invoke it from the [command
palette](https://posit.co/blog/rstudio-1-4-a-quick-tour/#command-palette-shortcuts).

## See also

Other high-level functions:
[`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md),
[`load_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/load_pkg.md),
[`purl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.md),
[`run_nopurl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/run_nopurl_rmd.md)
