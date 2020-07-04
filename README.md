# pkgpurl: Facilitate Authoring R Packages in the R Markdown File Format

pkgpurl facilitates R package authoring using a literate programming approach. The main idea behind this is to write all of the R source code in R Markdown files (`Rmd/*.Rmd`), which allows the actual code to be freely mixed with explanatory and supplementary information in expressive Markdown format. The main object of pkgpurl is to provide a standardized way to compile the bare `.R` files from the prose-enhanced and thus more human-oriented `.Rmd` files.

The basic idea behind the concept this package implements originates from Yihui Xie. See his blog post [*Write An R Package Using Literate Programming Techniques*](https://yihui.org/rlp/) for more details, it’s definitively worth reading. This package’s function `pkgpurl::purl_rmd()` is just a less cumbersome alternative to the Makefile approach outlined by him.

## Installation

To install the latest development version of pkgpurl, run the following in R:

``` r
if (!("remotes" %in% rownames(installed.packages()))) {
  install.packages(pkgs = "remotes",
                   repos = "https://cloud.r-project.org/")
}

remotes::install_gitlab(repo = "salim_b/r/pkgs/pkgpurl")
```

## Development

### R Markdown format

This package’s source code is written in the [R Markdown](https://rmarkdown.rstudio.com/) file format to facilitate practices commonly referred to as [*literate programming*](https://en.wikipedia.org/wiki/Literate_programming). It allows the actual code to be freely mixed with explanatory and supplementary information in expressive Markdown format instead of having to rely on [`#` comments](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Comments) only.

All the `-GEN.R` suffixed R source code found under [`R/`](R/) is generated from the respective R Markdown counterparts under [`Rmd/`](Rmd/) using [`pkgpurl::purl_rmd()`](https://gitlab.com/salim_b/r/pkgs/pkgpurl/)[^1]. Always make changes only to the `.Rmd` files – never the `.R` files – and then run `pkgpurl::purl_rmd()` to regenerate the R source files.

### Coding style

This package borrows a lot of the [Tidyverse](https://www.tidyverse.org/) design philosophies. The R code adheres to the principles specified in the [Tidyverse Design Guide](https://principles.tidyverse.org/) wherever possible and is formatted according to the [Tidyverse Style Guide](https://style.tidyverse.org/) (TSG) with the following exceptions:

-   Line width is limited to **160 characters**, double the [limit proposed by the TSG](https://style.tidyverse.org/syntax.html#long-lines) (80 characters is ridiculously little given today’s high-resolution wide screen monitors).

-   Usage of [magrittr’s compound assignment pipe-operator `%<>%`](https://magrittr.tidyverse.org/reference/compound.html) is desirable[^2].

-   Usage of [R’s right-hand assignment operator `->`](https://rdrr.io/r/base/assignOps.html) is not allowed[^3].

As far as possible, these deviations from the TSG plus some additional restrictions are formally specified in the [lintr configuration file](https://github.com/jimhester/lintr#project-configuration) [`.lintr`](.lintr), so lintr can be used right away to check for formatting issues:

``` r
pkgpurl::lint_rmd()
```

[^1]: This naming convention as well as the very idea to leverage the R Markdown format to author R packages was originally proposed by Yihui Xie. See his excellent [blog post](https://yihui.name/rlp/) for more detailed information about the benefits of literate programming techniques and some practical examples. Note that using `pkgpurl::purl_rmd()` is a less cumbersome alternative to the Makefile approach outlined by him.

[^2]: The TSG [explicitly instructs to avoid this operator](https://style.tidyverse.org/pipes.html#assignment-1) – presumably because it’s relatively unknown and therefore might be confused with the forward pipe operator `%>%` when skimming code only briefly. I don’t consider this to be an actual issue since there aren’t many sensible usage patterns of `%>%` at the beginning of a pipe sequence inside a function – I can only think of creating side effects and relying on [R’s implicit return of the last evaluated expression](https://rdrr.io/r/base/function.html). Therefore – and because I really like the `%<>%` operator – it’s usage is welcome.

[^3]: The TSG [explicitly accepts `->` for assignments at the end of a pipe sequence](https://style.tidyverse.org/pipes.html#assignment-1) while Google’s R Style Guide [considers this bad practice](https://google.github.io/styleguide/Rguide.html#right-hand-assignment) because it “makes it harder to see in code where an object is defined”. I second the latter.
