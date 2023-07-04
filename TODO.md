# TODOs

-   Add `.qmd` support

-   Mention RStudio-Add-In [splitChunk](https://github.com/LudvigOlsen/splitChunk/) and file a bug report for native RStudio support for this functionality (the
    add-in has a few issues; e.g. splits in the wrong source pane when invoked from command palette and the active source pane is not primary one)

-   Check out

    -   <https://rtask.thinkr.fr/when-development-starts-with-documentation/>
    -   pkg [fusen](https://github.com/ThinkR-open/fusen)

-   Advertise package at relevant places

    -   <https://github.com/rstudio/rstudio/issues/5809>

-   Propose a standard way to render a package's main `Rmd/*.Rmd` source file for the pkgdown site (as an article, or better as main menu item), so people can
    view all Markdown annotations etc. accompanying the code ("literate programming") in the most readable way? Contra: Benefit compared to the [rendered GitLab
    view](https://gitlab.com/rpkg.dev/pkgpurl/-/blob/master/Rmd/pkgpurl.Rmd?plain=0) would be minimal (currently mainly R code syntax highlighting).

    Basically, we'd need to add a vignette `vignettes/source_code.Rmd` with the following content:

    ```` rmd
    ---
    title: "R Markdown source code"
    output: rmarkdown::html_vignette
    vignette: >
      %\VignetteIndexEntry{R Markdown source code}
      %\VignetteEngine{knitr::rmarkdown}
      %\VignetteEncoding{UTF-8}
    ---

    ```{r, child = "../Rmd/pkgpurl.Rmd", results = "hide"}
    ```
    ````

    What's left to do would be

    1.  Differentiate between actual source code blocks (starting with ```` ```{r} ````) and code blocks in accompanying documentation!

    2.  Tweak the CSS to reduce the font size of source code blocks.

    3.  Tweak the CSS to extend width to the right for source code blocks.

