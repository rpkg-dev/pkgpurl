# pkgpurl's default linters

Opinionated set of
[linters](https://lintr.r-lib.org/reference/linters.html). Built from
[`lintr::linters_with_defaults()`](https://lintr.r-lib.org/reference/linters_with_defaults.html)
with lots of customizations. See the [relevant source
code](https://gitlab.com/rpkg.dev/pkgpurl/-/blob/master/Rmd/sysdata.nopurl.Rmd#default_linters)
for details.

## Usage

``` r
default_linters
```

## Format

A named list of
[lintr::linters](https://lintr.r-lib.org/reference/linters.html).

## See also

[`default_exclusions()`](https://pkgpurl.rpkg.dev/dev/reference/default_exclusions.md)
and [`lint_rmd()`](https://pkgpurl.rpkg.dev/dev/reference/lint_rmd.md)

## Examples

``` r
names(pkgpurl::default_linters)
#>  [1] "absolute_path_linter"             "any_duplicated_linter"            "any_is_na_linter"                 "assignment_linter"               
#>  [5] "boolean_arithmetic_linter"        "brace_linter"                     "class_equals_linter"              "commas_linter"                   
#>  [9] "commented_code_linter"            "comparison_negation_linter"       "condition_call_linter"            "condition_message_linter"        
#> [13] "conjunct_test_linter"             "consecutive_assertion_linter"     "consecutive_mutate_linter"        "cyclocomp_linter"                
#> [17] "empty_assignment_linter"          "equals_na_linter"                 "expect_comparison_linter"         "expect_identical_linter"         
#> [21] "expect_length_linter"             "expect_named_linter"              "expect_not_linter"                "expect_null_linter"              
#> [25] "expect_s3_class_linter"           "expect_s4_class_linter"           "expect_true_false_linter"         "expect_type_linter"              
#> [29] "fixed_regex_linter"               "for_loop_index_linter"            "function_argument_linter"         "function_left_parentheses_linter"
#> [33] "function_return_linter"           "if_not_else_linter"               "if_switch_linter"                 "ifelse_censor_linter"            
#> [37] "implicit_assignment_linter"       "implicit_integer_linter"          "infix_spaces_linter"              "inner_combine_linter"            
#> [41] "is_numeric_linter"                "keyword_quote_linter"             "length_levels_linter"             "length_test_linter"              
#> [45] "lengths_linter"                   "line_length_linter"               "list_comparison_linter"           "literal_coercion_linter"         
#> [49] "missing_argument_linter"          "nested_ifelse_linter"             "nrow_subset_linter"               "numeric_leading_zero_linter"     
#> [53] "nzchar_linter"                    "object_length_linter"             "object_name_linter"               "object_overwrite_linter"         
#> [57] "one_call_pipe_linter"             "outer_negation_linter"            "paren_body_linter"                "paste_linter"                    
#> [61] "pipe_call_linter"                 "pipe_continuation_linter"         "pipe_return_linter"               "print_linter"                    
#> [65] "redundant_equals_linter"          "redundant_ifelse_linter"          "regex_subset_linter"              "rep_len_linter"                  
#> [69] "repeat_linter"                    "return_linter"                    "routine_registration_linter"      "sample_int_linter"               
#> [73] "scalar_in_linter"                 "semicolon_linter"                 "seq_linter"                       "sort_linter"                     
#> [77] "spaces_inside_linter"             "spaces_left_parentheses_linter"   "sprintf_linter"                   "stopifnot_all_linter"            
#> [81] "string_boundary_linter"           "system_file_linter"               "T_and_F_symbol_linter"            "terminal_close_linter"           
#> [85] "todo_comment_linter"              "trailing_blank_lines_linter"      "undesirable_function_linter"      "undesirable_operator_linter"     
#> [89] "unnecessary_concatenation_linter" "unnecessary_lambda_linter"        "unnecessary_nesting_linter"       "unnecessary_placeholder_linter"  
#> [93] "unreachable_code_linter"          "vector_logic_linter"              "which_grepl_linter"               "whitespace_linter"               
#> [97] "yoda_test_linter"                
```
