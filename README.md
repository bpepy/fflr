
<!-- README.md is generated from README.Rmd. Please edit that file -->

# fflr <img src="man/figures/logo.png" align="right" width="120" />

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![CRAN
status](https://www.r-pkg.org/badges/version/fflr)](https://CRAN.R-project.org/package=fflr)
[![Travis build
status](https://travis-ci.org/kiernann/fflr.svg?branch=master)](https://travis-ci.org/kiernann/fflr)
<!-- badges: end -->

The `fflr` package is used to query the [ESPN Fantasy Football
API](https://fantasy.espn.com/apis/v3/games/ffl/), (for current and past
seasons) and format the results into [tidy data
frames](https://vita.had.co.nz/papers/tidy-data.pdf).

## Installation

You can install the released version of `fflr` from GitHub with:

``` r
# install.packages("remotes")
remotes::install_github("kiernann/fflr")
```

## Usage

Here we see how we can chain together `fantasy_*()` and `form_*()`
functions to plot the weekly scores of each team in a stacked bar chart.

``` r
library(tidyverse)
library(fflr)

gaa <- extract_lid("https://fantasy.espn.com/football/team?leagueId=252353&teamId=6")
teams <- form_teams(data = fantasy_members(lid = gaa))
scores <- form_matchup(data = fantasy_matchup(lid = gaa))

right_join(scores, teams) %>% 
  ggplot(aes(x = reorder(abbrev, score), y = score)) +
  geom_col(aes(fill = week)) +
  scale_fill_brewer(palette = "Dark2", guide = guide_legend(reverse = TRUE)) +
  labs(title = "Fantasy Football Scores", x = "team") +
  theme(legend.position = "bottom") +
  coord_flip()
```

<img src="man/figures/README-score_plot-1.png" width="100%" />
