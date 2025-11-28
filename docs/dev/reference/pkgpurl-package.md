# pkgpurl: Facilitate Authoring R Packages in the R Markdown File Format

pkgpurl facilitates R package authoring using a literate programming
workflow. It enables you to write all R source code in R Markdown files,
letting executable code coexist naturally with narrative explanations
and supplementary context in expressive Markdown format. The primary
purpose of pkgpurl is to provide a standardized way to compile the bare
\`.R\` files from the richer, prose-enhanced \`.Rmd\` files.

## Package configuration

Some of pkgpurl's functionality is controlled via package-specific
global configuration which can either be set via [R
options](https://rdrr.io/r/base/options.html) or [environment
variables](https://en.wikipedia.org/wiki/Environment_variable) (the
former take precedence). This configuration includes:

|  |  |  |  |
|----|----|----|----|
| **Description** | **R option** | **Environment variable** | **Default value** |
| Whether or not to add a **copyright notice** at the beginning of the generated `.R` files as recommended by e.g. the [GNU licenses](https://www.gnu.org/licenses/gpl-howto.html). The notice consists of the name and description of the program and the word `Copyright (C)` followed by the release years and the name(s) of the copyright holder(s), or if not specified, the author(s). The year is always the current year. All the other information is extracted from the package's `DESCRIPTION` file. | `pkgpurl.add_copyright_notice` | `R_PKGPURL_ADD_COPYRIGHT_NOTICE` | `TRUE` |
| Whether or not to add a **license notice** at the beginning of the generated `.R` files as recommended by e.g. the [GNU licenses](https://www.gnu.org/licenses/gpl-howto.html). The license is determined from the package's `DESCRIPTION` file and currently only the [`AGPL-3.0-or-later` license](https://spdx.org/licenses/AGPL-3.0-or-later.html) is supported. | `pkgpurl.add_license_notice` | `R_PKGPURL_ADD_LICENSE_NOTICE` | `TRUE` |
| Whether or not to overwrite [pkgdown](https://pkgdown.r-lib.org/)'s [reference index](https://pkgdown.r-lib.org/reference/build_reference.html#reference-index) in the configuration file `_pkgdown.yml` with an auto-generated one based on the main input file as described in [`gen_pkgdown_ref()`](https://pkgpurl.rpkg.dev/dev/reference/gen_pkgdown_ref.md). | `pkgpurl.gen_pkgdown_ref` | `R_PKGPURL_GEN_PKGDOWN_REF` | `TRUE` |

## See also

Useful links:

- <https://pkgpurl.rpkg.dev>

- <https://gitlab.com/rpkg.dev/pkgpurl>

- Report bugs at <https://gitlab.com/rpkg.dev/pkgpurl/-/issues>

## Author

**Maintainer**: Salim Brüggemann <salim@rpkg.dev>
([ORCID](https://orcid.org/0000-0002-5329-5987))
