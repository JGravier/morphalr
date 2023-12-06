
## package development

[![License: GPL
v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
![R](https://img.shields.io/badge/R-%3E%3D%202.10-blue) ![R CMD
check](https://img.shields.io/badge/R%20CMD%20check-passing-green)
![Coverage](https://img.shields.io/badge/coverage-20%25-red)

## morphalr: Morphological Analysis for Archaeology

A package to compute morphological indices of spatial entities
(e.g. parcels, buildings). It also provides visuals of multivariate
statistics results (ACP and HCA).

## Installing

Package currently exist as development on github.

Install package from github:

``` r
library(remotes)
install_github(repo = "JGravier/morphalr")
```

## Compute orientations from polygon

Load data of parcels of the city of Rouen in 1827 and extract segments
(lines) from polygons.

``` r
library(tidyverse)
library(sf)
library(morphalr)

# transform polygons as segments (lines)
linerouen <- morphalr_geom_to_segment(sfobject = rouen_1827, to = 'LINESTRING')
```

`morphal_geom_to_segment()` can be applied on LINESTRING, e.g. in the
case of streets modeled as lines.

Compute orientations of lines with North or East looking parameters.

``` r
orientationsnord <- morphalr_segment_orientation(sfsegments = linerouen)
orientationsest <- morphalr_segment_orientation(sfsegments = linerouen, looking = 'E')

orientationsnord |>
  mutate(looking = 'North') |>
  bind_rows(orientationsest |> mutate(looking = 'East')) |>
  ggplot() +
  geom_sf(aes(color = orientation)) +
  scale_color_gradient2() +
  facet_wrap(~looking) +
  theme_bw() +
  theme(axis.ticks = element_blank(), axis.text = element_blank())
```

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
