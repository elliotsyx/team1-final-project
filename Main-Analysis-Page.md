Main analysis page
================
Yixiao Sun
2023-11-20

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readr)
library(ggplot2)
library(ggmap)
```

    ## ℹ Google's Terms of Service: <https://mapsplatform.google.com>
    ##   Stadia Maps' Terms of Service: <https://stadiamaps.com/terms-of-service/>
    ##   OpenStreetMap's Tile Usage Policy: <https://operations.osmfoundation.org/policies/tiles/>
    ## ℹ Please cite ggmap if you use it! Use `citation("ggmap")` for details.

``` r
library(sf)
```

    ## Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE

``` r
library(BSDA)
```

    ## Loading required package: lattice
    ## 
    ## Attaching package: 'BSDA'
    ## 
    ## The following object is masked from 'package:datasets':
    ## 
    ##     Orange

``` r
NYPD_Shooting_Incident_2006_2022 = 
  read_csv("data/NYPD_Shooting_Incident_Data__Historic__20231120.csv") |>
  janitor::clean_names() |>
  select(-lon_lat, -statistical_murder_flag)
```

    ## Rows: 27312 Columns: 21
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (12): OCCUR_DATE, BORO, LOC_OF_OCCUR_DESC, LOC_CLASSFCTN_DESC, LOCATION...
    ## dbl   (7): INCIDENT_KEY, PRECINCT, JURISDICTION_CODE, X_COORD_CD, Y_COORD_CD...
    ## lgl   (1): STATISTICAL_MURDER_FLAG
    ## time  (1): OCCUR_TIME
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
NYPD_Shooting_Incident_2023 = 
  read_csv("data/NYPD_Shooting_Incident_Data__Year_To_Date__20231129.csv") |>
  janitor::clean_names() |>
  select(-new_georeferenced_column, -statistical_murder_flag)
```

    ## Rows: 991 Columns: 21
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (13): OCCUR_DATE, BORO, LOC_OF_OCCUR_DESC, LOC_CLASSFCTN_DESC, LOCATION...
    ## dbl   (5): INCIDENT_KEY, PRECINCT, JURISDICTION_CODE, Latitude, Longitude
    ## num   (2): X_COORD_CD, Y_COORD_CD
    ## time  (1): OCCUR_TIME
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
# NYUR <- read_csv("data/NYUR.csv")
# View(NYUR)
```

``` r
NYPD_Shooting_Incident_cleaned = 
  bind_rows(NYPD_Shooting_Incident_2006_2022, NYPD_Shooting_Incident_2023) |>
  separate(occur_date, into = c("month", "day", "year"), sep = "/") |>
  separate(occur_time, into = c("hour", "minute", "second"), sep = ":") |>
  select(-minute, -second, -loc_of_occur_desc, -loc_classfctn_desc, -location_desc)

# NYUR_cleaned = 
#   NYUR |>
#   janitor::clean_names() |>
#   separate(date, into = c("year", "month", "day"), sep = "-") |>
#   select(-day)
```

``` r
#incidents rate against the time
incidents_time = 
  NYPD_Shooting_Incident_cleaned |>
  mutate(hour = as.factor(hour)) |>
  group_by(hour) |>
  ggplot(aes(x = hour)) +
  geom_bar() +
  labs(x = "Time(hour)", y = "Incidents Numbers", title = "Incidents Distribution of Time Periods")
incidents_time
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#victim gender bar chart
victim_gender = 
  NYPD_Shooting_Incident_cleaned |>
  group_by(vic_sex) |>
  distinct(incident_key) |>
  summarise(count = n()) |>
  mutate(cumulative = cumsum(count),
         label_position = cumulative - (0.5 * count)) |>
  ggplot(aes(x = "", y = count, fill = vic_sex)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar("y", start = 0) +
  theme_void() +
  scale_fill_brewer(palette = "Dark2")
  
victim_gender 
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
#victim age bar chart
victim_age = 
  NYPD_Shooting_Incident_cleaned |>
  group_by(vic_age_group) |>
  distinct(incident_key) |>
  summarise(count = n()) |>
  mutate(cumulative = cumsum(count),
         label_position = cumulative - (0.5 * count)) |>
  ggplot(aes(x = "", y = count, fill = vic_age_group)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar("y", start = 0) +
  theme_void() +
  scale_fill_brewer(palette = "Dark2")
  
victim_age
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#incidents vs. year, seperated by borough
incidents_year = 
  NYPD_Shooting_Incident_cleaned |>
  group_by(year, boro) |>
  summarise(count = n(), .groups = "drop") |>
  ggplot(aes(x = year, y = count, group = boro, color = boro)) +
  geom_line() +
  facet_wrap(~boro) +
  labs(
    title = "Incidents vs Year",
    x = "Year",
    y = "Incidents"
  )
incidents_year
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
# map
new_york_map <- map_data("state", region = "new york")

# Define limits to focus on New York City
lon_min <- -74.3
lon_max <- -73.7
lat_min <- 40.5
lat_max <- 40.9

ggplot() +
  geom_polygon(data = new_york_map, aes(x = long, y = lat, group = group), fill = "lightgray", color = "white") +
  coord_fixed(ratio = 1, xlim = c(lon_min, lon_max), ylim = c(lat_min, lat_max)) +
  labs(title = "Map of New York City")
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
ggplot() +
  geom_polygon(data = new_york_map, aes(x = long, y = lat, group = group), fill = "lightgray", color = "white") +
  geom_point(data = NYPD_Shooting_Incident_cleaned, aes(x = longitude, y = latitude), color = "yellow") +
  coord_fixed(ratio = 1, xlim = c(lon_min, lon_max), ylim = c(lat_min, lat_max)) +
  labs(title = "Map of New York City with Data Points")
```

    ## Warning: Removed 51 rows containing missing values (`geom_point()`).

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

``` r
# map for Manhattan
manhattan_map <- map_data("state", region = "new york")

lon_min <- -74.0479
lon_max <- -73.79067
lat_min <- 40.6829
lat_max <- 40.8820

ggplot() +
  geom_polygon(data = manhattan_map, aes(x = long, y = lat, group = group), fill = "lightgray", color = "white") +
  coord_fixed(ratio = 1, xlim = c(lon_min, lon_max), ylim = c(lat_min, lat_max)) +
  labs(title = "Map of manhattan")
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

``` r
manhattan = 
  NYPD_Shooting_Incident_cleaned |>
  filter(boro == "MANHATTAN")

ggplot() +
  geom_polygon(data = manhattan_map, aes(x = long, y = lat, group = group), fill = "lightgray", color = "white") +
  geom_point(data = manhattan, aes(x = longitude, y = latitude), color = "yellow") +
  coord_fixed(ratio = 1, xlim = c(lon_min, lon_max), ylim = c(lat_min, lat_max)) +
  labs(title = "Map of Manhattan with Incident Points")
```

    ## Warning: Removed 13 rows containing missing values (`geom_point()`).

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-9-2.png)<!-- -->

Statistical Analysis: H0: There is no differences in mean incident
numbers between winter(Dec, Jan, Feb) and summer (June, July, Aug) H1:
the mean incident numbers between winter(Dec, Jan, Feb) and summer
(June, July, Aug) is different

``` r
season = 
  NYPD_Shooting_Incident_cleaned |>
  group_by(month) |>
  distinct(incident_key) |>
  summarise(count = n()) |>
  mutate(season = case_match(
    month,
    "01" ~ "winter",
    "02" ~ "winter",
    "03" ~ "spring",
    "04" ~ "spring",
    "05" ~ "spring",
    "06" ~ "summer",
    "07" ~ "summer",
    "08" ~ "summer",
    "09" ~ "fall",
    "10" ~ "fall",
    "11" ~ "fall",
    "12" ~ "winter",
  )) 

season |>
  ggplot(aes(x = season, y = count)) +
  geom_col()
```

![](Main-Analysis-Page_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

``` r
summer_winter = 
  NYPD_Shooting_Incident_cleaned |>
  group_by(month, year) |>
  distinct(incident_key) |>
  filter(!year == 2023) |>
  summarise(count = n()) |>
  mutate(season = case_match(
    month,
    "01" ~ "winter",
    "02" ~ "winter",
    "03" ~ "spring",
    "04" ~ "spring",
    "05" ~ "spring",
    "06" ~ "summer",
    "07" ~ "summer",
    "08" ~ "summer",
    "09" ~ "fall",
    "10" ~ "fall",
    "11" ~ "fall",
    "12" ~ "winter",
  )) |>
  select(-year) |>
  ungroup() |>
  filter(season %in% c("winter", "summer"))
```

    ## `summarise()` has grouped output by 'month'. You can override using the
    ## `.groups` argument.

``` r
summer = 
  summer_winter |>
  filter(season == "summer") |>
  select(count)
winter = 
  summer_winter |>
  filter(season == "winter") |>
  select(count)
summer_winter_after = 
  bind_cols(summer, winter) |>
  rename(summer = count...1,
         winter = count...2)
```

    ## New names:
    ## • `count` -> `count...1`
    ## • `count` -> `count...2`

``` r
z_test = z.test(x = summer_winter_after$summer, y = summer_winter_after$winter, sigma.x = sd(summer_winter_after$summer), sigma.y = sd(summer_winter_after$winter))
z_test
```

    ## 
    ##  Two-sample z-Test
    ## 
    ## data:  summer_winter_after$summer and summer_winter_after$winter
    ## z = 9.4566, p-value < 2.2e-16
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  48.79243 74.30561
    ## sample estimates:
    ## mean of x mean of y 
    ## 140.58824  79.03922

Proportion of male in \[manhattan\] = Proportion of male in \[queens\]

``` r
prop_df = 
  NYPD_Shooting_Incident_cleaned |>
  select(boro, vic_sex) |>
  group_by(boro, vic_sex) |>
  summarize(sum = n())
```

    ## `summarise()` has grouped output by 'boro'. You can override using the
    ## `.groups` argument.

``` r
num_brook = 
  prop_df |>
  filter(boro == "BROOKLYN")

num_SI = 
  prop_df |>
  filter(boro == "STATEN ISLAND")

num_brook = sum(pull(num_brook, sum))

num_SI = sum(pull(num_SI, sum))

num_brook_male =
  prop_df |>
  filter(boro == "BROOKLYN") |>
  filter(vic_sex == "M") |>
  pull(sum)

num_SI_male =
  prop_df |>
  filter(boro == "STATEN ISLAND") |>
  filter(vic_sex == "M") |>
  pull(sum)

prop.test(c(num_brook_male, num_SI_male), n = c(num_brook, num_SI))
```

    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(num_brook_male, num_SI_male) out of c(num_brook, num_SI)
    ## X-squared = 3.4568, df = 1, p-value = 0.06299
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.002741196  0.044648482
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.9024998 0.8815461
