``` r
milk = read.csv('milk.csv')
```

``` r
#Create new variables
milk$revenue = milk$sales * milk$price
##Check to see if a basic model would do any good
plot(milk$price, milk$revenue)
```

![](Problem4_files/figure-markdown_github/testrev-1.png)

``` r
###clearly not linear relationship
```

``` r
#Try logarithmic model
## Create new variables
logsales = log(milk$sales)
logprice = log(milk$price)
###Check to make sure i did it right
str(milk)
```

    ## 'data.frame':    116 obs. of  3 variables:
    ##  $ price  : num  3.48 3.12 2.95 2.68 3.62 4.32 2.42 3.81 2.46 3.34 ...
    ##  $ sales  : int  15 11 21 30 11 11 25 13 20 11 ...
    ##  $ revenue: num  52.2 34.3 62 80.4 39.8 ...

``` r
#Create logarithmic model
demandcurve = lm(logsales ~ logprice, data = milk)
## Check to see if the model is any good
summary(demandcurve)
```

    ## 
    ## Call:
    ## lm(formula = logsales ~ logprice, data = milk)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.65425 -0.18405 -0.01262  0.17986  0.65074 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  4.72060    0.09172   51.47   <2e-16 ***
    ## logprice    -1.61858    0.08116  -19.94   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.2687 on 114 degrees of freedom
    ## Multiple R-squared:  0.7772, Adjusted R-squared:  0.7753 
    ## F-statistic: 397.7 on 1 and 114 DF,  p-value: < 2.2e-16

``` r
plot(demandcurve,which = 1)
```

![](Problem4_files/figure-markdown_github/logmodel-1.png)

``` r
plot(demandcurve,which = 2)
```

![](Problem4_files/figure-markdown_github/logmodel-2.png)

``` r
### Log model works pretty well
```

``` r
# profit = revenue - C,
## = sales*price - C
### = e^-1.61858 * price - C
####I'm stuck

"Well, frankly I don't know what to do. Hopefully I can get partial creadit, but I can't figure out how to put this equation in terms of C"
```

    ## [1] "Well, frankly I don't know what to do. Hopefully I can get partial creadit, but I can't figure out how to put this equation in terms of C"
