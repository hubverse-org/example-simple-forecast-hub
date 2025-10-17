
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Example Simple Forecast Hub

This repository is designed as an example modeling Hub that follows the
infrastructure guidelines laid out by the [Consortium of Infectious
Disease Modeling Hubs](https://github.com/hubverse-org/). Additional
details are provided in [the examples given in
hubDocs](https://docs.hubverse.io/en/latest/user-guide/intro-data-formats.html#running-examples).

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

model_outputs <- hubData::connect_hub(hub_path = ".") |> 
    dplyr::collect()
model_outputs
#> # A tibble: 13,608 × 8
#>    origin_date horizon location target output_type output_type_id value model_id
#>    <date>        <int> <chr>    <chr>  <chr>                <dbl> <int> <chr>   
#>  1 2022-12-05       -6 20       inc c… quantile             0.01     22 UMass-ar
#>  2 2022-12-05       -6 20       inc c… quantile             0.025    24 UMass-ar
#>  3 2022-12-05       -6 20       inc c… quantile             0.05     26 UMass-ar
#>  4 2022-12-05       -6 20       inc c… quantile             0.1      28 UMass-ar
#>  5 2022-12-05       -6 20       inc c… quantile             0.15     30 UMass-ar
#>  6 2022-12-05       -6 20       inc c… quantile             0.2      32 UMass-ar
#>  7 2022-12-05       -6 20       inc c… quantile             0.25     34 UMass-ar
#>  8 2022-12-05       -6 20       inc c… quantile             0.3      35 UMass-ar
#>  9 2022-12-05       -6 20       inc c… quantile             0.35     36 UMass-ar
#> 10 2022-12-05       -6 20       inc c… quantile             0.4      37 UMass-ar
#> # ℹ 13,598 more rows

ts_target_data <- hubData::connect_target_timeseries() |> 
  dplyr::collect()
ts_target_data |> 
  dplyr::filter(location == 25) |> 
  dplyr::arrange(desc(time_idx))
#> # A tibble: 1,056 × 4
#>    time_idx   location value target        
#>    <date>     <chr>    <int> <chr>         
#>  1 2023-06-03 25          30 inc covid hosp
#>  2 2023-06-02 25          30 inc covid hosp
#>  3 2023-06-01 25          30 inc covid hosp
#>  4 2023-05-31 25          29 inc covid hosp
#>  5 2023-05-30 25          33 inc covid hosp
#>  6 2023-05-29 25          28 inc covid hosp
#>  7 2023-05-28 25          29 inc covid hosp
#>  8 2023-05-27 25          15 inc covid hosp
#>  9 2023-05-26 25          20 inc covid hosp
#> 10 2023-05-25 25          26 inc covid hosp
#> # ℹ 1,046 more rows
```

One slightly unusual aspect to these data are that the model outputs use
`origin_date` and `horizon` to indicate the date for which a forecast is
made. The time-scale is days, so a prediction with `origin_date` of
`2022-12-08` and `horizon` of `-6` corresponds to a prediction for
`2022-12-02`. There is no one task-id column in the model output files
that corresponds to this “target date”. In the time-series target data
file, there is the `time_idx` column, which does correspond to the date
being predicted.

We note that this set-up, while valid according to the hubverse
standards, might not work out of the box with downstream tools (e.g.,
predtimechart for the dashboards) without additional pre-processing. An
alternative to this set-up could be to include the column of `time_idx`
in the model-output files as a task id variables as well.
