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
# percent of total calories that are animal protein
data1$PercentCal <- data1$Share.of.calories.from.animal.protein..FAO..2017..
# GDP per capita
data1$GDPperCap <- data1$GDP.per.capita
# GDP
data1$GDP <- data1$GDP.per.capita*data1$Total.population..Gapminder..HYDE...UN.
# total population
data1$Pop <- data1$Total.population..Gapminder..HYDE...UN.
# redefining data
data2 <- data1[,-c(1,4,5,6,7)]
str(data2)
```

    ## 'data.frame':    51452 obs. of  8 variables:
    ##  $ Code      : Factor w/ 287 levels "","ABW","AFG",..: 175 3 3 3 3 3 3 3 3 3 ...
    ##  $ Year      : int  2015 1961 1962 1963 1964 1965 1966 1967 1968 1969 ...
    ##  $ Continent : chr  "Asia" "" "" "" ...
    ##  $ Country   : Factor w/ 304 levels "Abkhazia","Afghanistan",..: 1 2 2 2 2 2 2 2 2 2 ...
    ##  $ PercentCal: num  NA 1.8 1.85 2.11 1.94 ...
    ##  $ GDPperCap : num  NA 1309 1302 1298 1291 ...
    ##  $ GDP       : num  NA 1.20e+10 1.22e+10 1.24e+10 1.26e+10 ...
    ##  $ Pop       : num  NA 9169000 9351000 9543000 9745000 ...

### Removing Missing Values (NA)

``` r
data3 <- na.omit(data2)
```

### Isolating Data to Countries of Interest

``` r
data4 <- subset(data3, Country == "Gabon" | Country == "Botswana" | Country == "Mauritius" | Country == "Kuwait" | Country == "United Arab Emirates" | Country == "Saudi Arabia" | Country == "Norway" | Country == "Switzerland" | Country == "Ireland" | Country == "United States" | Country == "Canada" | Country == "Trinidad and Tobago" | Country == "Venezuela" | Country == "Chile" | Country == "Argentina" | Country == "Hong Kong" | Country == "Taiwan" | Country == "Japan")
data4$Country <- factor(data4$Country)
table(data4$Country)
```

    ## 
    ##            Argentina             Botswana               Canada 
    ##                   53                   53                   53 
    ##                Chile                Gabon            Hong Kong 
    ##                   53                   53                   53 
    ##              Ireland                Japan               Kuwait 
    ##                   53                   53                   53 
    ##            Mauritius               Norway         Saudi Arabia 
    ##                   53                   53                   53 
    ##          Switzerland               Taiwan  Trinidad and Tobago 
    ##                   53                   53                   53 
    ## United Arab Emirates        United States            Venezuela 
    ##                   53                   53                   53

### Assigning a Continent to Each Measurement

``` r
data5 <- data4
# Adding Africa
data5$Continent[data5$Country == "Botswana" | data5$Country == "Mauritius" | data5$Country == "Gabon"] <- "Africa"
# Adding Middle East
data5$Continent[data5$Country == "United Arab Emirates" | data5$Country == "Kuwait" | data5$Country == "Saudi Arabia"] <- "Middle East"
# Adding Europe
data5$Continent[data5$Country == "Ireland" | data5$Country == "Norway" | data5$Country == "Switzerland"] <- "Europe"
# Adding North America
data5$Continent[data5$Country == "United States" | data5$Country == "Canada" | data5$Country == "Trinidad and Tobago"] <- "North America"
# Adding South America
data5$Continent[data5$Country == "Argentina" | data5$Country == "Chile" | data5$Country == "Venezuela"] <- "South America"
# Adding Eastern Asia
data5$Continent[data5$Country == "Hong Kong" | data5$Country == "Taiwan" | data5$Country == "Japan"] <- "Eastern Asia"

# convert Continent to factor variable
data5$Continent <- factor(data5$Continent)
table(data5$Continent)
```

    ## 
    ##        Africa  Eastern Asia        Europe   Middle East North America 
    ##           159           159           159           159           159 
    ## South America 
    ##           159

### Final Data (as of 5/1)

``` r
FinalData <- data5
str(FinalData)
```

    ## 'data.frame':    954 obs. of  8 variables:
    ##  $ Code      : Factor w/ 287 levels "","ABW","AFG",..: 11 11 11 11 11 11 11 11 11 11 ...
    ##  $ Year      : int  1961 1962 1963 1964 1965 1966 1967 1968 1969 1970 ...
    ##  $ Continent : Factor w/ 6 levels "Africa","Eastern Asia",..: 6 6 6 6 6 6 6 6 6 6 ...
    ##  $ Country   : Factor w/ 18 levels "Argentina","Botswana",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ PercentCal: num  8.42 8.58 8.74 7.37 7.4 ...
    ##  $ GDPperCap : num  9344 9049 8695 9446 10155 ...
    ##  $ GDP       : num  1.95e+11 1.91e+11 1.87e+11 2.06e+11 2.25e+11 ...
    ##  $ Pop       : num  20817000 21153000 21489000 21824000 22160000 ...

``` r
dim(FinalData)
```

    ## [1] 954   8
