Consulting Project
================

# Document for Data Cleaning

### Loading Data

``` r
data0 <- read.csv("/Users/hannavenera/Documents/School/Pacific Lutheran University/Classes/Classes 2020-2021/Spring 2021/STAT 348/Consulting Project/share-of-calories-from-animal-protein-vs-gdp-per-capita.csv", header = TRUE)
dim(data0)
```

    ## [1] 51452     8

``` r
str(data0)
```

    ## 'data.frame':    51452 obs. of  8 variables:
    ##  $ Entity                                            : chr  "Abkhazia" "Afghanistan" "Afghanistan" "Afghanistan" ...
    ##  $ Code                                              : chr  "OWID_ABK" "AFG" "AFG" "AFG" ...
    ##  $ Year                                              : int  2015 1961 1962 1963 1964 1965 1966 1967 1968 1969 ...
    ##  $ Share.of.calories.from.animal.protein..FAO..2017..: num  NA 1.8 1.85 2.11 1.94 ...
    ##  $ GDP.per.capita                                    : num  NA 1309 1302 1298 1291 ...
    ##  $ X145446.annotations                               : chr  "" "" "" "" ...
    ##  $ Total.population..Gapminder..HYDE...UN.           : num  NA 9169000 9351000 9543000 9745000 ...
    ##  $ Continent                                         : chr  "Asia" "" "" "" ...

### Converting Factor Variables and Fixing Variable Names

``` r
data1 <- data0
# changing entity to country
data1$Country <- factor(data1$Entity)
# country code
data1$Code <- factor(data1$Code)
# continent as factor
data1$Continent <- factor(data1$Continent)
# percent of total calories that are animal protein
data1$PercentCal <- data1$Share.of.calories.from.animal.protein..FAO..2017..
# GDP per capita
data1$GDP <- data1$GDP.per.capita
# total population
data1$Pop <- data1$Total.population..Gapminder..HYDE...UN.
# redefining data
data2 <- data1[,-c(1,4,5,6,7)]
str(data2)
```

    ## 'data.frame':    51452 obs. of  7 variables:
    ##  $ Code      : Factor w/ 287 levels "","ABW","AFG",..: 175 3 3 3 3 3 3 3 3 3 ...
    ##  $ Year      : int  2015 1961 1962 1963 1964 1965 1966 1967 1968 1969 ...
    ##  $ Continent : Factor w/ 8 levels "","Africa","Antarctica",..: 4 1 1 1 1 1 1 1 1 1 ...
    ##  $ Country   : Factor w/ 304 levels "Abkhazia","Afghanistan",..: 1 2 2 2 2 2 2 2 2 2 ...
    ##  $ PercentCal: num  NA 1.8 1.85 2.11 1.94 ...
    ##  $ GDP       : num  NA 1309 1302 1298 1291 ...
    ##  $ Pop       : num  NA 9169000 9351000 9543000 9745000 ...

### Assigning a Continent to Each Measurement

``` r
table(data1$Continent)
```

    ## 
    ##                      Africa    Antarctica          Asia        Europe 
    ##         51167            61             4            62            75 
    ## North America       Oceania South America 
    ##            42            26            15

``` r
# only country measurements in year 2015 have a continent category
```

### Removing Missing Values (NA)

### Isolating Data to Countries of Interest
