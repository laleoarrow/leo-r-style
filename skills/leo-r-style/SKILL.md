---
name: leo-r-style
description: Use when writing, refactoring, or standardizing R code in Ao Lu's style, including compact formatting, explicit Package::function() usage in package functions, roxygen2 contracts, cli notifications, and leo_log conventions.
---

# LEO R Style / LEO R 风格

Apply these rules whenever handling R code writing, refactoring, or formatting.

Always enforce this style for R package code and analysis scripts.

## Workflow / 工作流

1. Identify task type:
- Interactive `.R` analysis file.
- `Rscript`/CLI `.R` file.
- `.Rmd` analysis/report file.
- New R package function.
- Rewriting an existing function.
- General R script formatting/refactor.
2. Apply strict style rules from this file.
3. Use explicit `Package::function()` for non-base calls in package function bodies.
4. Run minimal checks after edits (`parse`, and package-context checks when needed).
5. Format output using the case-specific contract below.

## File-Type Gate / 文件类型判定

Classify the target before editing:

- Default ambiguous `.R` files to interactive analysis files.
- Treat a `.R` file as `Rscript`/CLI only when the file header explicitly marks it that way or the user explicitly says it is a script.
- Treat `.Rmd` files as literate analysis/report files unless the user explicitly says they are part of a scripted report pipeline.
- Treat package code as package code even if the user runs it interactively.

## R Code Style (Strict) / R 代码风格（严格）

1. Use Tidyverse style (`%>%`) where appropriate, and prefer dataframe workflows (`df`) over named vectors.
   - Prefer `%>%` over base pipe `|>` unless the user or project explicitly adopted `|>`. Do not silently switch between the two.
2. Booleans: `T` / `F` are acceptable for brevity in analysis scripts.
   - In **R package code** (`R/` directory), use `TRUE` / `FALSE` to satisfy `R CMD check` and `lintr`. `T`/`F` are only shorthand-safe in interactive `.R` and `.Rmd`.
3. Naming and formatting:
- Avoid excessive abbreviations (`df` is OK, `tr` is not).
- Use meaningful snake_case names, for example `leo_log`, `train_path`.
- Keep one space after commas and around binary operators.
4. Function structure:
- Keep definitions compact; prefer one-line signatures when they fit.
- Keep `{` on the same line and `}` on its own line.
- Align wrapped arguments to the opening parenthesis using hanging indent.
5. Line breaking and compactness (critical):
- Maximize line usage; do not break lines unless length is roughly over 100-120 chars.
- Avoid meaningless breaks for calls like `message(sprintf(...))`; keep them one line or compactly wrapped.
- Break parameters only when there are many (>10) or when logical groups are distinct.
6. Control flow:
- Prefer explicit `for (...) {}` / `if (...) {}` over `map*` for clarity.
- Keep loops and conditions compact.
7. Use `leo_log(...)` directly; do not write `leo_log(glue::glue(...))`.
   - `leo_log` is a project-internal logging function from the `FastUKB` package (or the user's own utility). It accepts `glue`-style `{var}` interpolation and a `level` parameter (`"info"`, `"success"`, `"warning"`, `"error"`). If `leo_log` is not available in the current project, fall back to `cli::cli_alert_info()` / `message()` and note the substitution.
8. Use `cli::cli_alert_info()` for notifications.
9. Keep comments left-aligned inside functions; avoid decorative formatting.
10. Avoid meaningless redundant temporary variables and auxiliary helper functions.
11. Function body section comments:
- Use hierarchical section markers for clarity in long functions:
  - Level 1: `# section name ----`
  - Level 2: `## subsection name ----`
- Place section comments on their own line before the code block they describe.
- Use English for section names, keep them short and descriptive.
- Example:
```r
leo_cox_mediation <- function(...) {

  # initialization and validation ----
  t0 <- Sys.time()
  # ... validation code ...

  # helper functions ----
  format_num <- function(x) sprintf("%.3f", round(x, 3))

  # data preparation ----
  model_list <- .normalize_model_list(x_cov, df_colnames = names(df))

  # main loop ----
  for (model_name in names(model_list)) {
    ## exposure processing ----
    # ... code ...

    ## mediator processing ----
    # ... code ...

    ## regmedint fitting ----
    # ... code ...
  }

  # result assembly ----
  result_detail <- do.call(rbind, result_rows)
}
```

## Example: Compact Style

```r
# Preferred: compact definition
remove_unwant_hvg <- function(srt, pattern_list = list(m_genes = "^MT-", r_genes = "^(RPL|RPS)", t_genes = "^TR[ABDG]", h_genes = "^HB[AB]")) {
  # ... implementation ...
}

# Preferred: compact logic
message(sprintf("remove_unwant_hvg(): kept %d (%.1f%%), removed %d (%.1f%%)", n_rm, 100 * n_rm / n_tot, n_tot - n_rm, 100 * (n_tot - n_rm) / n_tot))
```

## Code Requirements By Case / 分场景代码要求

1. If editing an interactive `.R` analysis file:
- Default to this mode for ambiguous `.R` targets.
- Keep code in execution order from top to bottom.
- Put logic where it is used instead of extracting distant helpers.
- Avoid meaningless temporary variables and unnecessary helper functions.

2. If editing an `Rscript`/CLI `.R` file:
- Use this mode only when the file header or user context clearly marks it as a script.
- Keep the entrypoint obvious and preserve argument, config, and output semantics.
- Keep implementation compact; extract helpers only when they materially improve readability or reuse.

3. If editing an `.Rmd` analysis/report file:
- Keep chunk order aligned with the narrative and analysis flow.
- Keep code close to the tables, figures, and results that depend on it.
- Avoid refactors that hide state transitions across distant chunks.

4. If writing an R package function:
- Add roxygen2 docs (`@param`, `@return`, `@examples`, `@export` as needed).
- Add `@importFrom package function` for non-base dependencies.
- In function bodies, call non-base functions as `Package::function()`.
- Use `cli` prompts for necessary progress updates.
- Log completion with `leo_log(..., level = "success")`.

5. If rewriting an existing function:
- Output only the modified parts.
- Place modified code first.
- After code, add concise but vivid reasons for the changes.
- Avoid unrelated commentary.

6. Always avoid meaningless redundant temporary variable construction and unnecessary helper functions.

## Smoke Test / 烟测
- "Format this 200-line analysis script." → Classify as interactive `.R`, keep top-to-bottom order, apply compact style, use `%>%`.
- "Refactor this exported package function." → Add roxygen2, switch `T/F` to `TRUE/FALSE`, use `Package::function()`, add `leo_log(..., level = "success")`.
- "Clean up this Rmd." → Keep chunk order aligned with narrative, do not extract helpers to distant chunks.
- "This script uses `|>` everywhere, reformat it." → Respect the project's pipe choice, do not replace `|>` with `%>%`.
