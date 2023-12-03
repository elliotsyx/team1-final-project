report
================

## Motivation

The topic of shooting incidents worldwide raises concerns about the
safety and security of various communities. On the afternoon of Nov. 6,
EST, NYU Alert, the alert system of New York University, issued an
emergency alert that a shooting had occurred in the Tandon neighborhood,
advising people to “run if you can run, hide if you can’t run, and duel
if you can’t hide.” This specific case involving an NYU student
underscores the need for increased awareness and preventive measures
within educational institutions. Many schools are tasked with ensuring
the safety of their students, making it essential to address and
mitigate potential threats. Additionally, the safety and satisfaction of
tourists and fair labor conditions for employees are key factors in
maintaining the appeal of destinations like New York City. People
choosing NYC as their destination are often motivated by the city’s
diverse opportunities, which include education, employment, and cultural
experiences. Overall, the motivation to address these interconnected
issues lies in the shared goal of creating safer, more vibrant
communities and environments for individuals to thrive.

## Initial Questions ?

1.  Are shootings affected by seasonal differences?
2.  Do male victimization rates vary by region? 3.？

## Data sources:

1.  Shooting Incident from 2006 to 2022 dataset We choose the NYPD
    Shooting Incident Dataset as our main dataset in our study. This
    shooting incident datasets contain every shooting incident that
    occurred in NYC with years ranging from 2006 to 2022. Each record
    represents a shooting incident in NYC and includes information about
    the event, the location and time of occurrence. In addition,
    information related to suspect and victim demographics is also
    included. The code is read as following:

There are a total of 21 variables. Out of which, we consider the
variables and description is listed below: INCIDENT_KEY: Randomly
generated persistent ID for each arrest. OCCUR_DATE: Exact date of the
shooting incident. OCCUR_TIME: Exact time of the shooting incident.
BORO: Borough where the shooting incident occurred. PRECINCT: Precinct
where the shooting incident occurred. JURISDICTION_CODE: Jurisdiction
where the shooting incident occurred. Jurisdiction codes 0(Patrol),
1(Transit) and 2(Housing) represent NYPD whilst codes 3 and more
represent non NYPD jurisdictions. PERP_AGE_GROUP: Perpetrator’s age
within a category. PERP_SEX: Perpetrator’s sex description. PERP_RACE:
Perpetrator’s race description. VIC_AGE_GROUP: Victim’s age within a
category. VIC_SEX: Victim’s sex description. VIC_RACE: Victim’s race
description. X_COORD_CD: Midblock X-coordinate for New York State Plane
Coordinate System, Long Island Zone, NAD 83, units feet (FIPS 3104).
Y_COORD_CD: Midblock Y-coordinate for New York State Plane Coordinate
System, Long Island Zone, NAD 83, units feet (FIPS 3104). Latitude:
Latitude coordinate for Global Coordinate System, WGS 1984, decimal
degrees (EPSG 4326). Longitude: Longitude coordinate for Global
Coordinate System, WGS 1984, decimal degrees (EPSG 4326).

2.  Shooting incident data for 2023 This dataset list of every shooting
    incident that occurred in NYC during the current calendar year. As a
    supplement to the primary dataset, this dataset added 2023 shooting
    incidents up to November.Each record represents a shooting incident
    in NYC and includes information about the event, the location and
    time of occurrence. In addition, information related to suspect and
    victim demographics is also included. The data is read in the
    following code:

There are a total of 21 variables. Out of which, the variables and
description we considered are same as the given list above. 3.
Unemployment Rate in New York ?

## Data processing

1.  Merge the two datasets We used the `bind_rows` to bind these two
    dataste by row, making a longer result for the whole shooting
    incidents happened from 2006 to 2023.
2.  Adjustment of data format Separate the year, month and day in
    `occur_date` into three variables. Also, Separate the hours,
    minutes, and seconds in “occup_time” into three variables for
    subsequent statistical analysis.
3.  Data cleansing Remove variables that will not be used in subsequent
    statistical analysis of the data: `minute`, `second`,
    `loc_of_occur_desc`, `loc_classfctn_desc`, `location_desc`. The
    dataset is cleaned and merged in the following code.

## plot

1.  Regarding time and victimization: Presents an analysis of the
    distribution of NYPD shooting incidents across different time
    periods. The focus is on understanding when these incidents are more
    likely to occur during the day. The analysis utilizes the
    `NYPD_Shooting_Incident_cleaned` dataset. Use the `mutate` to create
    a variable `hour` as a factor. Then group the dataset by the `hour`
    variable. Use the `ggplot` to create a bar plot illustrating the
    distribution of incidents across different hours. The plot is making
    by the following code:

Incidents are mainly concentrated at night, with fewer occurring during
the day. Incidents occur in the lowest numbers from 7-9 pm, rising
gradually as the day progresses and peaking at 23:00 pm.

2.  Regarding gender and victimization: Begin the analysisby grouping
    the `NYPD_Shooting_Incident_cleaned` dataset by victim gender
    (`vic_sex`). Use the `distinct` function to identify unique
    incidents based on the `incident_key`. Subsequently, the `summarise`
    function calculates the count of unique incidents for each gender.
    Then use the `cumsum` function to calculate the cumulative count,
    and additional variables are created to determine label positions
    for better visualization. Finally, use the `ggplot` function to
    create a polar bar plot representing the distribution of shooting
    incidents by victim gender.The plot is making by the following code:

Victimization has significant gender differences, the number of male
victims is much higher than female.

3.  Regarding age and victimization: The analysis begins by grouping the
    dataset by the variable `vic_age_group`, which represents victim age
    groups. The `distinct` function is then applied to obtain unique
    incident keys for each age group. Then use the `summarise` function
    to calculates the count of distinct incidents for each age group.
    The cumulative count and label positions are calculated using the
    `mutate` function to facilitate the creation of a stacked bar chart.
    Finally, use the `ggplot` function to create a polar bar chart,
    providing a visual representation of the distribution of shooting
    incidents across different victim age groups. The code is as
    following:

There is a disparity in victimization among different age groups.
Victimization is concentrated among young people, especially the age
between 25-44.

4.  Regarding region and victimization: The dataset is grouped by both
    year and borough, summarizing the number of incidents for each
    combination. The resulting data is visualized through a line plot
    created with the ggplot2 package, displaying incidents over the
    years with separate lines for each borough. Additionally, the data
    is faceted by borough for a more detailed view. Code:

The number of incidents in Bronx and Brooklyn has been maintained at a
high level every year. The overall situation of Manhattan and Queens
remains relatively low and stable. But all four borough showed
significant declines in 2018-2019, and bounced back in the following
year. Compared with other boroughs, Staten Island have the fewer amount
of the incidents. The density of data points in Staten Island is
significantly lower than in other regions.

## Mapping

1.Mapping New York City To provide a more detailed and focused view of
New York City, specific geographic limits were defined. The longitude
and latitude boundaries were set as follows: Longitude: -74.3 to -73.7;
Latitude: 40.5 to 40.9. These limits were chosen to encompass the
central area of New York City, ensuring that the map primarily
highlights the city itself. Use the `geom_polygon` function to plot the
geographical polygons, with each polygon representing a different region
within the state of New York. Code:

The maps generated are called “New York City Maps” and effectively show
the outlines of geographic areas with light gray fills and white
borders.

2.  Mapping New York City with NYPD Shooting Incident Data Use
    `map_data` to obtain geographic data for the state of New York,
    focusing on the New York region.Use the `new_york_map` as base map
    data. The NYPD Shooting Incident dataset (
    `NYPD_Shooting_Incident_cleaned`) was used to identify and plot the
    location of specific incidents on a map. The latitude and longitude
    coordinates in this dataset are used to map the shooting incident
    data. The `geom_polygon` function is used to draw a geographic
    polygon of New York City with a light gray fill and white border.
    Use the `geom_point` function to overlay yellow data points
    representing the location of the shooting event. Code:

The map that resulted, entitled “Map of New York City with Data Points,”
effective combines the geographic layout of the city with the specific
locations of the shootings. The light gray background provides context
for the map, while the yellow data points highlight the spatial
distribution of shootings in New York City.

3.  Mapping Manhattan In order to provide a detailed and centralized
    view of Manhattan, the map defines specific geographic constraints.
    The longitude and latitude boundaries are set as follows: Longitude:
    -74.0479 to -73.79067; Latitude: 40.6829 to 40.8820. These
    boundaries cover all of Manhattan, ensuring that the map primarily
    highlights the borough itself. Use the `geom_polygon` function to
    draw a geographic polygon of Manhattan with a light gray fill and
    white border. Code:

Generates a “Manhattan Map” that effectively shows the outline of the
geographic area with a light gray fill and white border.

4.Mapping Manhattan with NYPD Shooting Incident Data Use the
`geom_polygon` function to draw a geographic polygon of Manhattan with a
light gray fill and white border. In addition, yellow data points
representing the locations of shootings within Manhattan were overlaid
using the `geom_point` function. Code:

“Manhattan Map with Event Points” effectively combines the geographic
layout of Manhattan with the specific locations of shooting incidents. A
light gray background provides context for the map, while yellow data
points highlight the spatial distribution of shootings within the
borough.

## Statistical Analysis

1.  Seasonal Comparison of Shooting Incidents This statistical analysis
    aims to investigate whether there are significant differences in the
    mean number of shooting incidents between the winter months
    (December, January, February) and the summer months (June, July,
    August) in New York City.

- Generated dataset `season`, visualized using bar graphs. x-axis
  represents the season (winter, spring, summer, fall), and y-axis
  represents the number of different shootings in each season. The bar
  plot provides a visual representation of the distribution of shooting
  incidents across different seasons. The largest gaps were found in the
  summer and winter seasons, which were selected for subsequent
  analysis.
- Summer-Winter Incident Comparison. Null Hypothesis (H0): There is no
  difference in mean incident numbers between summer and winter.
  Alternative Hypothesis (H1): The mean incident numbers between summer
  and winter are different. A two-sample z-test was conducted to compare
  the average of summer and winter shooting incidents. z-test yielded
  the following results: Z value: 9.4566 P-value: \< 2.2e-16 (very
  small, indicating strong evidence against the original hypothesis)
  Confidence interval: (48.79, 74.31) The results of the z-test for both
  samples indicate that there is a statistically significant difference
  between the average number of shootings in the summer and winter.
  p-value is extremely small, providing strong evidence against the
  original hypothesis. Means that there is a large difference between
  the average number of shootings in the summer and winter months.

2.  Proportion of Male Shooting Victims in Brooklyn vs. Staten Island.
    This statistical analysis aims to compare the proportion of male
    shooting victims between the boroughs of Manhattan and Queens in New
    York City. The goal is to investigate whether there is a significant
    difference in the proportion of male victims between these two
    boroughs. Null Hypothesis (H0): The proportion of male shooting
    victims in Brooklyn is equal to the proportion in Staten Island.
    Alternative Hypothesis (H1): The proportions of male shooting
    victims in Brooklyn and Staten Island are different. Use a
    two-sample test for equality of proportions with continuity
    correction to compare the proportions of male shooting victims in
    Brooklyn and Staten Island. And the test results indicate the
    following: X-squared Value: 3.4568 Degrees of Freedom (df): 1
    P-Value: 0.06299 Confidence Interval: (-0.0027, 0.0446) While the
    p-value is greater than the conventional significance level of 0.05,
    suggesting that we do not have strong evidence against the null
    hypothesis.
