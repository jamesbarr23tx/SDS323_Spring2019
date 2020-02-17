ABIA Data: Problem 1
====================

Question: Where did the most flights come from during SXSW & how does that differ from a usual week?
----------------------------------------------------------------------------------------------------

### Reading in the data & necessary packages

    ## Loading required package: dplyr

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    ## Loading required package: lattice

    ## Loading required package: ggformula

    ## Loading required package: ggplot2

    ## Loading required package: ggstance

    ## 
    ## Attaching package: 'ggstance'

    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     geom_errorbarh, GeomErrorbarh

    ## 
    ## New to ggformula?  Try the tutorials: 
    ##  learnr::run_tutorial("introduction", package = "ggformula")
    ##  learnr::run_tutorial("refining", package = "ggformula")

    ## Loading required package: mosaicData

    ## Loading required package: Matrix

    ## Registered S3 method overwritten by 'mosaic':
    ##   method                           from   
    ##   fortify.SpatialPolygonsDataFrame ggplot2

    ## 
    ## The 'mosaic' package masks several functions from core packages in order to add 
    ## additional features.  The original behavior of these functions should not be affected by this.
    ## 
    ## Note: If you use the Matrix package, be sure to load it BEFORE loading mosaic.

    ## 
    ## Attaching package: 'mosaic'

    ## The following object is masked from 'package:Matrix':
    ## 
    ##     mean

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     stat

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     count, do, tally

    ## The following objects are masked from 'package:stats':
    ## 
    ##     binom.test, cor, cor.test, cov, fivenum, IQR, median, prop.test,
    ##     quantile, sd, t.test, var

    ## The following objects are masked from 'package:base':
    ## 
    ##     max, mean, min, prod, range, sample, sum

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✔ tibble  2.1.3     ✔ purrr   0.3.3
    ## ✔ tidyr   1.0.2     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ mosaic::count()            masks dplyr::count()
    ## ✖ purrr::cross()             masks mosaic::cross()
    ## ✖ mosaic::do()               masks dplyr::do()
    ## ✖ tidyr::expand()            masks Matrix::expand()
    ## ✖ dplyr::filter()            masks stats::filter()
    ## ✖ ggstance::geom_errorbarh() masks ggplot2::geom_errorbarh()
    ## ✖ dplyr::lag()               masks stats::lag()
    ## ✖ tidyr::pack()              masks Matrix::pack()
    ## ✖ mosaic::stat()             masks ggplot2::stat()
    ## ✖ mosaic::tally()            masks dplyr::tally()
    ## ✖ tidyr::unpack()            masks Matrix::unpack()

### Creating New Variables

    ABIA$Date <- as.Date(with(ABIA, paste(Year, Month, DayofMonth,sep="-")), "%Y-%m-%d")
    ABIA$Week = strftime(ABIA$Date, format = "%V")

### Filter data down to flights coming to Austin

    ToAus = filter(ABIA, Dest == "AUS")
    SXSWFlightMonth = filter(ToAus, ToAus$Month == 3)
    SXSWFlight = filter(SXSWFlightMonth, between(SXSWFlightMonth$DayofMonth,10,16))

### Filter data to just flights to Austin in Spring

    Spring_list = c('07','08','09', '10','11','12','13','14','15')
    Spring = ToAus %>%
      filter(Week %in% Spring_list) 
    SXSWWeek = ToAus %>%
      filter(Week == '11')

\#\#\#How many flights came to Austin each week in Spring

    nrow(Spring)/9

    ## [1] 998.4444

### Create filtered data set for only flights to Austin in SXSW week

    SXSWWeek = ToAus %>%
      filter(Week == '11')
    nrow(SXSWWeek)

    ## [1] 1013

### Plot of how many flights come in each week to Austin in Spring

    Spring %>%
      mutate(SXSWWeek = ifelse(Week == 11, T,F)) %>%
    ggplot(aes(x=Week)) +
      geom_bar(aes(fill = SXSWWeek))+
      labs(x = 'Week', y = 'Number of Flights', title = 'Flights per Week to ABIA in Spring 2008')

![](Problem1_files/figure-markdown_strict/Longitude-1.png) \#\#\# Plot
of where flights come from throughout spring

    SpringOriginBarPlot = Spring %>%
      ggplot(aes(x=Origin)) +
      geom_bar()+
      coord_flip() +
        labs(x = 'Origin', y = 'Number of Flights', title = 'Origins of flights to ABIA in Spring 2008')

    SpringOriginBarPlot

![](Problem1_files/figure-markdown_strict/SpringOrigin-1.png) \#\#\#
Plot of where flights come from throughout SXSW
![](Problem1_files/figure-markdown_strict/SXSWOrigin-1.png)
\#\#Conclusion

    'There is not a huge difference between SXSW and a normal Spring week in terms of how many flights are coming in and where the flights are coming from.
    In both periods, Dallas sent by far the most flights to Austin compared to any other city.'

    ## [1] "There is not a huge difference between SXSW and a normal Spring week in terms of how many flights are coming in and where the flights are coming from.\nIn both periods, Dallas sent by far the most flights to Austin compared to any other city."
