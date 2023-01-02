
<!-- README.md is generated from README.Rmd. Please edit that file -->

# gguidance

<!-- badges: start -->

[![CRAN
status](https://www.r-pkg.org/badges/version/gguidance)](https://CRAN.R-project.org/package=gguidance)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![Codecov test
coverage](https://codecov.io/gh/teunbrand/gguidance/branch/master/graph/badge.svg)](https://app.codecov.io/gh/teunbrand/gguidance?branch=master)
[![R-CMD-check](https://github.com/teunbrand/gguidance/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/teunbrand/gguidance/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

> **Warning** You’re looking at an experimental branch that explores
> guide extensions with an upcoming ggproto overhaul in ggplot2.

The goal of gguidance is to provide additional guides to the ggplot2
ecosystem.

Please note that this repo is still being worked on and, while probably
usable, isn’t finished.

## Installation

You can install the development version of gguidance from
[GitHub](https://github.com/) with:

``` r
# install.packages("remotes")
remotes::install_github("tidyverse/ggplot2", ref = remotes::github_pull("4879"))
remotes::install_github("teunbrand/gguidance", ref = "main")
```

## Examples

Let’s first set up a basic plot to experiment with

``` r
library(gguidance)
#> Loading required package: ggplot2

p <- ggplot(mpg, aes(displ, hwy)) +
  geom_point() +
  labs(
    x = "Engine displacement",
    y = "Highway miles per gallon"
  ) +
  theme(axis.line = element_line())
```

### Legends

A ‘cross legend’ can show two variables in a single legend.

``` r
p + aes(colour = paste(cyl, year)) +
  guides(colour = "legend_cross")
```

<img src="man/figures/README-legend_cross-1.png" width="80%" />

A string legend doesn’t display keys, but colours the labels.

``` r
p + aes(colour = class) +
  guides(colour = "legend_string")
```

<img src="man/figures/README-legend_string-1.png" width="80%" />

### Colour bars

A capped colour bar:

``` r
p + aes(colour = cty) +
  scale_colour_viridis_c(guide = "colourbar_cap")
```

<img src="man/figures/README-colourbar_cap-1.png" width="80%" />

Using a violin as a colour guide:

``` r
p + aes(colour = cty) +
  scale_colour_viridis_c(guide = guide_colour_violin(density = mpg$cty))
```

<img src="man/figures/README-colour_violin-1.png" width="80%" />

### Axes

Using subtitles.

``` r
p + guides(x = guide_axis_extend(subtitle = c("Less", "More")))
```

<img src="man/figures/README-axis_extend-1.png" width="80%" />

Using minor ticks.

``` r
p + guides(x = "axis_minor")
```

<img src="man/figures/README-axis_minor-1.png" width="80%" />

Using capped lines.

``` r
p + guides(x = "axis_cap")
```

<img src="man/figures/README-axis_cap-1.png" width="80%" />

With bracketed ranges.

``` r
ggplot(mpg, aes(class, displ)) +
  geom_boxplot() +
  guides(x = guide_axis_nested(
    range_start = c(0.5, 3.5),
    range_end   = c(4.5, 6.5),
    range_name  = c("First range", "Second range"),
    bracket     = "square" 
  ))
```

<img src="man/figures/README-axis_nested-1.png" width="80%" />

Using a table as an axis guide.

``` r
# Creating summary table
my_table <- lapply(split(mpg[, c("displ", "cty", "hwy")], mpg$cyl), colMeans)
my_table <- as.data.frame(do.call(rbind, my_table))
my_table[] <- lapply(my_table, scales::number, accuracy = 0.01)
my_table$cyl <- rownames(my_table)

# Use summary table as axis guide
ggplot(mpg, aes(factor(cyl), displ)) +
  geom_boxplot() +
  guides(x = guide_axis_table(table = my_table, key_col = cyl))
```

<img src="man/figures/README-axis_table-1.png" width="80%" />
