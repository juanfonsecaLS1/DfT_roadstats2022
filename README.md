DfT Road Statistics Quick analysis
================
Juan P. Fonseca-Zamora

## Loading the libraries

``` r
library(dplyr)
library(ggplot2)
library(dftTrafficCounts)
library(tmap)
library(sf)
library(tmaptools)
```

## Loading the data

Data is loaded from the DfT repository

``` r
url = "https://storage.googleapis.com/dft-statistics/road-traffic/downloads/data-gov-uk/dft_traffic_counts_aadf.zip"
Roads = dtc_import(u = url)
```

Extracting a subset for the last couple of years

``` r
Recent_data = Roads |> filter(Year>2017)
```

## Quick stats

Number of sites per year by type of site

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

How many years of data per site:

``` r
per_year = Recent_data |> 
  group_by(Count_point_id) |>
  summarise(n_years=n()) 
```

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- --> \## Last
year’s data for Leeds

``` r
count_points=Recent_data |>
  filter(Year==2021,Local_authority_id==63) |>
  st_as_sf(coords=c("Longitude","Latitude"),crs="wgs84")

OSM_base = read_osm(count_points, ext=1.1)
tm_shape(OSM_base)+tm_rgb()+
tm_shape(count_points)+tm_dots(col="All_motor_vehicles",alpha=0.5,size = 1,palette = "viridis")
```

![](README_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
count_points |> ggplot(aes(factor(Road_type),y=All_motor_vehicles))+geom_jitter(alpha=0.3)+geom_boxplot(outlier.alpha = 0)
```

![](README_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

# To include in the analysis:

- get OSM network
- pair counts to links
- analyse width/other characteristic vs flow
- get webTRIS data for major
- compare estimated AADT
- speed flow curves…
