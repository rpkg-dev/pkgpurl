# Load R Markdown package

Executes the steps to load an R package written in R Markdown format in
one go:

1.  Purl all relevant `Rmd/*.Rmd` files to `R/*.gen.R` files using
    [`purl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.md).

2.  Loads the package using
    [`devtools::load_all()`](https://devtools.r-lib.org/reference/load_all.html).

## Usage

``` r
load_pkg(
  path = ".",
  add_copyright_notice = FALSE,
  add_license_notice = FALSE,
  gen_pkgdown_ref = FALSE,
  reset = TRUE,
  recompile = FALSE,
  export_all = TRUE,
  helpers = TRUE,
  quiet = FALSE,
  ...
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

- reset:

  **\[deprecated\]** This is no longer supported because preserving the
  namespace requires unlocking its environment, which is no longer
  possible in recent versions of R.

- recompile:

  DEPRECATED. force a recompile of DLL from source code, if present.
  This is equivalent to running
  [`pkgbuild::clean_dll()`](https://pkgbuild.r-lib.org/reference/clean_dll.html)
  before `load_all()`

- export_all:

  If `TRUE` (the default), export all objects. If `FALSE`, export only
  the objects that are listed as exports in the NAMESPACE file.

- helpers:

  if `TRUE` loads testthat test helpers.

- quiet:

  if `TRUE` suppresses output from this function.

- ...:

  Additional arguments passed to
  [`pkgload::load_all()`](https://pkgload.r-lib.org/reference/load_all.html).

## Value

`path`, invisibly.

## See also

Other high-level functions:
[`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md),
[`process_pkg()`](https://pkgpurl.rpkg.dev/dev/reference/process_pkg.md),
[`purl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/purl_rmd.md),
[`run_nopurl_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/run_nopurl_rmd.md)
