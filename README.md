# pkgpurl: Facilitate Authoring R Packages in the R Markdown File Format

pkgpurl facilitates R package authoring using a literate programming approach. The main idea behind this is to write all of the R source code in R Markdown files (`Rmd/*.Rmd`), which allows the actual code to be freely mixed with explanatory and supplementary information in expressive Markdown format. The main object of pkgpurl is to provide a standardized way to compile the bare `.R` files from the prose-enhanced and thus more human-oriented `.Rmd` files.

The basic idea behind the concept this package implements originates from Yihui Xie. See his blog post [*Write An R Package Using Literate Programming Techniques*](https://yihui.org/rlp/) for more details, it’s definitively worth reading. This package’s function `pkgpurl::purl_rmd()` is just a less cumbersome alternative to the Makefile approach outlined by him.

## Details

Besides the ability to write accompanying information in expressive [Markdown](https://en.wikipedia.org/wiki/Markdown), the [R Markdown](https://rmarkdown.rstudio.com/) format provides several further **advantages** over the bare R source format:

-   If you use RStudio or any other editor with proper R Markdown syntax highlighting, you will probably like the gained visual clarity for distinguishing individual functions/code parts (i.e. R code chunks). This also facilitates creating a meaningful document structure (in Markdown) alongside the actual R source code.

-   You can use RStudio’s [code outline](https://rviews.rstudio.com/2016/11/11/easy-tricks-you-mightve-missed/#code-outline) to easily navigate through longer scripts. It provides significantly better usability than the [code section standard](https://support.rstudio.com/hc/en-us/articles/200484568-Code-Folding-and-Sections) of classic R scripts. It makes it easy to find your way around source files that are thousands of lines long.

-   You can put development-only code which never lands in the generated R source files (and thus the R package) in separate code chunks with the chunk option [`purl = FALSE`](https://yihui.org/knitr/options/#extracting-source-code). This turns out to be very convenient in certain situations.

    For example, this is a good way to reproducibly document the generation of cleaned versions of [exported data](https://r-pkgs.org/data.html#data-data) as well as [internal data](https://r-pkgs.org/data.html#data-sysdata). This avoids having to outsource the code to separate files under `data-raw/` and adding the directory to `.Rbuildignore`, i.e. no need to use `usethis::use_data_raw()`. Instead, you just set `purl = FALSE` for the relevant code chunk(s). You can (and should) still use `usethis::use_data(...)` (optionally with `overwrite = TRUE`) to generate the files under `data/` holding external package data as well as the `R/sysdata.rda` file (using `internal = TRUE`) holding internal package data.

-   As you might already know, [you are limited to the ASCII character set for your code if you plan on submitting your package to CRAN](https://r-pkgs.org/r.html#r-cran). This of course also applies to R code chunks in the `Rmd/*.Rmd` files that are compiled to `R/*-GEN.R` files by pkgpurl. But since the accompanying Markdown documentation won’t land in the compiled R files, you can use the full Unicode spectrum of characters there (:partying_face:) – as long as you exclude the R Markdown source from the built package by putting a line `^Rmd$` in `.Rbuildignore` (which is recommended anyway).

But there are also a few **drawbacks** of the R Markdown format:

-   The pkgpurl approach on writing R packages in the R Markdown format introduces *one* additional step at the very beginning of typical package development workflows: Running `pkgpurl::purl_rmd()` to generate the `R/*-GEN.R` files from the original `Rmd/*.Rmd` sources before documenting/checking/testing/building the package. Given sufficient user demand, this could probably be integrated into [devtools](https://devtools.r-lib.org/)’ functions in the future, so that no additional action has to be taken by the user when relying on RStudio’s built-in package building infrastructure.

    For the time being, it’s recommended to set up a custom shortcut[^1] for `pkgpurl::purl_rmd()` which is registered as an [RStudio add-in](https://rstudio.github.io/rstudioaddins/).

-   Setting up a new project to write an R package in the R Markdown differs slightly from the classic approach. A suitable convenience function like `create_rmd_package()` to set up all the necessary parts could probably be added to [usethis](https://usethis.r-lib.org/) in the future.

    For the time being, you can use my ready-to-go [R Markdown Package Development Template](https://gitlab.com/salim_b/r/pkg-dev-tpl) as a starting point for creating new R packages in the R Markdown format.

-   Other than in `.R` files, RStudio currently doesn’t support auto-completion of [roxygen2 tags](https://roxygen2.r-lib.org/articles/rd.html) in `.Rmd` files. The same applies to the roxygen2 comment continuation when inserting a newline. These are [known issues](https://github.com/rstudio/rstudio/issues/5809) which will hopefully be resolved in the near future.

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

All the `-GEN.R` suffixed R source code found under [`R/`](R/) is generated from the respective R Markdown counterparts under [`Rmd/`](Rmd/) using [`pkgpurl::purl_rmd()`](https://gitlab.com/salim_b/r/pkgs/pkgpurl/)[^2]. Always make changes only to the `.Rmd` files – never the `.R` files – and then run `pkgpurl::purl_rmd()` to regenerate the R source files.

### Coding style

This package borrows a lot of the [Tidyverse](https://www.tidyverse.org/) design philosophies. The R code adheres to the principles specified in the [Tidyverse Design Guide](https://principles.tidyverse.org/) wherever possible and is formatted according to the [Tidyverse Style Guide](https://style.tidyverse.org/) (TSG) with the following exceptions:

-   Line width is limited to **160 characters**, double the [limit proposed by the TSG](https://style.tidyverse.org/syntax.html#long-lines) (80 characters is ridiculously little given today’s high-resolution wide screen monitors).

-   Usage of [magrittr’s compound assignment pipe-operator `%<>%`](https://magrittr.tidyverse.org/reference/compound.html) is desirable[^3].

-   Usage of [R’s right-hand assignment operator `->`](https://rdrr.io/r/base/assignOps.html) is not allowed[^4].

-   R source code is *not* split over several files as [suggested by the TSG](https://style.tidyverse.org/package-files.html) but instead is (as far as possible) kept in the single file [`Rmd/pkgpurl.Rmd`](Rmd/pkgpurl.Rmd) which is well-structured thanks to its [Markdown support](#r-markdown-format).

As far as possible, these deviations from the TSG plus some additional restrictions are formally specified in the [lintr configuration file](https://github.com/jimhester/lintr#project-configuration) [`.lintr`](.lintr), so lintr can be used right away to check for formatting issues:

``` r
pkgpurl::lint_rmd()
```

---

[^1]: I personally recommend to use <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>V</kbd> since it’s not occupied yet by any of the predefined [RStudio shortcuts](https://support.rstudio.com/hc/en-us/articles/200711853-Keyboard-Shortcuts).

[^2]: This naming convention as well as the very idea to leverage the R Markdown format to author R packages was originally proposed by Yihui Xie. See his excellent [blog post](https://yihui.name/rlp/) for more detailed information about the benefits of literate programming techniques and some practical examples. Note that using `pkgpurl::purl_rmd()` is a less cumbersome alternative to the Makefile approach outlined by him.

[^3]: The TSG [explicitly instructs to avoid this operator](https://style.tidyverse.org/pipes.html#assignment-2) – presumably because it’s relatively unknown and therefore might be confused with the forward pipe operator `%>%` when skimming code only briefly. I don’t consider this to be an actual issue since there aren’t many sensible usage patterns of `%>%` at the beginning of a pipe sequence inside a function – I can only think of creating side effects and relying on [R’s implicit return of the last evaluated expression](https://rdrr.io/r/base/function.html). Therefore – and because I really like the `%<>%` operator – it’s usage is welcome.

[^4]: The TSG [explicitly accepts `->` for assignments at the end of a pipe sequence](https://style.tidyverse.org/pipes.html#assignment-2) while Google’s R Style Guide [considers this bad practice](https://google.github.io/styleguide/Rguide.html#right-hand-assignment) because it “makes it harder to see in code where an object is defined”. I second the latter.
