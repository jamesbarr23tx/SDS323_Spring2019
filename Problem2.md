``` r
creatinine = read.csv('creatinine.csv')
```

\#Question1: What creatinine clearance rate should we expect, on
average, for a 55-year-old?

``` r
lm1 = lm(creatclear ~ age, data=creatinine)
predict(lm1,list(age=55))
```

    ##       1 
    ## 113.723

\#\#Answer: The expected Creatinine clearnace rate for a 55 year old is
113.7228.

\#Question2: How does creatinine clearance rate change with age? (This
should be a number with units ml/minute per year.)

``` r
coef(lm1)
```

    ## (Intercept)         age 
    ## 147.8129158  -0.6198159

\#\#Answer: On average,creatinine clearance rate decreases by
0.6198159ml/minute per year

\#Question3: Whose creatinine clearance rate is healthier (higher) for
their age: a 40-year-old with a rate of 135, or a 60-year-old with a
rate of 112?

``` r
predict(lm1,list(age=c(40,60)))
```

    ##        1        2 
    ## 123.0203 110.6240

``` r
#40 year old expected CCR is 123.0203
#60 year old expected CCR is 110.6240

#Formulas Below = Actual - Predicted
135-123.0203
```

    ## [1] 11.9797

``` r
#40 year old CCR is 11.9797mL/min healthier than average
112-110.6240
```

    ## [1] 1.376

``` r
#60 year old CCR is 1.376mL/min healthier than average
```

\#\#\#Answer: 40 year old has a healthier CCR for his age than the 60
year old
