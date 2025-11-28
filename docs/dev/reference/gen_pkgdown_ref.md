# Generate pkgdown reference index

Generates the [pkgdown reference
index](https://pkgdown.r-lib.org/reference/build_reference.html#reference-index)
based on the heading hierarchy structure of a package's main `.Rmd`
file.

## Usage

``` r
gen_pkgdown_ref(rmd, env = parent.frame())
```

## Arguments

- rmd:

  The (R) Markdown file content as a character scalar.

- env:

  Environment to evaluate R Markdown inline code expressions in.

## Value

A list.

## Details

Basically, all elements of the pkgdown reference index except `desc`
keys are derived from `rmd`'s [Markdown
**headings**](https://pandoc.org/MANUAL.html#headings) and their
**hierarchy**. The `desc` keys, however, are assembled from the
paragraph(s) below some specially named headings. See below for details.

## Special headings

Headings that exactly match certain strings (case-insensitive, but
without any inline formatting or additional text) are treated specially.
Here's an overview:

|  |  |
|----|----|
| **Headings** | **Meaning** |
| DESCRIPTION | Paragraph(s) below this heading, which in turn is hierarchically below a `title` or `subtitle` heading, are used as the description for this very (sub)title (`desc` in the pkgdown reference index). |
| EXPORTED | Will never be used as `title` or `subtitle` in the pkgdown reference index (i.e. ignored). Simply serves as a (usually top-level) heading to indicate that the objects defined below it are [exported](https://r-pkgs.org/dependencies-in-practice.html#exports) by the package. |
| INTERNAL , NOTES , TEMPORARY, TMP | Everything that's hierarchically below one of these headings is completely ignored for pkgdown reference index generation. |

## Parsing rules

To be able to unambiguously map an `.Rmd` file's heading hierarchy to
the pkgdown reference index, some parsing rules are necessary. Attention
must be paid to the fact that, while (R) Markdown supports up to six
heading levels (corresponding to HTML's `<h1>`–`<h6>` tags), the pkgdown
reference index only supports up to two (`title` and `subtitle`).

The following rules define how the reference index is generated:

1.  Headings below a heading named *INTERNAL*, *NOTES*, *TEMPORARY* or
    *TMP* (case-insensitive, but without any inline formatting) are
    simply ignored when generating the reference index.

2.  Every heading that is a) inline-formatted as
    [verbatim](https://pandoc.org/MANUAL.html#verbatim) and b) doesn't
    contain any whitespace characters is considered to be the name of a
    help topic (usually the name of a function or dataset) to be
    included in the reference index. This maps to the `contents` key of
    the reference index' YAML.

3.  Non-help-topic headings above help topic headings are used as
    reference index (sub)titles as far as hierarchical nesting allows.
    More precisely, non-help-topic headings *of the highest two levels*
    above the help topic heading are used as title and subtitle, the
    rest of the headings above the help topic heading is ignored – and a
    heading named *EXPORTED* (case-insensitive, but without any inline
    formatting) is always ignored regardless of its level. This maps to
    the `title` and `subtitle` keys of the reference index' YAML.

4.  Paragraph(s) below a heading named *DESCRIPTION* (case-insensitive,
    but without any inline formatting), that in turn is hierarchically
    below a title or subtitle heading, are used as the description for
    the respective (sub)title. This maps to the `desc` key of the
    reference index' YAML.

## Parsing example

An example might better explain how the parsing rules work than a
thousand words, so here's a simplified one.

When fed to `gen_pkgdown_ref()`, the following R Markdown content...

    ---
    editor_options:
      markdown:
        wrap: 80
    ---

    # `main_fn`

    The fn below is under no title or subtitle in the pkgdown index.

    ```{r}
    main_fn <- function(...) {...}
    ```

    # First group of fns

    ## DESCRIPTION

    The description text for *First group of fns*.

    ## `group_wide_fn`

    The fn below is only under a title, but no subtitle in the pkgdown index.

    ```{r}
    group_wide_fn <- function(...) {...}
    ```

    ## Some subgroup

    The fns below are both under a title and subtitle in the pkgdown index.

    ### `subgroup_fn_1`

    ```{r}
    subgroup_fn_1 <- function(...) {...}
    ```

    ### `subgroup_fn_2`

    ```{r}
    subgroup_fn_2 <- function(...) {...}
    ```

    ### Sub-subgroup

    The above *Sub-subgroup* heading is ignored for the pkgdown index because the
    latter only supports up to two heading levels.

    The fn below thus moves one hierarchy level up in the pkgdown index.

    #### `sub_subgroup_fn`

    ```{r}
    sub_subgroup_fn <- function(...) {...}
    ```

    ## Another subgroup

    ### DESCRIPTION

    The description text for *Another subgroup*.

    ### `another_subgroup_fn`

    ```{r}
    another_subgroup_fn <- function(...) {...}
    ```

    # Second group of fns

    ## Yet another subgroup

    ### `yet_another_subgroup_fn`

    ```{r}
    yet_another_subgroup_fn <- function(...) {...}
    ```

...yields this pkgdown index (converted [to
YAML](https://rdrr.io/pkg/yaml/man/as.yaml.html)):

    reference:
    - contents: main_fn
    - title: First group of fns
      desc: The description text for *First group of fns*.
    - contents: group_wide_fn
    - subtitle: Some subgroup
    - contents:
      - subgroup_fn_1
      - subgroup_fn_2
      - sub_subgroup_fn
    - subtitle: Another subgroup
      desc: The description text for *Another subgroup*.
    - contents: another_subgroup_fn
    - title: Second group of fns
    - subtitle: Yet another subgroup
    - contents: yet_another_subgroup_fn

## Inline R code

R Markdown [inline code](https://rmarkdown.rstudio.com/lesson-4.html) is
fully supported in headings and descriptions, except for the above
mentioned special headings (otherwise, they're not recognized as special
headings anymore).

## Examples

``` r
if (pal::is_pkg_installed("tinkr")) {
  yay::gh_text_file(path = "Rmd/pal.Rmd",
                    owner = "salim-b",
                    name = "pal") |>
    pkgpurl::gen_pkgdown_ref() |>
    yaml::as.yaml() |>
    cat()
}
#> reference:
#> - title: Statistical computing / numbers
#> - contents:
#>   - safe_seq_len
#>   - safe_max
#>   - safe_min
#>   - round_to
#>   - stat_mode
#> - title: Data frames / Tibbles
#> - contents:
#>   - assert_cols
#>   - is_equal_df
#>   - reduce_df_list
#> - title: Lists
#> - contents: as_flat_list
#> - title: Strings
#> - subtitle: Vectorized
#> - contents:
#>   - as_chr
#>   - escape_lf
#>   - phrase_nr
#>   - sentenceify
#>   - capitalize_first
#>   - wrap_chr
#> - subtitle: Non-vectorized
#> - contents:
#>   - as_line_feed_chr
#>   - dsv_colnames
#> - subtitle: Collapsed into character scalar
#> - contents:
#>   - as_str
#>   - as_comment_str
#>   - as_env_var_name
#>   - enum_str
#>   - fuse_regex
#> - title: Filesystem paths
#>   desc: Functions to work with filesystem paths.
#> - contents:
#>   - path_mod_time
#>   - flatten_path_tree
#>   - draw_path_tree
#> - title: Dots
#>   desc: |-
#>     Extending [rlang's *check dots* functions](https://rlang.r-lib.org/reference/index.html#check-dots), making the use of [R's `...` argument
#>     placeholder](https://rdrr.io/r/base/dots.html) yet another bit safer.
#> - contents: check_dots_named
#> - title: R packages
#> - contents:
#>   - ls_pkg
#>   - use_pkg
#>   - is_pkg_installed
#>   - is_pkg_cran
#>   - is_pkg_dir
#>   - is_pkgdown_dir
#>   - exists_in_namespace
#>   - reason_pkg_required
#> - subtitle: Package DESCRIPTION
#>   desc: Extending the [**desc**](https://github.com/r-lib/desc#readme) package.
#> - contents:
#>   - desc_list
#>   - desc_get_field_safe
#>   - desc_dep_vrsn
#>   - desc_url_git
#> - subtitle: Package documentation
#>   desc: Complementing and extending the [**roxygen2**](https://roxygen2.r-lib.org/)
#>     package.
#> - contents:
#>   - fn_param_defaults
#>   - enum_fn_param_defaults
#>   - roxy_to_md_links
#>   - roxy_blocks
#>   - roxy_obj
#>   - roxy_tag_value
#> - title: (Pandoc) Markdown
#> - contents:
#>   - md_verb
#>   - as_md_list
#>   - as_md_vals
#>   - as_md_val_list
#>   - pipe_table
#>   - strip_md
#>   - strip_md_footnotes
#> - subtitle: CommonMark parsing
#>   desc: Extending the [**commonmark**](https://github.com/r-lib/commonmark#readme)
#>     package.
#> - contents:
#>   - md_to_xml
#>   - md_xml_subnode_ix
#>   - xml_to_md
#> - title: R Markdown / knitr
#> - contents:
#>   - build_readme
#>   - knitr_table_format
#>   - strip_yaml_header
#> - subtitle: Output formats
#>   desc: |-
#>     [Custom R Markdown output formats](https://bookdown.org/yihui/rmarkdown/new-formats.html) which can be used in addition to the [default output
#>     formats](https://bookdown.org/yihui/rmarkdown/output-formats.html).
#> - contents: gitlab_document
#> - title: HTML widgets
#>   desc: Extending the [**htmlwidgets**](https://www.htmlwidgets.org/) package.
#> - contents:
#>   - write_widget
#>   - write_widget_deps
#> - title: checkmate
#>   desc: Extending the [**checkmate**](https://mllg.github.io/checkmate/) package.
#> - contents:
#>   - assert_class_any
#>   - assert_df_or_tibble
#>   - assert_inf_count
#> - title: cli
#>   desc: Extending the [**cli**](https://cli.r-lib.org/) package.
#> - contents:
#>   - cli_qty_lgl
#>   - cli_no_lgl
#>   - cli_progress_step_quick
#>   - cli_process_expr
#> - title: Git
#>   desc: Extending the [**gert**](https://docs.ropensci.org/gert/) and [**git2r**](https://docs.ropensci.org/git2r/)
#>     packages.
#> - contents:
#>   - git_file_mod_time
#>   - git_remote_tree_url
#> - title: HTTP
#>   desc: Extending the [**httr2**](https://httr2.r-lib.org/) package which is built
#>     upon the [curl](https://jeroen.cran.dev/curl/) package.
#> - contents:
#>   - is_http_success
#>   - is_url
#>   - http_get_cached
#> - title: TOML
#> - contents:
#>   - toml_read
#>   - toml_validate
#> - title: System interaction
#> - contents:
#>   - test_cli
#>   - assert_cli
#> - title: Miscellaneous
#> - contents:
#>   - capture_print
#>   - cat_lines
#>   - cols_regex
#>   - mime_to_ext
#>   - rename_from
#>   - arrange_by
#>   - when
```
