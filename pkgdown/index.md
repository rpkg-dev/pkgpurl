# pkgpurl: Facilitate Authoring R Packages in the R Markdown File Format

[![CRAN Status](https://r-pkg.org/badges/version/pkgpurl)](https://cran.r-project.org/package=pkgpurl){.pkgdown-release}

pkgpurl facilitates R package authoring using a literate programming approach. The main idea behind this is to write all of the R source code in R Markdown files (`Rmd/*.Rmd`), which allows the actual code to be freely mixed with explanatory and supplementary information in expressive Markdown format. The main object of pkgpurl is to provide a standardized way to compile the bare `.R` files from the prose-enhanced and thus more human-oriented `.Rmd` files.

The basic idea behind the concept this package implements originates from Yihui Xie. See his blog post [*Write An R Package Using Literate Programming Techniques*](https://yihui.org/rlp/) for more details, it's definitively worth reading. This package's function [`pkgpurl::purl_rmd()`](https://rpkg.dev/pkgpurl/reference/purl_rmd.html) is just a less cumbersome alternative to the Makefile approach outlined by him.

## Details

The [R Markdown](https://rmarkdown.rstudio.com/) format provides several **advantages** over the bare R source format when developing an R package:

-   **Mix Markdown and Code**

    It allows the actual code to be freely mixed with explanatory and supplementary information in expressive [Markdown](https://en.wikipedia.org/wiki/Markdown) format instead of having to rely on [`#` comments](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Comments) only. In general, this should encourage to actually record code-accompanying information because you're able to use the full spectrum of [Pandoc's Markdown syntax](https://pandoc.org/MANUAL.html#pandocs-markdown) like inline formatting, lists, tables, quotations or math[^1].

    It is especially powerful in combination with the [Visual R Markdown](https://rstudio.github.io/visual-markdown-editing/) feature introduced in RStudio 1.4, which -- in addition to the visual editor -- offers a feature whose utility can hardly be overestimated: Pandoc Markdown [canonicalization](https://en.wikipedia.org/wiki/Canonicalization) (on file save[^2]). For example, it allows paragraphs being wrapped automatically at the desired line width; or to write a minimal sloppy [pipe table](https://pandoc.org/MANUAL.html#extension-pipe_tables) that is automatically normalized to a beautifully formatted and actually readable one.

    The relevant editor options which adjust the canonical Markdown generation can either be set

    -   [per `.Rmd` file](https://rstudio.github.io/visual-markdown-editing/#/markdown?id=writer-options), e.g.

        ``` rmd
        ---
        editor_options:
          markdown:
            wrap: 160
            references:
              location: section
            canonical: true
        ---
        ```

    -   or [per project](https://rstudio.github.io/visual-markdown-editing/#/options?id=project-options) in the usual `PACKAGE_NAME.Rproj` file, e.g.

        ``` ini
        MarkdownWrap: Column
        MarkdownWrapAtColumn: 160
        MarkdownReferences: Section
        MarkdownCanonical: Yes
        ```

        (I'd recommend to set them *per project*, so they apply to the whole package including any `.Rmd` vignettes.)

-   **All your code in a single, well-structured file**

    The [traditional recommendation](https://style.tidyverse.org/package-files.html) to not lose overview of your package's R source code is to split it over multiple files. The popular (and very useful) book *R Packages* [gives the following advice](https://r-pkgs.org/r.html#code-organising):

    > If it's very hard to predict which file a function lives in, that suggests it's time to separate your functions into more files or reconsider how you are naming your functions and/or files.

    I think this is just ridiculous.

    Instead, I encourage you to keep all your code (as far as possible) in a single file `Rmd/PACKAGE_NAME.Rmd` and structure it according to the [rules described here](https://rpkg.dev/pkgpurl/reference/gen_pkgdown_ref.html#details), which even allows the [pkgdown `Reference:` index](https://pkgdown.r-lib.org/reference/build_reference.html#reference-index) to be automatically in sync with the source code structure. As a result, you re-organize (and thus most likely improve) your package's code structure whenever you intend to improve the pkgdown reference -- and vice versa.

    Keeping all code in a single file frees you from the traditional hassle of finding a viable (but in the end still unsatisfactory) way to organize your R source code across multiple files. Of course, there are still good reasons to outsource code into separate files *in certain situations*, which nothing is stopping you from doing. You can also [exclude whole `.Rmd` files from purling using the `.nopurl.Rmd` filename suffix](https://rpkg.dev/pkgpurl/reference/purl_rmd.html#-rmd-files-excluded-from-purling).

-   **Improved overview and navigation**

    You can rely on RStudio's [code outline](https://rviews.rstudio.com/2016/11/11/easy-tricks-you-mightve-missed/#code-outline) to easily navigate through longer `.Rmd` files. IMHO it provides significantly better usability than the [code section standard](https://support.rstudio.com/hc/en-us/articles/200484568-Code-Folding-and-Sections) of `.R` files. It makes it easy to find your way around source files that are thousands of lines long.

    RStudio's [*Go to File/Function* shortcut](https://support.rstudio.com/hc/en-us/articles/200711853-Keyboard-Shortcuts) works the same for `.Rmd` files as it does for `.R` files.

-   **Improved visual clarity**

    If you use RStudio or any other editor with proper R Markdown syntax highlighting, you will probably like the gained visual clarity for distinguishing individual functions/code parts (by putting them in separate R code chunks). This also facilitates creating a meaningful document structure (in Markdown) alongside the actual R source code.

-   **Easily toggle code inclusion**

    You can put development-only code which never lands in the generated R source files (and thus the R package) in separate code chunks with the chunk option [`purl = FALSE`](https://yihui.org/knitr/options/#extracting-source-code). This turns out to be very convenient in certain situations.

    For example, this is a good way to reproducibly document the generation of cleaned versions of [exported data](https://r-pkgs.org/data.html#data-data) as well as [internal data](https://r-pkgs.org/data.html#data-sysdata). This avoids having to outsource the code to separate files under `data-raw/` and adding the directory to `.Rbuildignore`, i.e. no need to use `usethis::use_data_raw()`. Instead, you just set `purl = FALSE` for the relevant code chunk(s). You can (and should) still use [`usethis::use_data()`](https://usethis.r-lib.org/reference/use_data.html) (optionally with `overwrite = TRUE`) to generate the files under `data/` holding external package data as well as the `R/sysdata.rda` file (using `internal = TRUE`) holding internal package data.

-   **Easily toggle styler**

    If you use [styler](https://styler.r-lib.org/) to auto-format your code globally by [setting `knitr::opts_chunk$set(tidy = "styler")`](https://styler.r-lib.org/articles/third-party-integrations.html), you can still opt-out on a per-chunk basis by setting [`tidy = FALSE`](https://github.com/r-lib/styler/releases/tag/v1.5.1). This gives pleasant flexibility.

Unfortunately, there are also a few notable **drawbacks** of the R Markdown format:

-   **Additional workflow step**

    The pkgpurl approach on writing R packages in the R Markdown format introduces *one* additional step at the very beginning of typical package development workflows: Running [`pkgpurl::purl_rmd()`](https://rpkg.dev/pkgpurl/reference/purl_rmd.html) to generate the `R/*.gen.R` files from the original `Rmd/*.Rmd` sources before documenting/checking/testing/building the package. Given sufficient user demand, this could probably be integrated into [devtools](https://devtools.r-lib.org/)' functions in the future, so that no additional action has to be taken by the user when relying on RStudio's built-in package building infrastructure.

    For the time being, it's recommended to set up a custom shortcut[^3] for one or both of [`pkgpurl::purl_rmd()`](https://rpkg.dev/pkgpurl/reference/purl_rmd.html) and [`pkgpurl::process_pkg()`](https://rpkg.dev/pkgpurl/reference/process_pkg.html) which are registered as [RStudio add-ins](https://rstudio.github.io/rstudioaddins/).

-   **Differing setup**

    Setting up a new project to write an R package in the R Markdown differs slightly from the classic approach. A suitable convenience function like `create_rmd_package()` to set up all the necessary parts could probably be added to [usethis](https://usethis.r-lib.org/) in the future.

    For the time being, you can use my ready-to-go [R Markdown Package Development Template](https://gitlab.com/salim_b/r/pkg-dev-tpl) as a starting point for creating new R packages in the R Markdown format.

-   **Unwieldy debugging**

    Debugging can be a bit more laborious since line numbers in warning and error messages always refer to the generated `R/*.gen.R` file(s), not the underlying `Rmd/*.Rmd` source code file(s). If need be, you first have to look up the line numbers in the `R/*.gen.R` file(s) to understand which function / code parts cause the issue in order to know where to fix it in the `Rmd/*.Rmd` source(s).

-   **Missing roxygen2 auto-completion**

    Other than in `.R` files, RStudio currently doesn't support auto-completion of [roxygen2 tags](https://roxygen2.r-lib.org/articles/rd.html) in `.Rmd` files and its <kbd>Reflow Comment</kbd> command doesn't properly work on them. These are [known issues](https://github.com/rstudio/rstudio/issues/5809#issuecomment-932228146) which will hopefully be resolved in the near future.

## Installation

To install the latest development version of pkgpurl, run the following in R:

``` r
if (!("remotes" %in% rownames(installed.packages()))) {
  install.packages(pkgs = "remotes",
                   repos = "https://cloud.r-project.org/")
}

remotes::install_gitlab(repo = "salim_b/r/pkgs/pkgpurl")
```

## Usage

The (function) reference is found [here](reference).

## Development

### R Markdown format

This package's source code is written in the [R Markdown](https://rmarkdown.rstudio.com/) file format to facilitate practices commonly referred to as [*literate programming*](https://en.wikipedia.org/wiki/Literate_programming). It allows the actual code to be freely mixed with explanatory and supplementary information in expressive Markdown format instead of having to rely on [`#` comments](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Comments) only.

All the `.gen.R` suffixed R source code found under [`R/`](R/) is generated from the respective R Markdown counterparts under [`Rmd/`](Rmd/) using [`pkgpurl::purl_rmd()`](https://rpkg.dev/pkgpurl/reference/purl_rmd.html)[^4]. Always make changes only to the `.Rmd` files -- never the `.R` files -- and then run `pkgpurl::purl_rmd()` to regenerate the R source files.

### Coding style

This package borrows a lot of the [Tidyverse](https://www.tidyverse.org/) design philosophies. The R code adheres to the principles specified in the [Tidyverse Design Guide](https://principles.tidyverse.org/) wherever possible and is formatted according to the [Tidyverse Style Guide](https://style.tidyverse.org/) (TSG) with the following exceptions:

-   Line width is limited to **160 characters**, double the [limit proposed by the TSG](https://style.tidyverse.org/syntax.html#long-lines) (80 characters is ridiculously little given today's high-resolution wide screen monitors).

-   Usage of [magrittr's compound assignment pipe-operator `%<>%`](https://magrittr.tidyverse.org/reference/compound.html) is desirable[^5].

-   Usage of [R's right-hand assignment operator `->`](https://rdrr.io/r/base/assignOps.html) is not allowed[^6].

-   R source code is *not* split over several files as [suggested by the TSG](https://style.tidyverse.org/package-files.html) but instead is (as far as possible) kept in the single file [`Rmd/pkgpurl.Rmd`](Rmd/pkgpurl.Rmd) which is well-structured thanks to its [Markdown support](#r-markdown-format).

As far as possible, these deviations from the TSG plus some additional restrictions are formally specified in the [lintr configuration file](https://github.com/jimhester/lintr#project-configuration) [`.lintr`](.lintr), so lintr can be used right away to check for formatting issues:

``` r
pkgpurl::lint_rmd()
```

[^1]: Actually, you could write anything you like in any syntax outside of R code chunks as long as you don't mind the file to be *knittable* (which it doesn't have to be).

[^2]: It basically sends the (R) Markdown file on a "Pandoc round trip" on every file save.

[^3]: I personally recommend to use the shortcut <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>V</kbd> since it's not occupied yet by any of the predefined [RStudio shortcuts](https://support.rstudio.com/hc/en-us/articles/200711853-Keyboard-Shortcuts).

[^4]: This naming convention as well as the very idea to leverage the R Markdown format to author R packages was originally proposed by Yihui Xie. See his excellent [blog post](https://yihui.name/rlp/) for more detailed information about the benefits of literate programming techniques and some practical examples. Note that using `pkgpurl::purl_rmd()` is a less cumbersome alternative to the Makefile approach outlined by him.

[^5]: The TSG [explicitly instructs to avoid this operator](https://style.tidyverse.org/pipes.html#assignment-2) -- presumably because it's relatively unknown and therefore might be confused with the forward pipe operator `%>%` when skimming code only briefly. I don't consider this to be an actual issue since there aren't many sensible usage patterns of `%>%` at the beginning of a pipe sequence inside a function -- I can only think of creating side effects and relying on [R's implicit return of the last evaluated expression](https://rdrr.io/r/base/function.html). Therefore -- and because I really like the `%<>%` operator -- it's usage is welcome.

[^6]: The TSG [explicitly accepts `->` for assignments at the end of a pipe sequence](https://style.tidyverse.org/pipes.html#assignment-2) while Google's R Style Guide [considers this bad practice](https://google.github.io/styleguide/Rguide.html#right-hand-assignment) because it "makes it harder to see in code where an object is defined". I second the latter.
