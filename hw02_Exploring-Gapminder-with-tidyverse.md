Gapminder Unwinder: Hwk\_2
================
*d8a-m8* a.k.a. *delta thru data* a.k.a Yevgen '*the d8a nonh8a*' Kovalenko
2016-09-26

Minder the Gap. The door's are now closing.
-------------------------------------------

Welcome! This should be a fun ride today for you and I. So in this following report we will be exploring the Gapminder dataset...however, I will have no idea what you will end up seeing.

So, get some coffee or tea or any consumable, potable liquid %&gt;% temperature(hot:cold) and let's get started. :D

Why?

``` r
library("random")
```

That's why. I will be using the *random* package to make this interesting. Let's explore the relationship between life expectancy as a function of GDP per capita. (*You will need internet connection to run some of this code. Especially pertaining to the **random** functions.*)

Initially, let us see what we are working with before any *<sub>CrA</sub>* *Z* *~<sub>~</sub> neSS ~<sub>~</sub>*.

``` r
str(gapminder)  
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

``` r
#Exploring tbl.df of gapminder, also could have formatted the script as in Chuck 4 (Summary)
```

As expected this dataset is a tbl = tibble dataframe. Buckets of observations (1704) of the six tracked variables: country(factor), continent(factor), year(integer), life expectancy(double/float), population(integer), GDP per capita(double/float). We can also see there were 152 surveyed countries on all **five** continents.

*<sub>looking</sub> <sub>at</sub> <sub>you</sub> **<sub>ANTarctica</sub>***

To be clear, Gapminder is a tibble dataframe with 1704 rows of observations catergorized by six columns of factors/variables. These variables and the corresponding flavours are listed above.

Apart from using str(), simply typing the object name would provide that data following "\#A tibble: ...".

Using the summary function, we can look at the basic statistical data of this dataframe. In particular, let us only consider the year, life expectancy, population, and GDP.

``` r
gapminder[,3:6] %>% 
  summary()
```

    ##       year         lifeExp           pop              gdpPercap       
    ##  Min.   :1952   Min.   :23.60   Min.   :6.001e+04   Min.   :   241.2  
    ##  1st Qu.:1966   1st Qu.:48.20   1st Qu.:2.794e+06   1st Qu.:  1202.1  
    ##  Median :1980   Median :60.71   Median :7.024e+06   Median :  3531.8  
    ##  Mean   :1980   Mean   :59.47   Mean   :2.960e+07   Mean   :  7215.3  
    ##  3rd Qu.:1993   3rd Qu.:70.85   3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
    ##  Max.   :2007   Max.   :82.60   Max.   :1.319e+09   Max.   :113523.1

Here we can see the global maximums and minimums of all four quantitative data, in addition to the first and third quartiles, the median, and the mean. Let's only consider life expectancy and GDP for now with respect to countries and continents.

Here are some histograms (actually they are density plots, since density &gt; histogram) of these two variables without respect to continent or country.

``` r
p_hist <- gapminder %>% 
  ggplot()

p_hist + geom_density(aes(x = lifeExp), alpha = 0.5)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/Histograms-1.png)

``` r
p_hist + geom_density(aes(x = gdpPercap),alpha = 0.5)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/Histograms-2.png)

With respect to continents? <sub>(</sub> <sub>countries</sub> <sub>would</sub> <sub>be</sub> <sub>crazy</sub> <sub>)</sub>

Sidenote: I was initially running into troubles with facet\_wrap and using the pipe function to isolate a variable from gapminder.

``` r
p_hist + facet_wrap(~ continent) + geom_density(aes(x = lifeExp), alpha = 0.5) 
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
p_hist + facet_wrap(~ continent) + geom_density(aes(x = gdpPercap), alpha = 0.5)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/unnamed-chunk-2-1.png)

Investigation of Socio-Ecomonics in Gapminder
---------------------------------------------

Now let's look at the exact same parameters as before and dive into how GDP and life expectancy correlate. *<sub>(</sub> <sub>Now</sub> <sub>with</sub> <sub>ggplots</sub> <sub>and</sub> <sub>dplyr</sub> <sub>)</sub>*

``` r
pt_lvg <- gapminder %>% 
  ggplot()

#Making plot space

pt_lvg + geom_point(aes(x = gdpPercap, y = lifeExp, colour = continent))  
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/Plot%20L%20v.%20G%20data-1.png)

``` r
#Assigning points
```

Interesting...I have an idea for a future homework assignment and if I get it done in time I'll add it here too.

Anyways, let's plot this on a different scale.

``` r
pt_lvg + geom_point(aes(x = log10(gdpPercap), y = log10(lifeExp), colour = continent)) + geom_smooth(aes(x = log10(gdpPercap), y = log10(lifeExp)))
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/Replot-1.png)

``` r
#or I could have added "+ scale_x_log10()" instead of two log10() :/
```

Really interesting. There does appear to be some linear correlation, particularly in the middle of the plot, but the correlation breaks down at the fringes. Possibly due to unknown, unconsidered factors.

~~ Pearson's Correlation Coefficient & Coefficient of Determination~~

``` r
r_p <- with(gapminder, cor(log10(gdpPercap), log10(lifeExp))) #Assigning object that is the P.C.C. of lifeExp and GDP

r_p #Pearson's C.C. (P.C.C.)
```

    ## [1] 0.7830724

``` r
r_p^2 #Coefficent of determination
```

    ## [1] 0.6132023

As seen above, there is some correlation between life expectancy and GDP per capita. Perhaps this is trivial because as GDP per capita increases, it could be said that quality of life also increases and thus, perhaps life expectancy. Although this clearly does not paint the complete picture (R = 0.783, (R<sup>2</sup>) = 0.613) as the other factors will affect both that may or may not affect the other.

Perhaps, later I will input a model here onced I figure out how to avoid the "singlar gradient" error. The help online is too maths for me, right now.

Back to business.

``` r
plot(lifeExp ~ continent, data = gapminder)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/unnamed-chunk-3-1.png)

``` r
plot(log10(gdpPercap) ~ continent, data = gapminder)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/unnamed-chunk-3-2.png)

The above trends may reflect the socio-economic status of a continent as life expectancy can be seen to correlate with GDP. Let's take a look to see if this trend holds up for different places. Let us make an arbitrary decision on which countries to investigate. *INCOMING: random data retreiver*

``` r
country_names <- unique(gapminder['country'])

cnm <- grep(
  as.list(randomStrings(n=1,len=1,digits=F)), 
  as.matrix(country_names[,1]))

name.list <- country_names[cnm,]

print(name.list)
```

    ## # A tibble: 3 × 1
    ##       country
    ##        <fctr>
    ## 1 New Zealand
    ## 2      Zambia
    ## 3    Zimbabwe

``` r
new_gp <- gapminder %>% 
  filter(country %in% c(as.matrix(name.list)))

new_gp_data <- new_gp %>% 
  select(lifeExp, gdpPercap)
  
print(new_gp)
```

    ## # A tibble: 36 × 6
    ##        country continent  year lifeExp     pop gdpPercap
    ##         <fctr>    <fctr> <int>   <dbl>   <int>     <dbl>
    ## 1  New Zealand   Oceania  1952   69.39 1994794  10556.58
    ## 2  New Zealand   Oceania  1957   70.26 2229407  12247.40
    ## 3  New Zealand   Oceania  1962   71.24 2488550  13175.68
    ## 4  New Zealand   Oceania  1967   71.52 2728150  14463.92
    ## 5  New Zealand   Oceania  1972   71.89 2929100  16046.04
    ## 6  New Zealand   Oceania  1977   72.22 3164900  16233.72
    ## 7  New Zealand   Oceania  1982   73.84 3210650  17632.41
    ## 8  New Zealand   Oceania  1987   74.32 3317166  19007.19
    ## 9  New Zealand   Oceania  1992   76.33 3437674  18363.32
    ## 10 New Zealand   Oceania  1997   77.55 3676187  21050.41
    ## # ... with 26 more rows
