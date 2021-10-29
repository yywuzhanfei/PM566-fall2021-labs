hw1
================
Mingxi Xu
9/21/2021

### 1. Given the formulated question from the assignment description, you will now conduct EDA Checklist items 2-4. First, download 2004 and 2019 data for all sites in California from the EPA Air Quality Data website. Read in the data using data.table(). For each of the two datasets, check the dimensions, headers, footers, variable names and variable types. Check for any data issues, particularly in the key variable we are analyzing. Make sure you write up a summary of all of your findings.

#### create two variables

``` r
library(tidyverse)
```

    ## ─ Attaching packages ──────────────────── tidyverse 1.3.1 ─

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.3     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ─ Conflicts ───────────────────── tidyverse_conflicts() ─
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
data_2014 <- read.csv("2014.csv")
data_2019 <- read.csv("2019.csv")
```

#### check the dimensions, headers, footers, variable names and variable types.

``` r
dim(data_2014)
```

    ## [1] 19233    20

``` r
head(data_2014)
```

    ##         Date Source  Site.ID POC Daily.Mean.PM2.5.Concentration    UNITS
    ## 1 01/01/2004    AQS 60010007   1                           11.0 ug/m3 LC
    ## 2 01/02/2004    AQS 60010007   1                           12.2 ug/m3 LC
    ## 3 01/03/2004    AQS 60010007   1                           16.5 ug/m3 LC
    ## 4 01/04/2004    AQS 60010007   1                           19.5 ug/m3 LC
    ## 5 01/05/2004    AQS 60010007   1                           11.5 ug/m3 LC
    ## 6 01/06/2004    AQS 60010007   1                           32.5 ug/m3 LC
    ##   DAILY_AQI_VALUE Site.Name DAILY_OBS_COUNT PERCENT_COMPLETE AQS_PARAMETER_CODE
    ## 1              46 Livermore               1              100              88502
    ## 2              51 Livermore               1              100              88502
    ## 3              60 Livermore               1              100              88502
    ## 4              67 Livermore               1              100              88502
    ## 5              48 Livermore               1              100              88502
    ## 6              94 Livermore               1              100              88502
    ##                       AQS_PARAMETER_DESC CBSA_CODE
    ## 1 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 2 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 3 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 4 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 5 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 6 Acceptable PM2.5 AQI & Speciation Mass     41860
    ##                           CBSA_NAME STATE_CODE      STATE COUNTY_CODE  COUNTY
    ## 1 San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 2 San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 3 San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 4 San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 5 San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 6 San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ##   SITE_LATITUDE SITE_LONGITUDE
    ## 1      37.68753      -121.7842
    ## 2      37.68753      -121.7842
    ## 3      37.68753      -121.7842
    ## 4      37.68753      -121.7842
    ## 5      37.68753      -121.7842
    ## 6      37.68753      -121.7842

``` r
tail(data_2014)
```

    ##             Date Source  Site.ID POC Daily.Mean.PM2.5.Concentration    UNITS
    ## 19228 12/14/2004    AQS 61131003   1                             11 ug/m3 LC
    ## 19229 12/17/2004    AQS 61131003   1                             16 ug/m3 LC
    ## 19230 12/20/2004    AQS 61131003   1                             17 ug/m3 LC
    ## 19231 12/23/2004    AQS 61131003   1                              9 ug/m3 LC
    ## 19232 12/26/2004    AQS 61131003   1                             24 ug/m3 LC
    ## 19233 12/29/2004    AQS 61131003   1                              9 ug/m3 LC
    ##       DAILY_AQI_VALUE            Site.Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ## 19228              46 Woodland-Gibson Road               1              100
    ## 19229              59 Woodland-Gibson Road               1              100
    ## 19230              61 Woodland-Gibson Road               1              100
    ## 19231              38 Woodland-Gibson Road               1              100
    ## 19232              76 Woodland-Gibson Road               1              100
    ## 19233              38 Woodland-Gibson Road               1              100
    ##       AQS_PARAMETER_CODE       AQS_PARAMETER_DESC CBSA_CODE
    ## 19228              88101 PM2.5 - Local Conditions     40900
    ## 19229              88101 PM2.5 - Local Conditions     40900
    ## 19230              88101 PM2.5 - Local Conditions     40900
    ## 19231              88101 PM2.5 - Local Conditions     40900
    ## 19232              88101 PM2.5 - Local Conditions     40900
    ## 19233              88101 PM2.5 - Local Conditions     40900
    ##                                     CBSA_NAME STATE_CODE      STATE COUNTY_CODE
    ## 19228 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 19229 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 19230 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 19231 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 19232 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 19233 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ##       COUNTY SITE_LATITUDE SITE_LONGITUDE
    ## 19228   Yolo      38.66121      -121.7327
    ## 19229   Yolo      38.66121      -121.7327
    ## 19230   Yolo      38.66121      -121.7327
    ## 19231   Yolo      38.66121      -121.7327
    ## 19232   Yolo      38.66121      -121.7327
    ## 19233   Yolo      38.66121      -121.7327

``` r
str(data_2014)
```

    ## 'data.frame':    19233 obs. of  20 variables:
    ##  $ Date                          : chr  "01/01/2004" "01/02/2004" "01/03/2004" "01/04/2004" ...
    ##  $ Source                        : chr  "AQS" "AQS" "AQS" "AQS" ...
    ##  $ Site.ID                       : int  60010007 60010007 60010007 60010007 60010007 60010007 60010007 60010007 60010007 60010007 ...
    ##  $ POC                           : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Daily.Mean.PM2.5.Concentration: num  11 12.2 16.5 19.5 11.5 32.5 15.5 29.9 21 15.7 ...
    ##  $ UNITS                         : chr  "ug/m3 LC" "ug/m3 LC" "ug/m3 LC" "ug/m3 LC" ...
    ##  $ DAILY_AQI_VALUE               : int  46 51 60 67 48 94 58 88 70 59 ...
    ##  $ Site.Name                     : chr  "Livermore" "Livermore" "Livermore" "Livermore" ...
    ##  $ DAILY_OBS_COUNT               : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ PERCENT_COMPLETE              : num  100 100 100 100 100 100 100 100 100 100 ...
    ##  $ AQS_PARAMETER_CODE            : int  88502 88502 88502 88502 88502 88502 88502 88502 88502 88101 ...
    ##  $ AQS_PARAMETER_DESC            : chr  "Acceptable PM2.5 AQI & Speciation Mass" "Acceptable PM2.5 AQI & Speciation Mass" "Acceptable PM2.5 AQI & Speciation Mass" "Acceptable PM2.5 AQI & Speciation Mass" ...
    ##  $ CBSA_CODE                     : int  41860 41860 41860 41860 41860 41860 41860 41860 41860 41860 ...
    ##  $ CBSA_NAME                     : chr  "San Francisco-Oakland-Hayward, CA" "San Francisco-Oakland-Hayward, CA" "San Francisco-Oakland-Hayward, CA" "San Francisco-Oakland-Hayward, CA" ...
    ##  $ STATE_CODE                    : int  6 6 6 6 6 6 6 6 6 6 ...
    ##  $ STATE                         : chr  "California" "California" "California" "California" ...
    ##  $ COUNTY_CODE                   : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ COUNTY                        : chr  "Alameda" "Alameda" "Alameda" "Alameda" ...
    ##  $ SITE_LATITUDE                 : num  37.7 37.7 37.7 37.7 37.7 ...
    ##  $ SITE_LONGITUDE                : num  -122 -122 -122 -122 -122 ...

``` r
dim(data_2019)
```

    ## [1] 53086    20

``` r
head(data_2019)
```

    ##         Date Source  Site.ID POC Daily.Mean.PM2.5.Concentration    UNITS
    ## 1 01/01/2019    AQS 60010007   3                            5.7 ug/m3 LC
    ## 2 01/02/2019    AQS 60010007   3                           11.9 ug/m3 LC
    ## 3 01/03/2019    AQS 60010007   3                           20.1 ug/m3 LC
    ## 4 01/04/2019    AQS 60010007   3                           28.8 ug/m3 LC
    ## 5 01/05/2019    AQS 60010007   3                           11.2 ug/m3 LC
    ## 6 01/06/2019    AQS 60010007   3                            2.7 ug/m3 LC
    ##   DAILY_AQI_VALUE Site.Name DAILY_OBS_COUNT PERCENT_COMPLETE AQS_PARAMETER_CODE
    ## 1              24 Livermore               1              100              88101
    ## 2              50 Livermore               1              100              88101
    ## 3              68 Livermore               1              100              88101
    ## 4              86 Livermore               1              100              88101
    ## 5              47 Livermore               1              100              88101
    ## 6              11 Livermore               1              100              88101
    ##         AQS_PARAMETER_DESC CBSA_CODE                         CBSA_NAME
    ## 1 PM2.5 - Local Conditions     41860 San Francisco-Oakland-Hayward, CA
    ## 2 PM2.5 - Local Conditions     41860 San Francisco-Oakland-Hayward, CA
    ## 3 PM2.5 - Local Conditions     41860 San Francisco-Oakland-Hayward, CA
    ## 4 PM2.5 - Local Conditions     41860 San Francisco-Oakland-Hayward, CA
    ## 5 PM2.5 - Local Conditions     41860 San Francisco-Oakland-Hayward, CA
    ## 6 PM2.5 - Local Conditions     41860 San Francisco-Oakland-Hayward, CA
    ##   STATE_CODE      STATE COUNTY_CODE  COUNTY SITE_LATITUDE SITE_LONGITUDE
    ## 1          6 California           1 Alameda      37.68753      -121.7842
    ## 2          6 California           1 Alameda      37.68753      -121.7842
    ## 3          6 California           1 Alameda      37.68753      -121.7842
    ## 4          6 California           1 Alameda      37.68753      -121.7842
    ## 5          6 California           1 Alameda      37.68753      -121.7842
    ## 6          6 California           1 Alameda      37.68753      -121.7842

``` r
tail(data_2019)
```

    ##             Date Source  Site.ID POC Daily.Mean.PM2.5.Concentration    UNITS
    ## 53081 11/11/2019    AQS 61131003   1                           13.5 ug/m3 LC
    ## 53082 11/17/2019    AQS 61131003   1                           18.1 ug/m3 LC
    ## 53083 11/29/2019    AQS 61131003   1                           12.5 ug/m3 LC
    ## 53084 12/17/2019    AQS 61131003   1                           23.8 ug/m3 LC
    ## 53085 12/23/2019    AQS 61131003   1                            1.0 ug/m3 LC
    ## 53086 12/29/2019    AQS 61131003   1                            9.1 ug/m3 LC
    ##       DAILY_AQI_VALUE            Site.Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ## 53081              54 Woodland-Gibson Road               1              100
    ## 53082              64 Woodland-Gibson Road               1              100
    ## 53083              52 Woodland-Gibson Road               1              100
    ## 53084              76 Woodland-Gibson Road               1              100
    ## 53085               4 Woodland-Gibson Road               1              100
    ## 53086              38 Woodland-Gibson Road               1              100
    ##       AQS_PARAMETER_CODE       AQS_PARAMETER_DESC CBSA_CODE
    ## 53081              88101 PM2.5 - Local Conditions     40900
    ## 53082              88101 PM2.5 - Local Conditions     40900
    ## 53083              88101 PM2.5 - Local Conditions     40900
    ## 53084              88101 PM2.5 - Local Conditions     40900
    ## 53085              88101 PM2.5 - Local Conditions     40900
    ## 53086              88101 PM2.5 - Local Conditions     40900
    ##                                     CBSA_NAME STATE_CODE      STATE COUNTY_CODE
    ## 53081 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 53082 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 53083 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 53084 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 53085 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 53086 Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ##       COUNTY SITE_LATITUDE SITE_LONGITUDE
    ## 53081   Yolo      38.66121      -121.7327
    ## 53082   Yolo      38.66121      -121.7327
    ## 53083   Yolo      38.66121      -121.7327
    ## 53084   Yolo      38.66121      -121.7327
    ## 53085   Yolo      38.66121      -121.7327
    ## 53086   Yolo      38.66121      -121.7327

``` r
str(data_2019)
```

    ## 'data.frame':    53086 obs. of  20 variables:
    ##  $ Date                          : chr  "01/01/2019" "01/02/2019" "01/03/2019" "01/04/2019" ...
    ##  $ Source                        : chr  "AQS" "AQS" "AQS" "AQS" ...
    ##  $ Site.ID                       : int  60010007 60010007 60010007 60010007 60010007 60010007 60010007 60010007 60010007 60010007 ...
    ##  $ POC                           : int  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ Daily.Mean.PM2.5.Concentration: num  5.7 11.9 20.1 28.8 11.2 2.7 2.8 7 3.1 7.1 ...
    ##  $ UNITS                         : chr  "ug/m3 LC" "ug/m3 LC" "ug/m3 LC" "ug/m3 LC" ...
    ##  $ DAILY_AQI_VALUE               : int  24 50 68 86 47 11 12 29 13 30 ...
    ##  $ Site.Name                     : chr  "Livermore" "Livermore" "Livermore" "Livermore" ...
    ##  $ DAILY_OBS_COUNT               : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ PERCENT_COMPLETE              : num  100 100 100 100 100 100 100 100 100 100 ...
    ##  $ AQS_PARAMETER_CODE            : int  88101 88101 88101 88101 88101 88101 88101 88101 88101 88101 ...
    ##  $ AQS_PARAMETER_DESC            : chr  "PM2.5 - Local Conditions" "PM2.5 - Local Conditions" "PM2.5 - Local Conditions" "PM2.5 - Local Conditions" ...
    ##  $ CBSA_CODE                     : int  41860 41860 41860 41860 41860 41860 41860 41860 41860 41860 ...
    ##  $ CBSA_NAME                     : chr  "San Francisco-Oakland-Hayward, CA" "San Francisco-Oakland-Hayward, CA" "San Francisco-Oakland-Hayward, CA" "San Francisco-Oakland-Hayward, CA" ...
    ##  $ STATE_CODE                    : int  6 6 6 6 6 6 6 6 6 6 ...
    ##  $ STATE                         : chr  "California" "California" "California" "California" ...
    ##  $ COUNTY_CODE                   : int  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ COUNTY                        : chr  "Alameda" "Alameda" "Alameda" "Alameda" ...
    ##  $ SITE_LATITUDE                 : num  37.7 37.7 37.7 37.7 37.7 ...
    ##  $ SITE_LONGITUDE                : num  -122 -122 -122 -122 -122 ...
