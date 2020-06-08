# pkgpurl: Purl an Rmd package.

Facilitate authoring R packages in the [R Markdown file format](https://rmarkdown.rstudio.com/).

## Features

\...

## Details

\...

## Installation

To install the latest development version of **pkgpurl**, run the following in R:

``` {.r}
if (!("remotes" %in% rownames(installed.packages()))) {
  install.packages(pkgs = "remotes",
                   repos = "https://cloud.r-project.org/")
}
remotes::install_gitlab(repo = "salim_b/r/pkgs/pkgpurl")
```

## Development

### Literate Programming

This package is written using [literate programming](https://en.wikipedia.org/wiki/Literate_programming) techniques [proposed by Yihui Xie](https://yihui.name/rlp/). This means all the `-GEN.R` suffixed R source code found under [`R/`](R/) is generated from their respective [R Markdown](https://rmarkdown.rstudio.com/) counterparts found under [`Rmd/`](Rmd/) as instructed by the [`Makefile`](Makefile). Always make changes only to the `.Rmd` files -- not the `.R` files -- and then run the following from the root of this repository to regenerate the R source code and build and install the package:

``` {.sh}
make && Rscript -e "devtools::install('.', keep_source = TRUE)"
```

Make sure that [`make`](https://de.wikipedia.org/wiki/GNU_Make)[^1] is available on your system and that the R packages [`knitr`](https://cran.r-project.org/package=knitr) and [`rmarkdown`](https://cran.r-project.org/package=rmarkdown) are up to date beforehand.

### Coding Style

This package borrows a lot of the [Tidyverse](https://www.tidyverse.org/) design philosophies. The R code adheres to the principles specified in the [Tidyverse Design Guide](https://principles.tidyverse.org/) wherever possible and is formatted according to the [Tidyverse Style Guide](https://style.tidyverse.org/) (TSG) with the following exceptions:

-   Line width is limited to **160 characters**, double the [limit proposed by the TSG](https://style.tidyverse.org/syntax.html#long-lines) (80 characters is ridiculously little given today\'s high-resolution wide screen monitors).

-   Usage of [magrittr\'s compound assignment pipe-operator `%<>%`](https://magrittr.tidyverse.org/reference/compound.html) is desirable[^2].

-   Usage of [R\'s right-hand assignment operator `->`](https://rdrr.io/r/base/assignOps.html) is not allowed[^3].

As far as possible, these deviations from the TSG plus some additional restrictions are formally specified in the [lintr configuration file](https://github.com/jimhester/lintr#project-configuration) [`.lintr`](.lintr), so lintr can be used right away to check for formatting issues:

``` {.r}
lintr::lint_dir(pattern = "\\.Rmd$",
                exclusions = list.files(path = "vignettes",
                                        recursive = TRUE,
                                        full.names = TRUE))
```

## See also

-   [Write An R Package Using Literate Programming Techniques](https://yihui.org/rlp/) by Yihui Xie

[^1]: On Windows, `make` is included in [Rtools](https://cran.rstudio.com/bin/windows/Rtools/). On macOS, you have to install the [Xcode command line tools](https://stackoverflow.com/a/10301513/7196903).

[^2]: The TSG [explicitly instructs to avoid this operator](https://style.tidyverse.org/pipes.html#assignment-1) -- presumably because it\'s relatively unknown and therefore might be confused with the forward pipe operator `%>%` when skimming code only briefly. I don\'t consider this to be an actual issue since there aren\'t many sensible usage patterns of `%>%` at the beginning of a pipe sequence inside a function -- I can only think of creating side effects and relying on [R\'s implicit return of the last evaluated expression](https://rdrr.io/r/base/function.html). Therefore -- and because I really like the `%<>%` operator -- it\'s usage is welcome.

[^3]: The TSG [explicitly accepts `->` for assignments at the end of a pipe sequence](https://style.tidyverse.org/pipes.html#assignment-1) while Google\'s R Style Guide [considers this bad practice](https://google.github.io/styleguide/Rguide.html#right-hand-assignment) because it \"makes it harder to see in code where an object is defined\". I second the latter.
