
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Example Simple Forecast Hub

**This repository is under construction.**

This repository is designed as an example modeling Hub that follows the
infrastructure guidelines laid out by the [Consortium of Infectious
Disease Modeling Hubs](https://github.com/hubverse-org/). [The examples
given in
hubDocs](https://hubverse.io/en/latest/user-guide/intro-data-formats.html#running-examples)
provide additional details.

The example model outputs provided here are adapted from forecasts
submitted to the US COVID-19 Forecast Hub but have been modified to
provide examples of nowcasts. They should be viewed only as
illustrations of the data formats, not as realistic examples of nowcasts
and forecasts. In particular, scores calculated by comparing the model
outputs to the target data will not give a meaningful measure of
predictive skill.

## Working with the data

To work with the data in R, you can use code like the following:

``` r
library(hubData)
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union

model_outputs <- hubData::connect_hub(hub_path = ".") %>%
    dplyr::collect()
head(model_outputs)
#> # A tibble: 6 × 8
#>   origin_date horizon location target  output_type output_type_id value model_id
#>   <date>        <int> <chr>    <chr>   <chr>                <dbl> <int> <chr>   
#> 1 2022-12-05       -6 20       inc co… quantile             0.01     22 UMass-ar
#> 2 2022-12-05       -6 20       inc co… quantile             0.025    24 UMass-ar
#> 3 2022-12-05       -6 20       inc co… quantile             0.05     26 UMass-ar
#> 4 2022-12-05       -6 20       inc co… quantile             0.1      28 UMass-ar
#> 5 2022-12-05       -6 20       inc co… quantile             0.15     30 UMass-ar
#> 6 2022-12-05       -6 20       inc co… quantile             0.2      32 UMass-ar

target_data <- read.csv("target-data/covid-hospitalizations.csv")
head(target_data)
#>     time_idx location value         target
#> 1 2021-03-21       46    12 inc covid hosp
#> 2 2021-03-04       45    82 inc covid hosp
#> 3 2021-02-26       46     7 inc covid hosp
#> 4 2021-02-20       44    21 inc covid hosp
#> 5 2021-02-09       44    19 inc covid hosp
#> 6 2021-01-25       25   224 inc covid hosp
```
