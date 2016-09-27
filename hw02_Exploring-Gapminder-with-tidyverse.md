Gapminder Unwinder: Hwk\_2
================
*d8a-m8* a.k.a. *delta thru data* a.k.a Yevgen '*the d8a nonh8a*' Kovalenko
2016-09-26

Minder the Gap. The door's are now closing.
-------------------------------------------

Welcome! This should be a fun ride today for you and I. So in this following report we will be exploring the Gapminder dataset...however, I will have no idea what you will end up seeing.

Why?

``` r
library("random")
```

That's why. I will be using the *random* package to make this interesting. Let's explore the relationship between life expectancy as a fucntion of GDP per capita. (*You will need internet connection to run some of this code. Especially pertaining to the **random** functions.*)

Initially, let us see what we are working with before any *<sub>CrA</sub>* *Z* *~<sub>~</sub> neSS ~<sub>~</sub>*.

``` r
str(gapminder)  #Exploring tbl.df of gapminder
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

Expectedly, this dataset a tbl = tibble dataframe. Buckets of observations of the six tracked variables: country, continent, year, life expectancy, population, GDP per capita. We can also see there were 152 survey countries on all **five** continents. *<sub>looking</sub> <sub>at</sub> <sub>you</sub> **<sub>ANTarctica</sub>***

Investigation of Socio-Ecomonics in Gapminder
---------------------------------------------

Now let's look at the exact same parameters as before and dive into how GDP and life expectancy correlate. <sub>Now</sub> <sub>with</sub> <sub>ggplots</sub> <sub>and</sub> <sub>dplyr</sub>

``` r
p_lvg <- gapminder %>% 
  ggplot(aes(x = log10(gdpPercap), y = log10(lifeExp))) #Making plot space

p_lvg + geom_point() + geom_smooth(lwd = 1, se = F, method = "lm")  #Assigning points and a trendline
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/Plot%20L%20v.%20G%20data-1.png)

~~ Pearson's Correlation Coefficent & Coefficent of Determination~~

``` r
r_p <- with(gapminder, cor(log10(gdpPercap), log10(lifeExp))) #Assigning object that is the P.C.C. of lifeExp and GDP

print(r_p) #Pearson's C.C. (P.C.C.)
```

    ## [1] 0.7830724

``` r
r_p^2 #Coefficent of determination
```

    ## [1] 0.6132023

As seen above, there is some correlation between life expectancy and GDP per capita. Perhaps this is trivial becasue as GDP per capita increases, it could be said that quality of life also increases and thus, perhaps life expectancy. Although this clearly does not paint the complete picture (R = 0.783, (R<sup>2</sup>) = 0.613) as the other factors will affect both that may or may not affect the other.

``` r
plot(lifeExp ~ continent, data = gapminder)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
plot(log10(gdpPercap) ~ continent, data = gapminder)
```

![](hw02_Exploring-Gapminder-with-tidyverse_files/figure-markdown_github/unnamed-chunk-1-2.png)

The above trends may reflect the socio-economic status of a continent as life expectancy can be seen to corelate with GDP. Let's take a look to see if this trend holds up for different places. Let us make an abitrary decision on which countries to investigate.

``` r
country_names <- unique(gapminder['country'])

cnm <- grep(
  as.list(randomStrings(n=1,len=1,digits=F)), 
  as.matrix(country_names[,1]))

name.list <- country_names[cnm,]

print(name.list)
```

    ## # A tibble: 36 × 1
    ##                     country
    ##                      <fctr>
    ## 1                 Australia
    ## 2                   Austria
    ## 3                   Belgium
    ## 4                  Bulgaria
    ## 5              Burkina Faso
    ## 6                   Burundi
    ## 7  Central African Republic
    ## 8                      Cuba
    ## 9            Czech Republic
    ## 10                 Djibouti
    ## # ... with 26 more rows

``` r
new_gp <- gapminder %>% 
  filter(country %in% c(as.matrix(name.list)))

new_gp_data <- new_gp %>% 
  select(lifeExp, gdpPercap)
  
print(new_gp)
```

    ## # A tibble: 432 × 6
    ##      country continent  year lifeExp      pop gdpPercap
    ##       <fctr>    <fctr> <int>   <dbl>    <int>     <dbl>
    ## 1  Australia   Oceania  1952   69.12  8691212  10039.60
    ## 2  Australia   Oceania  1957   70.33  9712569  10949.65
    ## 3  Australia   Oceania  1962   70.93 10794968  12217.23
    ## 4  Australia   Oceania  1967   71.10 11872264  14526.12
    ## 5  Australia   Oceania  1972   71.93 13177000  16788.63
    ## 6  Australia   Oceania  1977   73.49 14074100  18334.20
    ## 7  Australia   Oceania  1982   74.74 15184200  19477.01
    ## 8  Australia   Oceania  1987   76.32 16257249  21888.89
    ## 9  Australia   Oceania  1992   77.56 17481977  23424.77
    ## 10 Australia   Oceania  1997   78.83 18565243  26997.94
    ## # ... with 422 more rows

%&gt;% pipe operator fucn(arg1,arg2...) v. arg1 %&gt;% func(arg2....)
=====================================================================

\`\`\`
