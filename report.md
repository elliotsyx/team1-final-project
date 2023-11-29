report
================

## Motivation

The topic of shooting incidents worldwide raises concerns about the
safety and security of various communities. A specific case involving an
NYU student underscores the need for increased awareness and preventive
measures within educational institutions. Many schools are tasked with
ensuring the safety of their students, making it essential to address
and mitigate potential threats. Additionally, the safety and
satisfaction of tourists and fair labor conditions for employees are key
factors in maintaining the appeal of destinations like New York City.
People choosing NYC as their destination are often motivated by the
city’s diverse opportunities, which include education, employment, and
cultural experiences. Overall, the motivation to address these
interconnected issues lies in the shared goal of creating safer, more
vibrant communities and environments for individuals to thrive.

## Questions

## Data sources:

1.  Shooting Incident dataset We choose the NYPD Shooting Incident
    Dataset as our main dataset in our study. Shooting incident datasets
    contain every shooting incident that occurred in NYC with years
    ranging from 2006 to 2022. There are a total of 21 variables. Out of
    which, we consider the variables and description is listed below:
    INCIDENT_KEY: Randomly generated persistent ID for each arrest.
    OCCUR_DATE: Exact date of the shooting incident. OCCUR_TIME: Exact
    time of the shooting incident. BORO: Borough where the shooting
    incident occurred. PRECINCT: Precinct where the shooting incident
    occurred. JURISDICTION_CODE: Jurisdiction where the shooting
    incident occurred. Jurisdiction codes 0(Patrol), 1(Transit) and
    2(Housing) represent NYPD whilst codes 3 and more represent non NYPD
    jurisdictions. STATISTICAL_MURDER_FLAG: Shooting resulted in the
    victim’s death which would be counted as a murder. PERP_AGE_GROUP:
    Perpetrator’s age within a category. PERP_SEX: Perpetrator’s sex
    description. PERP_RACE: Perpetrator’s race description.
    VIC_AGE_GROUP: Victim’s age within a category. VIC_SEX: Victim’s sex
    description. VIC_RACE: Victim’s race description. X_COORD_CD:
    Midblock X-coordinate for New York State Plane Coordinate System,
    Long Island Zone, NAD 83, units feet (FIPS 3104). Y_COORD_CD:
    Midblock Y-coordinate for New York State Plane Coordinate System,
    Long Island Zone, NAD 83, units feet (FIPS 3104). Latitude: Latitude
    coordinate for Global Coordinate System, WGS 1984, decimal degrees
    (EPSG 4326). Longitude: Longitude coordinate for Global Coordinate
    System, WGS 1984, decimal degrees (EPSG 4326). Lon_Lat: Longitude
    and Latitude Coordinates for mapping.
2.  Unemployment Rate in New York ?

## Data cleaning

## plot

1.  Regarding time and victimization: Incidents are mainly concentrated
    at night, with fewer occurring during the day. Incidents occur in
    the lowest numbers from 7-9 pm, rising gradually as the day
    progresses and peaking at 23:00 pm.
2.  Regarding gender and victimization: Victimization has significant
    gender differences, the number of male victims is much higher than
    female.
3.  Regarding age and victimization: There is a disparity in
    victimization among different age groups. Victimization is
    concentrated among young people, especially the age between 25-44.
4.  ?Regarding region and victimization: How was the incident amount
    changed in different boroughs from 2006-2022? How cases are
    distributed in the New York City？

## Statistical Analysis
