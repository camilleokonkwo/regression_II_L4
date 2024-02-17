HW4
================
Camille Okonkwo
2024-02-17

### reading in data

``` r
hwdata1 = read_csv("data/hwdata1.csv")
```

    ## Rows: 686 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): diagdate, recdate
    ## dbl (11): id, age, menopause, hormone, size, grade, nodes, prog_recp, estrg_...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hwdata1)
```

    ## # A tibble: 6 × 13
    ##      id diagdate  recdate     age menopause hormone  size grade nodes prog_recp
    ##   <dbl> <chr>     <chr>     <dbl>     <dbl>   <dbl> <dbl> <dbl> <dbl>     <dbl>
    ## 1     1 17aug1984 15apr1988    38         1       1    18     3     5       141
    ## 2     2 25apr1985 15mar1989    52         1       1    20     1     1        78
    ## 3     3 11oct1984 12apr1988    47         1       1    30     2     1       422
    ## 4     4 29jun1984 24nov1984    40         1       1    24     1     3        25
    ## 5     5 03jul1984 09aug1989    64         2       2    19     2     1        19
    ## 6     6 24jul1984 08nov1989    49         2       2    56     1     3       356
    ## # ℹ 3 more variables: estrg_recp <dbl>, rectime <dbl>, censrec <dbl>

# Cox model

``` r
library(survival)

hwdata1$hormone <- as.factor(hwdata1$hormone)

fit = coxph(Surv(rectime,censrec) ~ hormone,
            data = hwdata1,
            ties = "efron")

summary(fit)
```

    ## Call:
    ## coxph(formula = Surv(rectime, censrec) ~ hormone, data = hwdata1, 
    ##     ties = "efron")
    ## 
    ##   n= 686, number of events= 299 
    ## 
    ##             coef exp(coef) se(coef)      z Pr(>|z|)   
    ## hormone2 -0.3640    0.6949   0.1250 -2.911   0.0036 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ##          exp(coef) exp(-coef) lower .95 upper .95
    ## hormone2    0.6949      1.439    0.5438    0.8879
    ## 
    ## Concordance= 0.543  (se = 0.014 )
    ## Likelihood ratio test= 8.82  on 1 df,   p=0.003
    ## Wald test            = 8.47  on 1 df,   p=0.004
    ## Score (logrank) test = 8.57  on 1 df,   p=0.003
