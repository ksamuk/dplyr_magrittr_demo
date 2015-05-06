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
##   [1]  0.73028894 -0.82581829  0.53691797  1.04127930 -0.36721063
##   [6] -0.26482429  1.25596443 -1.07602312 -1.27565893  0.58105813
##  [11]  0.36143767 -1.13377393 -1.73270928 -0.10752315 -0.11850624
##  [16]  0.91619432  0.97353422  1.27462228  0.11886807 -0.19880354
##  [21]  0.72994068 -1.98362725 -1.20064801 -0.14534103  0.52406124
##  [26] -1.56028915 -0.30090149  0.36997309  1.55651193  2.05936337
##  [31]  0.76743294 -1.47868916 -0.47424333 -0.79107887  0.79907281
##  [36]  0.19041331  1.91806916 -0.50144761 -0.28953573 -1.86071194
##  [41] -1.28299991 -0.02877454 -0.30618307 -0.29306163 -1.52319458
##  [46] -0.73058091  1.22166946  1.45938404  0.51595683 -0.61755184
##  [51] -0.76746085  0.52318851  0.13622010 -1.14991040  0.20233143
##  [56] -0.17927172 -1.11442865  0.28085615 -0.32349948 -0.76733212
##  [61]  1.57045235  0.14025960  0.49392695  0.37921552 -2.56445477
##  [66]  0.44383339 -1.78247509  1.37066497 -1.03226780 -0.18105956
##  [71]  0.30087590 -0.91811939  1.37077467 -0.34968123  0.10024533
##  [76]  0.16548152 -0.14270046 -0.14905645 -0.47950852 -0.07885604
##  [81] -0.48886173 -0.14338856 -0.51511153  0.74391444 -0.15163732
##  [86]  1.01798921  0.68493954  0.42510428 -0.03123434 -0.27249142
##  [91]  0.13355936 -2.03739398 -0.13607382 -0.20012420  0.91807225
##  [96]  1.12720887 -1.74742245 -0.86162533  0.53338741 -1.28622260
```

```r
# basic pipe usage
# output of pipe is (by default) passed to *first* argument of target function
rnorm(100) %>% mean
```

```
## [1] -0.06338429
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
## [1] 0.1743511
```

```r
100 %>% rnorm %>% abs %>% sqrt %>% mean
```

```
## [1] 0.827466
```

```r
#compare to parenthesis mess below
mean(sqrt(abs(rnorm(100))))
```

```
## [1] 0.8146814
```

```r
#another example
letters %>% toupper %>% rev %>% paste0 (collapse = "") 
```

```
## [1] "ZYXWVUTSRQPONMLKJIHGFEDCBA"
```

```r
#### ANDREW 

# what if we want to pipe in places other than the first argument?
# use "."
10 %>% rnorm(n = 100, mean = .) %>% mean
```

```
## [1] 10.05962
```

```r
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
## [1] 0.7888882
```

```r
# can use these as you would use any function e.g. with lapply
rnorm.list <- replicate(5, rnorm(30), simplify = FALSE)
lapply(rnorm.list, funct)
```

```
## [[1]]
## [1] 0.8892125
## 
## [[2]]
## [1] 0.8028951
## 
## [[3]]
## [1] 0.8187079
## 
## [[4]]
## [1] 0.8222541
## 
## [[5]]
## [1] 0.8586219
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
## [1] -0.08558304
```

```r
# or, short form is the %$% ("exposition") operator
list(x = rnorm(100), y = runif(100)) %$% cor(x, y)
```

```
## [1] -0.03749035
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
##   [1] -0.36459862 -1.45320532 -0.60828001 -0.03510861 -1.38099303
##   [6]  0.26927002 -1.49458666 -1.60436388  0.81024426 -1.37107614
##  [11]  0.22430913 -1.01660071 -1.86468174 -0.90167974  0.69534030
##  [16]  0.91487319 -0.82791984 -0.92693584 -0.37726842 -1.40522555
##  [21] -0.33873381 -0.94447941 -2.30849847 -1.12897746 -0.80003915
##  [26] -0.51881071 -0.81388776 -2.89369933  0.32063982  0.52962273
##  [31] -0.98755128  0.78154490 -2.99163056 -0.09621044  0.65045602
##  [36] -1.83455678  0.21817306 -2.15031135 -0.59742247 -0.11112556
##  [41]  0.39802964 -1.25527647  0.35385496 -1.84107647 -0.10054537
##  [46]  0.28413043 -0.05232280 -0.79036349 -1.20841894 -0.23506006
##  [51] -1.53028268 -1.10485335 -1.60303433 -0.43764266 -0.13945878
##  [56] -0.33769085 -1.20157423 -0.85916274  0.07897963 -1.68024896
##  [61] -0.62216346 -1.82578253 -0.04556000 -0.53905412  0.38315339
##  [66]  0.05275899 -0.42616907 -0.14222350 -0.69983581  0.05211981
##  [71] -0.19324325  0.08098984 -0.29165103 -0.04324247 -1.65138457
##  [76] -0.29462987 -1.21833414 -0.55507257 -0.12216752 -1.13840007
##  [81]  0.47301915  0.08879558 -1.26569794 -2.21733277 -0.22320667
##  [86] -1.74805790 -0.16249814 -3.08361844 -1.00284314 -0.44702379
##  [91]  0.27354423 -1.02340142 -3.65230655 -0.40269871  0.57855450
##  [96] -1.19735593 -4.45465715 -1.31008350 -1.03670764  0.54422803
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
##     Min      1Q  Median      3Q     Max 
## -2.7607 -0.6213  0.0562  0.6174  2.2909 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept) -0.20549    0.10364  -1.983   0.0502 .
## x           -0.08186    0.11417  -0.717   0.4751  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.036 on 98 degrees of freedom
## Multiple R-squared:  0.005218,	Adjusted R-squared:  -0.004933 
## F-statistic: 0.514 on 1 and 98 DF,  p-value: 0.4751
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
## -2.22647 -0.63532  0.02137  0.67276  2.52237 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)
## (Intercept)  0.086499   0.093203   0.928    0.356
## x           -0.001243   0.093496  -0.013    0.989
## 
## Residual standard error: 0.9317 on 98 degrees of freedom
## Multiple R-squared:  1.803e-06,	Adjusted R-squared:  -0.0102 
## F-statistic: 0.0001767 on 1 and 98 DF,  p-value: 0.9894
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
## 1       Asia Afghanistan                49
## 2     Europe     Albania                32
## 3     Africa     Algeria                45
## 4     Africa      Angola                44
## 5   Americas   Argentina                43
## 6    Oceania   Australia                44
## 7     Europe     Austria                29
## 8       Asia     Bahrain                35
## 9       Asia  Bangladesh                36
## 10    Europe     Belgium                54
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
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453                49
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530                49
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007                49
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971                49
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811                49
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134                49
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114                49
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959                49
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414                49
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414                49
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
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453                NA
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530                NA
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007                NA
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971                NA
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811                NA
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134                NA
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114                NA
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959                NA
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414                NA
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414                NA
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

