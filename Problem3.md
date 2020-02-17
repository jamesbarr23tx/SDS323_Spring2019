\#Question: Is it worth it to make a building green?

\#\#Load greenbuilding files & packages needed

``` r
library(tidyverse)
```

    ## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.2     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(mosaic)
```

    ## Loading required package: lattice

    ## Loading required package: ggformula

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

    ## 
    ## Attaching package: 'Matrix'

    ## The following objects are masked from 'package:tidyr':
    ## 
    ##     expand, pack, unpack

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

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     count, do, tally

    ## The following object is masked from 'package:purrr':
    ## 
    ##     cross

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     stat

    ## The following objects are masked from 'package:stats':
    ## 
    ##     binom.test, cor, cor.test, cov, fivenum, IQR, median, prop.test,
    ##     quantile, sd, t.test, var

    ## The following objects are masked from 'package:base':
    ## 
    ##     max, mean, min, prod, range, sample, sum

``` r
library(knitr)
greenbuildings = read.csv('greenbuildings.csv')
```

\#\#Create Necessary data

``` r
greenbuildings$ERR = greenbuildings$Rent * greenbuildings$leasing_rate / 100
    # I made this column so that I could see how much money the building was actually making
greenbuildings$class_c = as.numeric(when(1, greenbuildings$class_a == 0 & greenbuildings$class_b == 0))
    # I made this ^ column to doublecheck the below column
greenbuildings$class_rtg = 
  ifelse(greenbuildings$class_a == 1, "Class 1" , ifelse(greenbuildings$class_b == 1, "Class 2","Class 3"))
    # I made this column so that I could group buildings by class
greenbuildings$is_green = ifelse(greenbuildings$green_rating == 1, 'Green', 'Not Green')
    # I made this as a work around because my boxplots were reading green_rating as a continuous variable
```

How does green rating affect different classes of buildings?
------------------------------------------------------------

``` r
class_mean = greenbuildings %>%
  group_by(class_rtg, green_rating) %>% 
  summarize(ERR = mean(ERR))
class_mean
```

    ## # A tibble: 6 x 3
    ## # Groups:   class_rtg [3]
    ##   class_rtg green_rating   ERR
    ##   <chr>            <int> <dbl>
    ## 1 Class 1              0  28.9
    ## 2 Class 1              1  28.1
    ## 3 Class 2              0  21.9
    ## 4 Class 2              1  22.4
    ## 5 Class 3              0  17.5
    ## 6 Class 3              1  25.2

### Bargraph of ERR & Class for Green vs. Regular

``` r
class_mean_bargraph = ggplot(data = class_mean, aes(x=class_rtg,y=ERR,fill=factor(green_rating))) +
  geom_bar(position='dodge', stat='identity') + 
  labs(x="Class Rating", y="Revenue per Sq.Ft.", fill = "Green Rating", title = "Impacts of Class & Green Rating on Revenue")
class_mean_bargraph
```

![](Problem3_files/figure-markdown_github/graph1-1.png)

``` r
###It's worth noting that there were exceedingly few data points for class 3 green buildings so this data should not be hevily considered
```

### Boxplot of ERR & Class for Green vs. Regular

``` r
class_green_boxplot = ggplot(data=greenbuildings) + 
  geom_boxplot(aes(x = is_green, y=ERR, group=is_green)) + 
  facet_wrap(~class_rtg) + 
  labs(x = "Green Rating", y = "Effective Rental Rate" , 
       title = "Effects of Building Class and Green Rating on Effective Rental Rate"
      )
class_green_boxplot
```

![](Problem3_files/figure-markdown_github/boxplot1-1.png)

``` r
###It's worth noting that there were exceedingly few data points for class 3 green buildings so this data should not be hevily considered
```

### Boxplot of ERR & Class for Green vs. Regular

``` r
class_green_violin = ggplot(data=greenbuildings) + 
  geom_violin(mapping = aes(x=is_green, y=ERR), fill = 'green') + 
  facet_wrap(~class_rtg) +
  labs(x = "Green Rating", y = "Effective Rental Rate" , 
       title = "Effects of Building Class and Green Rating on Effective Rental Rate")
class_green_violin
```

![](Problem3_files/figure-markdown_github/violinplot1-1.png)

``` r
### There are only seven class 3 green buildings
```

### Breakdown of percentage of buildings that fall into each category

``` r
t1 = xtabs(~ is_green + class_rtg, data = greenbuildings)
t1 %>%
  prop.table(margin=2) %>%
  round(3)%>%
  kable
```

|           |  Class 1|  Class 2|  Class 3|
|-----------|--------:|--------:|--------:|
| Green     |    0.173|    0.036|    0.006|
| Not Green |    0.827|    0.964|    0.994|

\#Answer:

``` r
#The excel guru never accounted for the fact that green buildings are disproportionately likely to be Class 1, therefore, it would be incorrect for him to attribute the higher rental rates to the fact that the building is green. It would be more correct to say that Class 1 buildings generally rent for higher rates than Class 2 & 3 buildings, and Class 1 buildings are more likely to be green than any other class
```
