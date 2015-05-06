# magrittr/dplyr demo
Andrew and Kieran  
May 5, 2015  


### Part 1: magrittr basics
install.packages("magrittr")
install.packages("gapminder")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("visreg")


```r
#### KIERAN

# load libraries
library("magrittr")
library("gapminder")
library("dplyr")
```

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library("ggplot2")
library("visreg")

# recall function rnorm
rnorm(n = 100, mean = 0)
```

```
##   [1] -1.79177221  0.92635842  0.75414638  0.39382717 -0.50866225
##   [6]  0.39390611 -1.17898857  1.33637275 -0.93695296  0.78402416
##  [11] -0.34711699 -1.34663536 -0.64536553  1.47496360  1.66053870
##  [16] -0.04996412 -0.37488168 -0.84324693  1.71645262  0.83069719
##  [21]  2.79438201  1.32676433 -0.15674622  0.59733836 -0.49679888
##  [26]  0.03213423 -0.03135591  0.14177145 -0.50477862  0.75443386
##  [31]  0.36848786 -0.87814103 -1.15724716  0.03714142 -1.19945915
##  [36]  1.36870544  0.33550339  0.09928145 -0.06546360 -0.63701600
##  [41] -0.43629401  1.29100307 -0.36019269 -0.03667265  0.97399519
##  [46]  0.13523812 -0.77889065 -1.20591018 -2.15066116 -1.16669771
##  [51]  0.41175545 -0.64744202  1.11182471  3.33408900  1.64206842
##  [56]  1.70139278  0.41598993 -0.24783701  0.88293884 -1.22912052
##  [61] -0.43052954  0.10968675  0.46647985 -0.74980406 -0.09545764
##  [66]  0.60957356 -0.83467814  0.80756512  1.29368654 -0.51364559
##  [71] -1.59836212  0.34648100  0.99682515 -0.42176791 -1.12453668
##  [76]  0.60741436 -0.88335850  1.33760311  0.21049577  2.13824756
##  [81]  0.46903085  1.04994781 -0.36440875  1.49839031  1.72357446
##  [86]  2.05185469 -1.00365815 -0.10417621  1.71154599  1.62435759
##  [91] -0.23618776  0.08085988 -0.80201598 -0.37428369 -0.58098327
##  [96]  0.42840889 -1.02706835 -0.01620939 -0.50212315  0.34455687
```

```r
# basic pipe usage
# output of pipe is (by default) passed to *first* argument of target function
rnorm(100) %>% mean
```

```
## [1] -0.2415454
```

```r
rnorm(100) %>% hist
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-1-1.png) 

```r
#this includes when arguments other than the first are explicitly defined, e.g.
100 %>% rnorm(mean = 10) %>% hist
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-1-2.png) 

```r
# can pipe to as many functions as you like
100 %>% rnorm %>% mean
```

```
## [1] 0.01029309
```

```r
100 %>% rnorm %>% abs %>% sqrt %>% mean
```

```
## [1] 0.7892278
```

```r
#compare to parenthesis mess below
mean(sqrt(abs(rnorm(100))))
```

```
## [1] 0.8369002
```

```r
#another example
letters %>% toupper %>% rev %>% paste0 (collapse = "") 
```

```
## [1] "ZYXWVUTSRQPONMLKJIHGFEDCBA"
```

```r
# what if we want to pipe in places other than the first argument?
# use "."
10 %>% rnorm(n = 100, mean = .) %>% mean
```

```
## [1] 9.954663
```

```r
#### ANDREW 

# pipe a dataframe to subset 
gapminder %>% subset(country == "Zambia") 
```

```
##      country continent year lifeExp      pop gdpPercap
## 1681  Zambia    Africa 1952  42.038  2672000  1147.389
## 1682  Zambia    Africa 1957  44.077  3016000  1311.957
## 1683  Zambia    Africa 1962  46.023  3421000  1452.726
## 1684  Zambia    Africa 1967  47.768  3900000  1777.077
## 1685  Zambia    Africa 1972  50.107  4506497  1773.498
## 1686  Zambia    Africa 1977  51.386  5216550  1588.688
## 1687  Zambia    Africa 1982  51.821  6100407  1408.679
## 1688  Zambia    Africa 1987  50.821  7272406  1213.315
## 1689  Zambia    Africa 1992  46.100  8381163  1210.885
## 1690  Zambia    Africa 1997  40.238  9417789  1071.354
## 1691  Zambia    Africa 2002  39.193 10595811  1071.614
## 1692  Zambia    Africa 2007  42.384 11746035  1271.212
```

```r
# what will this do?
gapminder %>% lapply(class)
```

```
## $country
## [1] "factor"
## 
## $continent
## [1] "factor"
## 
## $year
## [1] "numeric"
## 
## $lifeExp
## [1] "numeric"
## 
## $pop
## [1] "numeric"
## 
## $gdpPercap
## [1] "numeric"
```

```r
# pipin hot linear models
gapminder %>% lm(lifeExp ~ gdpPercap, data = .)
```

```
## 
## Call:
## lm(formula = lifeExp ~ gdpPercap, data = .)
## 
## Coefficients:
## (Intercept)    gdpPercap  
##   5.396e+01    7.649e-04
```

```r
gapminder %>% lm(lifeExp ~ gdpPercap, data = .) %>% summary
```

```
## 
## Call:
## lm(formula = lifeExp ~ gdpPercap, data = .)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -82.754  -7.758   2.176   8.225  18.426 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 5.396e+01  3.150e-01  171.29   <2e-16 ***
## gdpPercap   7.649e-04  2.579e-05   29.66   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 10.49 on 1702 degrees of freedom
## Multiple R-squared:  0.3407,	Adjusted R-squared:  0.3403 
## F-statistic: 879.6 on 1 and 1702 DF,  p-value: < 2.2e-16
```

```r
gapminder %>% lm(lifeExp ~ gdpPercap, data = .) %>% anova
```

```
## Analysis of Variance Table
## 
## Response: lifeExp
##             Df Sum Sq Mean Sq F value    Pr(>F)    
## gdpPercap    1  96813   96813  879.58 < 2.2e-16 ***
## Residuals 1702 187335     110                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
gapminder %>% lm(lifeExp ~ gdpPercap + continent + country + pop, data=.) %>% drop1
```

```
## Single term deletions
## 
## Model:
## lifeExp ~ gdpPercap + continent + country + pop
##            Df Sum of Sq    RSS    AIC
## <none>                   59768 6350.0
## gdpPercap   1      6713  66481 6529.4
## continent   0         0  59768 6350.0
## country   137     58987 118754 7245.9
## pop         1      6285  66053 6518.3
```

```r
# we can also assign the output of a pipe like so:
gm.mod <- gapminder %>% lm(lifeExp ~ log(gdpPercap), data = .)
visreg(gm.mod)
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-1-3.png) 

### Part 2: magrittr extended


```r
#### KIERAN

# define "functional sequences""
funct <- . %>% abs %>% sqrt %>% mean()
rnorm(100) %>% funct
```

```
## [1] 0.8827504
```

```r
# can use these as you would use any function e.g. with lapply
rnorm.list <- replicate(5, rnorm(30), simplify = FALSE)
lapply(rnorm.list, funct)
```

```
## [[1]]
## [1] 0.8333068
## 
## [[2]]
## [1] 0.850107
## 
## [[3]]
## [1] 0.8672524
## 
## [[4]]
## [1] 0.8191296
## 
## [[5]]
## [1] 0.8886771
```

```r
# same as
f <- function(x){
  mean(sqrt(abs(x)))
}

# what if we need to make each step a bit more complex? 
# could define a function outside, but can also do it "in line"
# i.e. lambda expressions
 rnorm(100) %>% 
  abs() %>% {
  x <- sqrt(.)
  y <- exp(.)
  x * y
  } %>%
  hist
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-2-1.png) 

```r
# what if we want to pass more than one arg?
# can use "with"
list(x = rnorm(100), y = runif(100)) %>% with(cor(x, y))
```

```
## [1] -0.01532394
```

```r
# or, short form is the %$% ("exposition") operator
list(x = rnorm(100), y = runif(100)) %$% cor(x, y)
```

```
## [1] 0.1200025
```

```r
# what if we want to pipe through a function with no return value (e.g. a plot?), but continue the pipe?
# can use the "tee" operator %T>%
# creates a "branch" in the pipe

rnorm(100) %>%
  abs %T>%
  hist %>%
  log
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-2-2.png) 

```
##   [1] -0.41027565 -0.53460629 -0.27416895 -0.91369865 -1.27248490
##   [6] -2.52673521 -1.99958161  0.08938978  0.32790456 -0.03802384
##  [11]  0.27983106 -0.67448757  0.21334716 -0.40678429  0.46817843
##  [16] -1.46194593 -3.25705363  0.42908810 -0.03862497 -0.54982444
##  [21] -0.65127000 -1.27528708  0.52504361 -0.58632575 -1.98586428
##  [26]  1.10360331 -0.82390767 -2.15943334 -0.53944085 -1.63443289
##  [31] -0.98870620 -2.31804510  0.25559815  0.23422078  0.03568867
##  [36] -0.94173799 -2.59523449 -1.51593783 -1.49450111  0.50880111
##  [41] -1.54555417 -2.59486081 -3.42518953  0.73529609 -0.69949228
##  [46] -0.61527828 -4.14519143 -0.36968567 -0.75467288 -2.77189178
##  [51] -3.58658555 -0.73748274  0.29155663  0.66546179 -2.40305701
##  [56] -0.25671843 -0.52888584  0.21155820 -0.19204236 -2.51063908
##  [61] -0.47129143 -1.36087188 -0.68155326 -0.72698114 -0.69248213
##  [66] -0.47656707  0.77155562 -0.02107222  0.16028526 -0.16902296
##  [71] -0.99261153  0.09058211 -0.82864348 -0.32874181 -2.69519365
##  [76]  0.36531869 -0.98041554 -2.75285184 -1.45830050 -1.28157689
##  [81]  0.08690275  0.29348486  0.21149728 -0.70755577 -0.36861550
##  [86]  0.67322046  0.07618371  0.13320931 -0.24906616 -0.84873134
##  [91]  0.31759137  0.78785102  0.22420230  0.90473010 -0.35197264
##  [96] -2.00073522 -0.90719646  0.02084653 -1.17174508 -0.08125942
```

```r
list(x = rnorm(100), y = rnorm(100)) %T>% 
  with(plot(x, y)) %>%
  with(lm(y ~ x)) %T>% 
  abline %>% 
  summary
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-2-3.png) 

```
## 
## Call:
## lm(formula = y ~ x)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -2.32104 -0.58658 -0.05603  0.59722  2.92799 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept) -0.02991    0.09384  -0.319   0.7506  
## x           -0.21786    0.10748  -2.027   0.0454 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.9382 on 98 degrees of freedom
## Multiple R-squared:  0.04024,	Adjusted R-squared:  0.03044 
## F-statistic: 4.109 on 1 and 98 DF,  p-value: 0.04538
```

```r
plot.model <- . %T>% 
  with(plot(x, y)) %>%
  with(lm(y ~ x)) %T>% 
  abline %>% 
  summary

list(x = rnorm(100), y = rnorm(100)) %>% plot.model
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-2-4.png) 

```
## 
## Call:
## lm(formula = y ~ x)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -1.92745 -0.62483 -0.00727  0.59795  2.71905 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)
## (Intercept) -0.03133    0.09417  -0.333     0.74
## x           -0.04657    0.08622  -0.540     0.59
## 
## Residual standard error: 0.9412 on 98 degrees of freedom
## Multiple R-squared:  0.002968,	Adjusted R-squared:  -0.007206 
## F-statistic: 0.2917 on 1 and 98 DF,  p-value: 0.5903
```

### Part 3: dplyr basics


```r
#### ANDREW

# create a data frame tbl (not usually necessary)
gm <- gapminder %>% tbl_df 

# tbl_dfs can be easily converted back to datafames
gm %>% data.frame() %>% head()
```

```
##       country continent year lifeExp      pop gdpPercap
## 1 Afghanistan      Asia 1952  28.801  8425333  779.4453
## 2 Afghanistan      Asia 1957  30.332  9240934  820.8530
## 3 Afghanistan      Asia 1962  31.997 10267083  853.1007
## 4 Afghanistan      Asia 1967  34.020 11537966  836.1971
## 5 Afghanistan      Asia 1972  36.088 13079460  739.9811
## 6 Afghanistan      Asia 1977  38.438 14880372  786.1134
```

```r
# arrage dataframe by a variable
gm %>% arrange(year)
```

```
## Source: local data frame [1,704 x 6]
## 
##        country continent year lifeExp      pop  gdpPercap
## 1  Afghanistan      Asia 1952  28.801  8425333   779.4453
## 2      Albania    Europe 1952  55.230  1282697  1601.0561
## 3      Algeria    Africa 1952  43.077  9279525  2449.0082
## 4       Angola    Africa 1952  30.015  4232095  3520.6103
## 5    Argentina  Americas 1952  62.485 17876956  5911.3151
## 6    Australia   Oceania 1952  69.120  8691212 10039.5956
## 7      Austria    Europe 1952  66.800  6927772  6137.0765
## 8      Bahrain      Asia 1952  50.939   120447  9867.0848
## 9   Bangladesh      Asia 1952  37.484 46886859   684.2442
## 10     Belgium    Europe 1952  68.000  8730405  8343.1051
## ..         ...       ...  ...     ...      ...        ...
```

```r
# select columns of a dataframe
gm %>% select(year, country, continent)
```

```
## Source: local data frame [1,704 x 3]
## 
##    year     country continent
## 1  1952 Afghanistan      Asia
## 2  1957 Afghanistan      Asia
## 3  1962 Afghanistan      Asia
## 4  1967 Afghanistan      Asia
## 5  1972 Afghanistan      Asia
## 6  1977 Afghanistan      Asia
## 7  1982 Afghanistan      Asia
## 8  1987 Afghanistan      Asia
## 9  1992 Afghanistan      Asia
## 10 1997 Afghanistan      Asia
## ..  ...         ...       ...
```

```r
# create a new column 
gm %>% mutate(pop.thou = pop/1000)
```

```
## Source: local data frame [1,704 x 7]
## 
##        country continent year lifeExp      pop gdpPercap  pop.thou
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453  8425.333
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530  9240.934
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007 10267.083
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971 11537.966
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811 13079.460
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134 14880.372
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114 12881.816
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959 13867.957
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414 16317.921
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414 22227.415
## ..         ...       ...  ...     ...      ...       ...       ...
```

```r
gm %>% mutate(pop.thou = pop/1000, pop.mil = pop/1000000)
```

```
## Source: local data frame [1,704 x 8]
## 
##        country continent year lifeExp      pop gdpPercap  pop.thou
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453  8425.333
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530  9240.934
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007 10267.083
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971 11537.966
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811 13079.460
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134 14880.372
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114 12881.816
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959 13867.957
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414 16317.921
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414 22227.415
## ..         ...       ...  ...     ...      ...       ...       ...
## Variables not shown: pop.mil (dbl)
```

```r
# filter rows by some criteria
gm %>% filter(country == "Zambia")
```

```
## Source: local data frame [12 x 6]
## 
##    country continent year lifeExp      pop gdpPercap
## 1   Zambia    Africa 1952  42.038  2672000  1147.389
## 2   Zambia    Africa 1957  44.077  3016000  1311.957
## 3   Zambia    Africa 1962  46.023  3421000  1452.726
## 4   Zambia    Africa 1967  47.768  3900000  1777.077
## 5   Zambia    Africa 1972  50.107  4506497  1773.498
## 6   Zambia    Africa 1977  51.386  5216550  1588.688
## 7   Zambia    Africa 1982  51.821  6100407  1408.679
## 8   Zambia    Africa 1987  50.821  7272406  1213.315
## 9   Zambia    Africa 1992  46.100  8381163  1210.885
## 10  Zambia    Africa 1997  40.238  9417789  1071.354
## 11  Zambia    Africa 2002  39.193 10595811  1071.614
## 12  Zambia    Africa 2007  42.384 11746035  1271.212
```

```r
gm %>% filter(country == "Zambia", year < 1977)
```

```
## Source: local data frame [5 x 6]
## 
##   country continent year lifeExp     pop gdpPercap
## 1  Zambia    Africa 1952  42.038 2672000  1147.389
## 2  Zambia    Africa 1957  44.077 3016000  1311.957
## 3  Zambia    Africa 1962  46.023 3421000  1452.726
## 4  Zambia    Africa 1967  47.768 3900000  1777.077
## 5  Zambia    Africa 1972  50.107 4506497  1773.498
```

```r
# create a grouped tbl_df ("grouped_df")
gm %>% 
  group_by(country)
```

```
## Source: local data frame [1,704 x 6]
## Groups: country
## 
##        country continent year lifeExp      pop gdpPercap
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414
## ..         ...       ...  ...     ...      ...       ...
```

```r
# apply a function to the groups (e.g. summarise)
gm %>% 
  group_by(country) %>%
  summarize(mean_life = mean(lifeExp))
```

```
## Source: local data frame [142 x 2]
## 
##        country mean_life
## 1  Afghanistan  37.47883
## 2      Albania  68.43292
## 3      Algeria  59.03017
## 4       Angola  37.88350
## 5    Argentina  69.06042
## 6    Australia  74.66292
## 7      Austria  73.10325
## 8      Bahrain  65.60567
## 9   Bangladesh  49.83408
## 10     Belgium  73.64175
## ..         ...       ...
```

```r
# multiple groups can be specified
gm %>% 
  group_by(continent, country) 
```

```
## Source: local data frame [1,704 x 6]
## Groups: continent, country
## 
##        country continent year lifeExp      pop gdpPercap
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414
## ..         ...       ...  ...     ...      ...       ...
```

```r
# functions are applied from last to first specified
gm %>% 
  group_by(continent, country) %>%
  summarize(mean_life = mean(lifeExp)) 
```

```
## Source: local data frame [142 x 3]
## Groups: continent
## 
##    continent                  country mean_life
## 1     Africa                  Algeria  59.03017
## 2     Africa                   Angola  37.88350
## 3     Africa                    Benin  48.77992
## 4     Africa                 Botswana  54.59750
## 5     Africa             Burkina Faso  44.69400
## 6     Africa                  Burundi  44.81733
## 7     Africa                 Cameroon  48.12850
## 8     Africa Central African Republic  43.86692
## 9     Africa                     Chad  46.77358
## 10    Africa                  Comoros  52.38175
## ..       ...                      ...       ...
```

```r
# after the last function is applied, a tbl_df (no groups) is returned
gm %>% 
  group_by(continent, country) %>%
  summarize(mean_life = mean(lifeExp)) %>%
  summarize(mean_contient_life = mean(mean_life))
```

```
## Source: local data frame [5 x 2]
## 
##   continent mean_contient_life
## 1    Africa           48.86533
## 2  Americas           64.65874
## 3      Asia           60.06490
## 4    Europe           71.90369
## 5   Oceania           74.32621
```

```r
# tally: count observations in groups
gm %>% 
  group_by(continent, country) %>%
  tally %>%
  tally
```

```
## Using n as weighting variable
```

```
## Source: local data frame [5 x 2]
## 
##   continent   n
## 1    Africa 624
## 2  Americas 300
## 3      Asia 396
## 4    Europe 360
## 5   Oceania  24
```

```r
### KIERAN

# example of a complete chain
library(ggplot2)

gm %>% 
  group_by(continent, year) %>%
  summarize(meanlife = mean(lifeExp)) %>%
  ggplot(aes(x = year, y = meanlife, colour = continent)) + 
    geom_point() + 
    geom_line()
```

![](dplyr_magrittr_demo_files/figure-html/unnamed-chunk-3-1.png) 

### Part 4: dplyr extended


```r
### KIERAN

## let's make some fake data
pirates <- gm %>% 
  select(continent, country) %>% 
  distinct %>% 
  rowwise %>%
  mutate(number_of_pirates = rpois(1, lambda = 42))

pirates
```

```
## Source: local data frame [142 x 3]
## Groups: <by row>
## 
##    continent     country number_of_pirates
## 1       Asia Afghanistan                46
## 2     Europe     Albania                42
## 3     Africa     Algeria                32
## 4     Africa      Angola                44
## 5   Americas   Argentina                42
## 6    Oceania   Australia                45
## 7     Europe     Austria                47
## 8       Asia     Bahrain                52
## 9       Asia  Bangladesh                41
## 10    Europe     Belgium                48
## ..       ...         ...               ...
```

```r
## join to the original
gm_pirates <- left_join(gm, pirates)
```

```
## Joining by: c("country", "continent")
```

```r
gm_pirates
```

```
## Source: local data frame [1,704 x 7]
## 
##        country continent year lifeExp      pop gdpPercap number_of_pirates
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453                46
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530                46
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007                46
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971                46
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811                46
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134                46
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114                46
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959                46
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414                46
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414                46
## ..         ...       ...  ...     ...      ...       ...               ...
```

```r
## this also works if there are missing values:

some_pirates <- pirates %>% 
  sample_frac(0.6,) 

# dim, unique, blah blah

some_pirates %>% 
  left_join(gm, .)
```

```
## Joining by: c("country", "continent")
```

```
## Source: local data frame [1,704 x 7]
## 
##        country continent year lifeExp      pop gdpPercap number_of_pirates
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453                46
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530                46
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007                46
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971                46
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811                46
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134                46
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114                46
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959                46
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414                46
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414                46
## ..         ...       ...  ...     ...      ...       ...               ...
```



## do lets you perform any arbitrary calculations

```r
#### ANDREW 

model_pirates <- gm_pirates %>% 
  group_by(country) %>% 
  do(model = lm(lifeExp ~ year, data = .))

## in a list
model_pirates$model[1]
```

```
## [[1]]
## 
## Call:
## lm(formula = lifeExp ~ year, data = .)
## 
## Coefficients:
## (Intercept)         year  
##   -507.5343       0.2753
```

```r
## a model
model_pirates$model[[1]]
```

```
## 
## Call:
## lm(formula = lifeExp ~ year, data = .)
## 
## Coefficients:
## (Intercept)         year  
##   -507.5343       0.2753
```

```r
model_pirates %>% 
  mutate(rsq = summary(model)$r.squared)
```

```
## Source: local data frame [142 x 3]
## Groups: <by row>
## 
##        country   model       rsq
## 1  Afghanistan <S3:lm> 0.9477123
## 2      Albania <S3:lm> 0.9105778
## 3      Algeria <S3:lm> 0.9851172
## 4       Angola <S3:lm> 0.8878146
## 5    Argentina <S3:lm> 0.9955681
## 6    Australia <S3:lm> 0.9796477
## 7      Austria <S3:lm> 0.9921340
## 8      Bahrain <S3:lm> 0.9667398
## 9   Bangladesh <S3:lm> 0.9893609
## 10     Belgium <S3:lm> 0.9945406
## ..         ...     ...       ...
```

```r
get_rsq <- function(df){
  mod <- lm(lifeExp ~ year, data = df)
  rsq <- summary(mod)$r.squared
  data.frame(Rval = rsq)
}

gm_pirates %>% 
  group_by(country) %>% 
  do(get_rsq(df = .))
```

```
## Source: local data frame [142 x 2]
## Groups: country
## 
##        country      Rval
## 1  Afghanistan 0.9477123
## 2      Albania 0.9105778
## 3      Algeria 0.9851172
## 4       Angola 0.8878146
## 5    Argentina 0.9955681
## 6    Australia 0.9796477
## 7      Austria 0.9921340
## 8      Bahrain 0.9667398
## 9   Bangladesh 0.9893609
## 10     Belgium 0.9945406
## ..         ...       ...
```



```r
# won't knit for some reason

# is.out <- . %>% {
#   quant <- . %>% 
#   quantile(
#     na.rm = TRUE,
#     probs = 0.95) %>% 
#   extract2(1)
#  
#   test <- . %>% 
#     is_greater_than(quant)
#   
#   .[test(.)]
#     
# }
# 
# is.out(rnorm(30))
```

