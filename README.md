# R Package Development Template

This is intended as a starting point for the development of R packages using [literate programming](https://en.wikipedia.org/wiki/Literate_programming) techniques [proposed by Yihui Xie](https://yihui.name/rlp/).

## Intructions

1. Copy all files and folders from this repository except `README.md` and `.git/` to the root of your new R package project.

2. Rename the file `gitignore` to `.gitignore` and delete all the `.gitkeep` files.

3. Rename all the `RENAME_ME.*` files to match your new R package.

4. Make sure the [R options](https://rdrr.io/r/base/options.html) `usethis.description` and `usethis.full_name` [are set](https://usethis.r-lib.org/articles/articles/usethis-setup.html)[^inspect-opts] and then run `usethis::use_description()` to initialize the new package.


[^inspect-opts]: To see the current values the DESCRIPTION will be based on, you can run `usethis::use_description_defaults()`.
