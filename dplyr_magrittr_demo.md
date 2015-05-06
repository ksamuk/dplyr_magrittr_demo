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
##   [1]  0.54498166 -0.55740463  2.10139266 -1.19970746 -0.51800911
##   [6]  0.14237035  0.61618766 -1.56816613 -0.77433014  1.15856474
##  [11] -0.59352236  0.38797796 -1.32907819  1.64107734  2.15082016
##  [16]  0.96565100 -0.98048638 -0.95678211  0.53603560 -0.07344494
##  [21] -1.28674344  1.02439593  1.18617003 -0.79595462  0.77566624
##  [26]  0.68950038 -0.12651633  2.14685381 -2.37854798  0.22836029
##  [31]  0.01257522  0.52259518  1.62091090 -0.21420539 -0.48310802
##  [36]  0.09385042  0.83117018  0.87206076 -0.43102938  0.46416346
##  [41]  0.75127557  1.22317989  1.10062568 -0.23923855 -1.93412584
##  [46] -1.33778846 -0.02427907 -0.67904458 -0.62838695 -0.28414471
##  [51]  0.16326662  1.03977671  0.46926941  1.34442882  0.65050190
##  [56]  1.33308620  0.61080784 -0.39731185  1.05769083  1.40744877
##  [61]  0.26169844  1.15576827  0.08492133 -0.66351108  1.30174786
##  [66]  0.01482880 -1.25823199 -0.76904282 -0.09994480 -0.69476656
##  [71] -0.06138468 -0.21446104  1.47218037 -1.39566340 -0.17968629
##  [76] -1.03053669  0.42929445 -0.24744040 -0.34307883 -0.48401229
##  [81] -2.12114694 -0.66844397 -0.05567913 -0.78108042  0.51831830
##  [86] -0.64280763 -1.08916789 -1.81676949 -2.54190813  0.86991049
##  [91]  0.69092822  0.04235787 -0.56566695  0.50476283 -0.40272168
##  [96]  1.57998291 -1.01696305 -0.09129852 -1.74306742  0.20775417
```

```r
# basic pipe usage
# output of pipe is (by default) passed to *first* argument of target function
rnorm(100) %>% mean
```

```
## [1] 0.05844024
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
## [1] -0.1028969
```

```r
100 %>% rnorm %>% abs %>% sqrt %>% mean
```

```
## [1] 0.806555
```

```r
#compare to parenthesis mess below
mean(sqrt(abs(rnorm(100))))
```

```
## [1] 0.8022413
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
## [1] 10.0576
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
# pipe to lapply
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
## [1] 0.925453
```

```r
# can use these as you would use any function e.g. with lapply
rnorm.list <- replicate(5, rnorm(30), simplify = FALSE)
lapply(rnorm.list, funct)
```

```
## [[1]]
## [1] 0.7430265
## 
## [[2]]
## [1] 0.8082493
## 
## [[3]]
## [1] 0.8037662
## 
## [[4]]
## [1] 0.8066425
## 
## [[5]]
## [1] 0.915428
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
## [1] -0.005846378
```

```r
# or, short form is the %$% ("exposition") operator
list(x = rnorm(100), y = runif(100)) %$% cor(x, y)
```

```
## [1] 0.1040541
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
##   [1] -0.405098264 -0.193042330 -0.294282093 -0.064975732  0.602973471
##   [6] -0.740370094  0.351886670 -0.647131879  0.267753544 -0.695025967
##  [11]  0.803845090 -0.582505046  0.391038348 -0.357235377  0.018058563
##  [16]  0.952432101 -0.133773837 -1.768400763 -0.381699801 -1.289179790
##  [21]  0.281975934 -0.561196742  0.044913764  0.503894551  0.218549880
##  [26]  0.463584968  0.369444492 -0.774395729 -0.633589796  0.902663853
##  [31]  0.234537902 -0.301683873 -0.454608429 -0.096963036 -1.530599054
##  [36]  0.425935694 -0.492410875 -1.835768384 -1.141216566 -2.422091971
##  [41] -0.411065855 -0.785979078 -0.477836367 -0.546501258 -0.893735824
##  [46] -1.451987203 -2.197097170  1.071512907  0.414016290 -0.512688455
##  [51] -0.352351576 -1.692049235  0.105134211  0.221875130 -3.401931632
##  [56] -9.925832832 -1.173068061 -1.909619751 -0.083484318  0.146669703
##  [61] -0.882776044 -0.014029983 -0.771158455 -3.811997158 -0.473208240
##  [66] -1.664139507  0.987723190 -1.796295127 -2.134910548  0.475973768
##  [71]  0.009685447 -0.757346305  0.280462192 -0.182669660 -0.341240893
##  [76]  0.172230642 -2.451434577 -1.361638147  0.555760577  0.284833186
##  [81] -0.364012009 -0.430904943 -0.507126368  0.469777729 -0.659087914
##  [86]  0.130964272 -1.597238896 -0.443055793 -0.444453505 -0.552238766
##  [91] -1.459837959  0.104745978 -0.008232791  0.019038581 -1.499063967
##  [96] -0.159386794 -0.242817908 -1.566110240 -1.295971447 -0.762212860
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
## -2.31144 -0.74246  0.09954  0.67330  2.52671 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)
## (Intercept) -0.06605    0.10372  -0.637    0.526
## x            0.05164    0.09002   0.574    0.568
## 
## Residual standard error: 1.011 on 98 degrees of freedom
## Multiple R-squared:  0.003347,	Adjusted R-squared:  -0.006823 
## F-statistic: 0.3291 on 1 and 98 DF,  p-value: 0.5675
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
## -2.50505 -0.73383  0.06782  0.68550  2.39060 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)
## (Intercept) -0.007119   0.103074  -0.069    0.945
## x            0.069247   0.095991   0.721    0.472
## 
## Residual standard error: 1.024 on 98 degrees of freedom
## Multiple R-squared:  0.005282,	Adjusted R-squared:  -0.004868 
## F-statistic: 0.5204 on 1 and 98 DF,  p-value: 0.4724
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
## 1       Asia Afghanistan                48
## 2     Europe     Albania                40
## 3     Africa     Algeria                42
## 4     Africa      Angola                32
## 5   Americas   Argentina                37
## 6    Oceania   Australia                33
## 7     Europe     Austria                40
## 8       Asia     Bahrain                41
## 9       Asia  Bangladesh                37
## 10    Europe     Belgium                45
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
## 1  Afghanistan      Asia 1952  28.801  8425333  779.4453                48
## 2  Afghanistan      Asia 1957  30.332  9240934  820.8530                48
## 3  Afghanistan      Asia 1962  31.997 10267083  853.1007                48
## 4  Afghanistan      Asia 1967  34.020 11537966  836.1971                48
## 5  Afghanistan      Asia 1972  36.088 13079460  739.9811                48
## 6  Afghanistan      Asia 1977  38.438 14880372  786.1134                48
## 7  Afghanistan      Asia 1982  39.854 12881816  978.0114                48
## 8  Afghanistan      Asia 1987  40.822 13867957  852.3959                48
## 9  Afghanistan      Asia 1992  41.674 16317921  649.3414                48
## 10 Afghanistan      Asia 1997  41.763 22227415  635.3414                48
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

