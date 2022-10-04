viz_part_01
================

## lets import data’

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.0      ✔ stringr 1.4.0 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.1 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggridges)
library(viridis)
```

    ## Loading required package: viridisLite

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2022-09-29 10:34:03 (8.401)

    ## file min/max dates: 1869-01-01 / 2022-09-30

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-09-29 10:34:08 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-09-29 10:34:11 (0.95)

    ## file min/max dates: 1999-09-01 / 2022-09-30

``` r
weather_df
```

    ## # A tibble: 1,095 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows
    ## # ℹ Use `print(n = ...)` to see more rows

## scatterplot

but better

``` r
weather_df %>% 
  ggplot(aes(x=tmin, y=tmax, color = name)) +
  geom_point(alpha=.5) +
  labs(
    x = "minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily Temp",
    caption = "data came from the rnoaa package"
  ) +
  scale_x_continuous(
    breaks = c(-10, 0, 15),
    labels = c("-10", "0", "15")
  ) + scale_y_continuous(
    trans = "sqrt"
  )
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x=tmin, y=tmax, color = name)) +
  geom_point(alpha=.5) +
  labs(
    x = "minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily Temp",
    caption = "data came from the rnoaa package"
  ) +
  scale_color_hue(
    name = "location",
    h = c(100,300)
    )
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
gg_weather = 
  weather_df %>% 
  ggplot(aes(x=tmin, y=tmax, color = name)) +
  geom_point(alpha=.5) +
  labs(
    x = "minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily Temp",
    caption = "data came from the rnoaa package"
  ) +
  viridis::scale_color_viridis(
    name = "Location", 
    discrete = TRUE
  )
```

## Themes

``` r
gg_weather +
  theme(legend.position = "bottom")
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
gg_weather +
  theme_bw()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
gg_weather +
  theme_minimal()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-7-2.png)<!-- -->

``` r
 gg_weather +
  theme_classic()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-7-3.png)<!-- -->

``` r
gg_weather +
  theme_minimal() +
  theme(legend.position = "bottom") 
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_par_02_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
