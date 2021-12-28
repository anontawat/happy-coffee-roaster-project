Observing Max Average Coffee Scores (2017)
================
Anontawat Wiriyakijja
December 28, 2021

This notebook is a part of Google Data Analytics Professional
Certificate case study. The notebook showcases of how to use R to
manipulate data and answer a business question.

**Question:** What are the top 3 countries that produce the highest
average cupping scores?

Firstly, import R packages “tidyverse”, “ggplot2”, “dplyr”, and
“janitor”.

``` r
library("tidyverse")
```

    ## Warning: package 'tidyverse' was built under R version 4.1.2

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.1.0     v forcats 0.5.1

    ## Warning: package 'ggplot2' was built under R version 4.1.2

    ## Warning: package 'tibble' was built under R version 4.1.2

    ## Warning: package 'tidyr' was built under R version 4.1.2

    ## Warning: package 'readr' was built under R version 4.1.2

    ## Warning: package 'purrr' was built under R version 4.1.2

    ## Warning: package 'stringr' was built under R version 4.1.2

    ## Warning: package 'forcats' was built under R version 4.1.2

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library("ggplot2")
library("dplyr")
library("janitor")
```

    ## Warning: package 'janitor' was built under R version 4.1.2

    ## 
    ## Attaching package: 'janitor'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

The next step is to load in the dataframe and explore it.

``` r
arabica_quality <- read_csv("arabica_quality_data.csv")
```

    ## New names:
    ## * `` -> ...1

    ## Rows: 1311 Columns: 44

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (24): Species, Owner, Country.of.Origin, Farm.Name, Lot.Number, Mill, IC...
    ## dbl (20): ...1, Number.of.Bags, Aroma, Flavor, Aftertaste, Acidity, Body, Ba...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
summary(arabica_quality)
```

    ##       ...1          Species             Owner           Country.of.Origin 
    ##  Min.   :   1.0   Length:1311        Length:1311        Length:1311       
    ##  1st Qu.: 328.5   Class :character   Class :character   Class :character  
    ##  Median : 656.0   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   : 656.0                                                           
    ##  3rd Qu.: 983.5                                                           
    ##  Max.   :1312.0                                                           
    ##                                                                           
    ##   Farm.Name          Lot.Number            Mill            ICO.Number       
    ##  Length:1311        Length:1311        Length:1311        Length:1311       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    Company            Altitude            Region            Producer        
    ##  Length:1311        Length:1311        Length:1311        Length:1311       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  Number.of.Bags    Bag.Weight        In.Country.Partner Harvest.Year      
    ##  Min.   :   0.0   Length:1311        Length:1311        Length:1311       
    ##  1st Qu.:  14.5   Class :character   Class :character   Class :character  
    ##  Median : 175.0   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   : 153.9                                                           
    ##  3rd Qu.: 275.0                                                           
    ##  Max.   :1062.0                                                           
    ##                                                                           
    ##  Grading.Date         Owner.1            Variety          Processing.Method 
    ##  Length:1311        Length:1311        Length:1311        Length:1311       
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##      Aroma           Flavor        Aftertaste       Acidity     
    ##  Min.   :0.000   Min.   :0.000   Min.   :0.000   Min.   :0.000  
    ##  1st Qu.:7.420   1st Qu.:7.330   1st Qu.:7.250   1st Qu.:7.330  
    ##  Median :7.580   Median :7.580   Median :7.420   Median :7.500  
    ##  Mean   :7.564   Mean   :7.518   Mean   :7.398   Mean   :7.533  
    ##  3rd Qu.:7.750   3rd Qu.:7.750   3rd Qu.:7.580   3rd Qu.:7.750  
    ##  Max.   :8.750   Max.   :8.830   Max.   :8.670   Max.   :8.750  
    ##                                                                 
    ##       Body          Balance        Uniformity       Clean.Cup     
    ##  Min.   :0.000   Min.   :0.000   Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.:7.330   1st Qu.:7.330   1st Qu.:10.000   1st Qu.:10.000  
    ##  Median :7.500   Median :7.500   Median :10.000   Median :10.000  
    ##  Mean   :7.518   Mean   :7.518   Mean   : 9.833   Mean   : 9.833  
    ##  3rd Qu.:7.670   3rd Qu.:7.750   3rd Qu.:10.000   3rd Qu.:10.000  
    ##  Max.   :8.580   Max.   :8.750   Max.   :10.000   Max.   :10.000  
    ##                                                                   
    ##    Sweetness      Cupper.Points    Total.Cup.Points    Moisture      
    ##  Min.   : 0.000   Min.   : 0.000   Min.   : 0.00    Min.   :0.00000  
    ##  1st Qu.:10.000   1st Qu.: 7.250   1st Qu.:81.17    1st Qu.:0.09000  
    ##  Median :10.000   Median : 7.500   Median :82.50    Median :0.11000  
    ##  Mean   : 9.903   Mean   : 7.498   Mean   :82.12    Mean   :0.08886  
    ##  3rd Qu.:10.000   3rd Qu.: 7.750   3rd Qu.:83.67    3rd Qu.:0.12000  
    ##  Max.   :10.000   Max.   :10.000   Max.   :90.58    Max.   :0.28000  
    ##                                                                      
    ##  Category.One.Defects    Quakers           Color           Category.Two.Defects
    ##  Min.   : 0.0000      Min.   : 0.0000   Length:1311        Min.   : 0.000      
    ##  1st Qu.: 0.0000      1st Qu.: 0.0000   Class :character   1st Qu.: 0.000      
    ##  Median : 0.0000      Median : 0.0000   Mode  :character   Median : 2.000      
    ##  Mean   : 0.4264      Mean   : 0.1771                      Mean   : 3.592      
    ##  3rd Qu.: 0.0000      3rd Qu.: 0.0000                      3rd Qu.: 4.000      
    ##  Max.   :31.0000      Max.   :11.0000                      Max.   :55.000      
    ##                       NA's   :1                                                
    ##   Expiration        Certification.Body Certification.Address
    ##  Length:1311        Length:1311        Length:1311          
    ##  Class :character   Class :character   Class :character     
    ##  Mode  :character   Mode  :character   Mode  :character     
    ##                                                             
    ##                                                             
    ##                                                             
    ##                                                             
    ##  Certification.Contact unit_of_measurement altitude_low_meters
    ##  Length:1311           Length:1311         Min.   :     1     
    ##  Class :character      Class :character    1st Qu.:  1100     
    ##  Mode  :character      Mode  :character    Median :  1311     
    ##                                            Mean   :  1760     
    ##                                            3rd Qu.:  1600     
    ##                                            Max.   :190164     
    ##                                            NA's   :227        
    ##  altitude_high_meters altitude_mean_meters
    ##  Min.   :     1       Min.   :     1      
    ##  1st Qu.:  1100       1st Qu.:  1100      
    ##  Median :  1350       Median :  1311      
    ##  Mean   :  1809       Mean   :  1784      
    ##  3rd Qu.:  1650       3rd Qu.:  1600      
    ##  Max.   :190164       Max.   :190164      
    ##  NA's   :227          NA's   :227

Next one is to calculate the percentages of each column missing values.

``` r
missing_percentage <- arabica_quality %>% 
  summarise_all(list(name = ~sum(is.na(.)) / length(.) * 100))

#Use pivot_longer to better show the result in longer format
missing_percentage <- pivot_longer(round(missing_percentage, 2), cols = colnames(missing_percentage), names_to = "col_name", values_to = "percentage")

#Sort the values in descending order
missing_percentage[order(missing_percentage$percentage, decreasing = TRUE), ]
```

    ## # A tibble: 44 x 2
    ##    col_name                  percentage
    ##    <chr>                          <dbl>
    ##  1 Lot.Number_name                 79.4
    ##  2 Farm.Name_name                  27.2
    ##  3 Mill_name                       23.4
    ##  4 Producer_name                   17.5
    ##  5 altitude_low_meters_name        17.3
    ##  6 altitude_high_meters_name       17.3
    ##  7 altitude_mean_meters_name       17.3
    ##  8 Altitude_name                   17.0
    ##  9 Color_name                      16.5
    ## 10 Company_name                    15.9
    ## # ... with 34 more rows

The column **Lot.Number** can be entirely dropped due to the very high
percentage of missing values.

The dataframe is also needed to be sliced in order to get only the
following columns. These column names are also needed to be cleaned.

1.  Owner
2.  Country.of.Origin
3.  Farm.Name
4.  Altitude
5.  Region
6.  Producer
7.  Variety
8.  Processing.Method
9.  Aroma
10. Flavor
11. Aftertaste
12. Acidity
13. Body
14. Balance
15. Uniformity
16. Clean.Cup
17. Sweetness
18. Cupper.Points
19. Total.Cup.Points

``` r
#Clean column names with backing up the data
arabica_col_cleaned <- clean_names(arabica_quality)
head(arabica_col_cleaned)
```

    ## # A tibble: 6 x 44
    ##      x1 species owner   country_of_orig~ farm_name   lot_number mill  ico_number
    ##   <dbl> <chr>   <chr>   <chr>            <chr>       <chr>      <chr> <chr>     
    ## 1     1 Arabica metad ~ Ethiopia         "metad plc" <NA>       meta~ 2014/2015 
    ## 2     2 Arabica metad ~ Ethiopia         "metad plc" <NA>       meta~ 2014/2015 
    ## 3     3 Arabica ground~ Guatemala        "san marco~ <NA>       <NA>  <NA>      
    ## 4     4 Arabica yidnek~ Ethiopia         "yidnekach~ <NA>       wole~ <NA>      
    ## 5     5 Arabica metad ~ Ethiopia         "metad plc" <NA>       meta~ 2014/2015 
    ## 6     6 Arabica ji-ae ~ Brazil            <NA>       <NA>       <NA>  <NA>      
    ## # ... with 36 more variables: company <chr>, altitude <chr>, region <chr>,
    ## #   producer <chr>, number_of_bags <dbl>, bag_weight <chr>,
    ## #   in_country_partner <chr>, harvest_year <chr>, grading_date <chr>,
    ## #   owner_1 <chr>, variety <chr>, processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>, moisture <dbl>, category_one_defects <dbl>, ...

``` r
#Slice columns to only get the columns above
arabica_final_cols <- arabica_col_cleaned[, c("owner", "country_of_origin", "farm_name", "altitude", "region", "producer", "variety", "processing_method", "aroma", "flavor", "aftertaste", "acidity", "body", "balance", "uniformity", "clean_cup", "sweetness", "cupper_points", "total_cup_points")]

head(arabica_final_cols)
```

    ## # A tibble: 6 x 19
    ##   owner    country_of_origin farm_name      altitude region producer     variety
    ##   <chr>    <chr>             <chr>          <chr>    <chr>  <chr>        <chr>  
    ## 1 metad p~ Ethiopia          "metad plc"    1950-22~ guji-~ METAD PLC    <NA>   
    ## 2 metad p~ Ethiopia          "metad plc"    1950-22~ guji-~ METAD PLC    Other  
    ## 3 grounds~ Guatemala         "san marcos b~ 1600 - ~ <NA>   <NA>         Bourbon
    ## 4 yidneka~ Ethiopia          "yidnekachew ~ 1800-22~ oromia Yidnekachew~ <NA>   
    ## 5 metad p~ Ethiopia          "metad plc"    1950-22~ guji-~ METAD PLC    Other  
    ## 6 ji-ae a~ Brazil             <NA>          <NA>     <NA>   <NA>         <NA>   
    ## # ... with 12 more variables: processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

Check for the number of missing values again in each columns.

``` r
colSums(is.na(arabica_final_cols))  
```

    ##             owner country_of_origin         farm_name          altitude 
    ##                 7                 1               356               223 
    ##            region          producer           variety processing_method 
    ##                57               229               201               152 
    ##             aroma            flavor        aftertaste           acidity 
    ##                 0                 0                 0                 0 
    ##              body           balance        uniformity         clean_cup 
    ##                 0                 0                 0                 0 
    ##         sweetness     cupper_points  total_cup_points 
    ##                 0                 0                 0

It seems that there are so many missing values here for string columns,
however the numeric columns such as **total_cup_points** and the others
have no missing values.

Next up, observe the **owner** column first to decide what to do with
the NAs.

``` r
owner_na <- arabica_final_cols[is.na(arabica_final_cols$owner),]

owner_na
```

    ## # A tibble: 7 x 19
    ##   owner country_of_origin farm_name      altitude   region  producer     variety
    ##   <chr> <chr>             <chr>          <chr>      <chr>   <chr>        <chr>  
    ## 1 <NA>  Honduras          los hicaques   1350       comaya~ Reynerio Ze~ Caturra
    ## 2 <NA>  Honduras          los hicaques   1350       comaya~ Reynerio Ze~ Caturra
    ## 3 <NA>  Colombia          supply chain ~ 1400 thru~ south ~ Supply Chai~ Caturra
    ## 4 <NA>  Honduras          gran manzana ~ 1350       comaya~ Tomás Sosa,~ Caturra
    ## 5 <NA>  Honduras          gran manzana ~ 1400       comaya~ Tomás Sosa,~ Caturra
    ## 6 <NA>  Honduras          los hicaques   1450 mals  centra~ Reinerio Ze~ Catuai 
    ## 7 <NA>  Honduras          los hicaques   1450 mals  centra~ Reinerio Ze~ Catuai 
    ## # ... with 12 more variables: processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

All of the missing owner values are in **Honduras** and **Colombia**.

Therefore, dig more into **Honduras** country to see if those NAs can be
filled with some information by extracting the Honduras out from
**arabica_final_cols**. Then filter only the NAs in the Honduras.

``` r
honduras <- arabica_final_cols %>% 
  filter(country_of_origin == "Honduras")
honduras[is.na(honduras$owner),]
```

    ## # A tibble: 6 x 19
    ##   owner country_of_origin farm_name      altitude  region   producer     variety
    ##   <chr> <chr>             <chr>          <chr>     <chr>    <chr>        <chr>  
    ## 1 <NA>  Honduras          los hicaques   1350      comayag~ Reynerio Ze~ Caturra
    ## 2 <NA>  Honduras          los hicaques   1350      comayag~ Reynerio Ze~ Caturra
    ## 3 <NA>  Honduras          gran manzana ~ 1350      comayag~ Tomás Sosa,~ Caturra
    ## 4 <NA>  Honduras          gran manzana ~ 1400      comayag~ Tomás Sosa,~ Caturra
    ## 5 <NA>  Honduras          los hicaques   1450 mals central~ Reinerio Ze~ Catuai 
    ## 6 <NA>  Honduras          los hicaques   1450 mals central~ Reinerio Ze~ Catuai 
    ## # ... with 12 more variables: processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

We can see that the missing owner values are from either “**los
hicaques**” or “**gran manzana y el aguacate**” farm so we will filter
out only all row in those 2 farms in Honduras.

``` r
hon_farm <- c("los hicaques", "gran manzana y el aguacate")
filter(honduras, farm_name %in% hon_farm)
```

    ## # A tibble: 13 x 19
    ##    owner           country_of_origin farm_name  altitude region producer variety
    ##    <chr>           <chr>             <chr>      <chr>    <chr>  <chr>    <chr>  
    ##  1 bismarck castro Honduras          los hicaq~ 1400     comay~ Reineri~ Caturra
    ##  2 bismarck castro Honduras          los hicaq~ 1500     centr~ Reineri~ Catuai 
    ##  3 bismarck castro Honduras          los hicaq~ 1400     comay~ Reineri~ Caturra
    ##  4 <NA>            Honduras          los hicaq~ 1350     comay~ Reyneri~ Caturra
    ##  5 <NA>            Honduras          los hicaq~ 1350     comay~ Reyneri~ Caturra
    ##  6 bismarck castro Honduras          los hicaq~ 1400     comay~ Reineri~ Caturra
    ##  7 bismarck castro Honduras          los hicaq~ 1396     comay~ Reineri~ Caturra
    ##  8 bismarck castro Honduras          los hicaq~ 1400     comay~ Reineri~ Caturra
    ##  9 <NA>            Honduras          gran manz~ 1350     comay~ Tomás S~ Caturra
    ## 10 <NA>            Honduras          gran manz~ 1400     comay~ Tomás S~ Caturra
    ## 11 <NA>            Honduras          los hicaq~ 1450 ma~ centr~ Reineri~ Catuai 
    ## 12 <NA>            Honduras          los hicaq~ 1450 ma~ centr~ Reineri~ Catuai 
    ## 13 bismarck castro Honduras          los hicaq~ 1400     comay~ Reineri~ Caturra
    ## # ... with 12 more variables: processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

It seems that those missing values from owner can be all filled in with
the information from the same farm_name.

``` r
#Fill in NAs of los hicaques with bismarck castro
arabica_final_cols$owner[is.na(arabica_final_cols$owner) & arabica_final_cols$farm_name == "los hicaques"] <- "bismarck castro"

#Fill in NAs of gran manzana y el aguacate farm with Tomás Sosa, Juan Damaso
arabica_final_cols$owner[is.na(arabica_final_cols$owner) & arabica_final_cols$farm_name == "gran manzana y el aguacate"] <- "Tomás Sosa, Juan Damaso"

#Check if the owner of Honduras still contains NA?
subset(arabica_final_cols, country_of_origin == "Honduras" & is.na(owner) == TRUE)
```

    ## # A tibble: 0 x 19
    ## # ... with 19 variables: owner <chr>, country_of_origin <chr>, farm_name <chr>,
    ## #   altitude <chr>, region <chr>, producer <chr>, variety <chr>,
    ## #   processing_method <chr>, aroma <dbl>, flavor <dbl>, aftertaste <dbl>,
    ## #   acidity <dbl>, body <dbl>, balance <dbl>, uniformity <dbl>,
    ## #   clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

Check for **Colombia** country where the farm_name is “**supply chain
ecom cca s.a.**”

``` r
subset(arabica_final_cols, farm_name == "supply chain ecom cca s.a.")
```

    ## # A tibble: 1 x 19
    ##   owner country_of_origin farm_name      altitude   region  producer     variety
    ##   <chr> <chr>             <chr>          <chr>      <chr>   <chr>        <chr>  
    ## 1 <NA>  Colombia          supply chain ~ 1400 thru~ south ~ Supply Chai~ Caturra
    ## # ... with 12 more variables: processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

There is only 1 row so I will fill in the NA of the owner column with
the producer name.

``` r
arabica_final_cols$owner[is.na(arabica_final_cols$owner) & arabica_final_cols$farm_name == "supply chain ecom cca s.a."] <- "Supply Chain ECOM CCA S.A."

#Check total NA in the owner column
sum(is.na(arabica_final_cols$owner))
```

    ## [1] 0

Move to the **country_of_origin** column that contains 1 missing value.

``` r
arabica_final_cols[is.na(arabica_final_cols$country_of_origin),]
```

    ## # A tibble: 1 x 19
    ##   owner              country_of_orig~ farm_name altitude region producer variety
    ##   <chr>              <chr>            <chr>     <chr>    <chr>  <chr>    <chr>  
    ## 1 racafe & cia s.c.a <NA>             <NA>      <NA>     <NA>   <NA>     <NA>   
    ## # ... with 12 more variables: processing_method <chr>, aroma <dbl>,
    ## #   flavor <dbl>, aftertaste <dbl>, acidity <dbl>, body <dbl>, balance <dbl>,
    ## #   uniformity <dbl>, clean_cup <dbl>, sweetness <dbl>, cupper_points <dbl>,
    ## #   total_cup_points <dbl>

We can only use “**racafe & cia s.c.a**” from the owner column to try to
fill in its **country_of_origin** column.

After searching the owner name, we found that it is from **Colombia** so
we can use it to fill in the country.

``` r
#Fill in the only NA in country_of_origin column with Colombia
arabica_final_cols$country_of_origin[is.na(arabica_final_cols$country_of_origin)] <- "Colombia"

#Confirm that country_of_origin contains no missing value
sum(is.na(arabica_final_cols$country_of_origin))
```

    ## [1] 0

We have already cleaned two columns but there are still more missing
values in the other columns. However, they will not affect our analysis
except that the stakeholders still need to know more information about
those beans (only if the end result show the rows that contained with
missing values).

Next up, we will check if the **total_cup_points** are summed up
correctly or not.

``` r
#Sum up the numbers from aroma column to cupper_points column and compare it with the total_cup_points column.
check_total_points <- arabica_final_cols %>% 
  mutate(correct_total_points = select(., aroma:cupper_points) %>% 
           rowSums(na.rm = TRUE))

head(check_total_points[, c("total_cup_points", "correct_total_points")], n = 10)
```

    ## # A tibble: 10 x 2
    ##    total_cup_points correct_total_points
    ##               <dbl>                <dbl>
    ##  1             90.6                 90.6
    ##  2             89.9                 89.9
    ##  3             89.8                 89.8
    ##  4             89                   89.0
    ##  5             88.8                 88.8
    ##  6             88.8                 88.8
    ##  7             88.8                 88.8
    ##  8             88.7                 88.7
    ##  9             88.4                 88.4
    ## 10             88.2                 88.2

The **total_points** are accurate with some differs less than 0.01
points so this column can be used.

Next, we will drop from **aroma** column to **cupper_points** column as
we will not need them for the analysis process.

``` r
arabica_final_cols_sliced <- arabica_final_cols[, c("owner", "country_of_origin", "farm_name", "altitude", "region", "producer", "variety", "processing_method", "total_cup_points")]

head(arabica_final_cols_sliced)
```

    ## # A tibble: 6 x 9
    ##   owner    country_of_origin farm_name      altitude region producer     variety
    ##   <chr>    <chr>             <chr>          <chr>    <chr>  <chr>        <chr>  
    ## 1 metad p~ Ethiopia          "metad plc"    1950-22~ guji-~ METAD PLC    <NA>   
    ## 2 metad p~ Ethiopia          "metad plc"    1950-22~ guji-~ METAD PLC    Other  
    ## 3 grounds~ Guatemala         "san marcos b~ 1600 - ~ <NA>   <NA>         Bourbon
    ## 4 yidneka~ Ethiopia          "yidnekachew ~ 1800-22~ oromia Yidnekachew~ <NA>   
    ## 5 metad p~ Ethiopia          "metad plc"    1950-22~ guji-~ METAD PLC    Other  
    ## 6 ji-ae a~ Brazil             <NA>          <NA>     <NA>   <NA>         <NA>   
    ## # ... with 2 more variables: processing_method <chr>, total_cup_points <dbl>

Check if there is any incorrect or outlier points in the
**total_cup_points** column against the **country_of_origin** column.

``` r
#Use boxplot to identify the outliers 
ggplot(arabica_final_cols_sliced, aes(x = country_of_origin, y = total_cup_points)) +
  geom_boxplot() +
  theme(axis.text.x = element_text(angle = 90))
```

![](2017_world_coffee_scores_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

It seems that there is one incorrect point in the **total_cup_points**
column so, we will observe it.

``` r
unique(arabica_final_cols_sliced$total_cup_points)
```

    ##   [1] 90.58 89.92 89.75 89.00 88.83 88.75 88.67 88.42 88.25 88.08 87.92 87.83
    ##  [13] 87.58 87.42 87.33 87.25 87.17 87.08 86.92 86.83 86.67 86.58 86.50 86.42
    ##  [25] 86.33 86.25 86.17 86.08 86.00 85.92 85.83 85.75 85.58 85.50 85.42 85.33
    ##  [37] 85.25 85.17 85.08 85.00 84.92 84.83 84.75 84.67 84.58 84.50 84.42 84.33
    ##  [49] 84.25 84.17 84.13 84.08 84.00 83.92 83.83 83.75 83.67 83.58 83.50 83.42
    ##  [61] 83.38 83.33 83.25 83.17 83.08 83.00 82.92 82.83 82.75 82.67 82.58 82.50
    ##  [73] 82.42 82.33 82.25 82.17 82.08 82.00 81.92 81.83 81.75 81.67 81.58 81.50
    ##  [85] 81.42 81.33 81.25 81.17 81.08 81.00 80.92 80.83 80.75 80.67 80.58 80.50
    ##  [97] 80.42 80.33 80.25 80.17 80.08 80.00 79.92 79.83 79.75 79.67 79.58 79.50
    ## [109] 79.42 79.33 79.25 79.17 79.08 79.00 78.92 78.83 78.75 78.67 78.58 78.50
    ## [121] 78.42 78.33 78.25 78.17 78.08 78.00 77.92 77.83 77.67 77.58 77.50 77.42
    ## [133] 77.33 77.25 77.17 77.00 76.83 76.75 76.50 76.42 76.33 76.25 76.17 76.08
    ## [145] 76.00 75.83 75.67 75.58 75.50 75.17 75.00 74.92 74.83 74.75 74.67 74.42
    ## [157] 74.33 74.00 73.83 73.67 73.50 73.42 72.92 72.83 72.58 72.33 71.75 71.08
    ## [169] 71.00 70.75 70.67 69.33 69.17 68.33 67.92 63.08 59.83  0.00

Notice that there is **0.00** score in the last row, but I need to check
if the other score columns can be used to help in calculation or not.

``` r
arabica_quality[arabica_quality$Total.Cup.Points == 0.00, ]
```

    ## # A tibble: 1 x 44
    ##    ...1 Species Owner   Country.of.Origin Farm.Name Lot.Number Mill   ICO.Number
    ##   <dbl> <chr>   <chr>   <chr>             <chr>     <chr>      <chr>  <chr>     
    ## 1  1312 Arabica bismar~ Honduras          los hica~ 103        cigra~ 13-111-053
    ## # ... with 36 more variables: Company <chr>, Altitude <chr>, Region <chr>,
    ## #   Producer <chr>, Number.of.Bags <dbl>, Bag.Weight <chr>,
    ## #   In.Country.Partner <chr>, Harvest.Year <chr>, Grading.Date <chr>,
    ## #   Owner.1 <chr>, Variety <chr>, Processing.Method <chr>, Aroma <dbl>,
    ## #   Flavor <dbl>, Aftertaste <dbl>, Acidity <dbl>, Body <dbl>, Balance <dbl>,
    ## #   Uniformity <dbl>, Clean.Cup <dbl>, Sweetness <dbl>, Cupper.Points <dbl>,
    ## #   Total.Cup.Points <dbl>, Moisture <dbl>, Category.One.Defects <dbl>, ...

Those all scores columns are also 0 and we could not find other
information on the Internet so we will drop this row out.

``` r
#Get the row index first
which(arabica_final_cols_sliced$total_cup_points == 0.00,) #This will result at 1311 index
```

    ## [1] 1311

``` r
#Remove that row
arabica_final_cols_sliced <- arabica_final_cols_sliced[-c(1311), ]

#Confirm again if it has already been removed by checking the min value of the column
min(arabica_final_cols_sliced$total_cup_points)
```

    ## [1] 59.83

Now, we are ready to answer the question by aggregating the data.

``` r
arabica_quality_analysed <- arabica_final_cols_sliced %>% 
  group_by(country_of_origin) %>%
  summarise(across(total_cup_points, mean, na.rm = TRUE))

final_answer <- arabica_quality_analysed %>% 
  arrange(desc(total_cup_points))

colnames(final_answer) <- c("country", "average_cupping_scores")

head(final_answer, n = 3)
```

    ## # A tibble: 3 x 2
    ##   country          average_cupping_scores
    ##   <chr>                             <dbl>
    ## 1 United States                      86.0
    ## 2 Papua New Guinea                   85.8
    ## 3 Ethiopia                           85.5

It surprises us that the highest average cupping score is from the US
instead of Colombia or Ethiopia which are well-known countries for
producing high quality coffee, so it is worth to observe more about
coffee in the US.

``` r
arabica_final_cols_sliced[arabica_final_cols_sliced$country_of_origin == "United States", ]
```

    ## # A tibble: 8 x 9
    ##   owner     country_of_origin farm_name     altitude   region producer   variety
    ##   <chr>     <chr>             <chr>         <chr>      <chr>  <chr>      <chr>  
    ## 1 cqi q co~ United States     el filo       meters ab~ antio~ Alfredo D~ Other  
    ## 2 cqi q co~ United States     los cedros    meters ab~ antio~ Jorge Wal~ Other  
    ## 3 cqi q co~ United States     el águila     meters ab~ antio~ María Let~ Other  
    ## 4 cqi q co~ United States     el rodeo      meters ab~ antio~ Nicolás R~ Other  
    ## 5 cqi q co~ United States     la curva      meters ab~ antio~ Silvia El~ Other  
    ## 6 cqi q co~ United States     la primavera  meters ab~ antio~ Hugo Sepú~ Other  
    ## 7 joshua m~ United States     kampung keli~ 1200 m     beras~ PT. Olam ~ Other  
    ## 8 wali ali  United States     <NA>          5600 feet  centr~ JuanAna C~ Pache ~
    ## # ... with 2 more variables: processing_method <chr>, total_cup_points <dbl>

The result shows that the original beans are not from the US because the
farm name and most of the regions are in Colombia and the last two are
in Indonesia and Guatemala respectively. On the other hand, this also
tells us that the US is one of the top roasters in the World!

We will need to replace all the **United States** in the
**country_of_origin** with the countries listed above.

``` r
arabica_final_cols_sliced$country_of_origin[ arabica_final_cols_sliced$country_of_origin == "United States"] <- c("Colombia", "Colombia", "Colombia", "Colombia", "Colombia", "Colombia", "Indonesia", "Guatemala")

#Check if whether is there any other United States left or not?
table(arabica_final_cols_sliced$country_of_origin)
```

    ## 
    ##                       Brazil                      Burundi 
    ##                          132                            2 
    ##                        China                     Colombia 
    ##                           16                          190 
    ##                   Costa Rica                Cote d?Ivoire 
    ##                           51                            1 
    ##                      Ecuador                  El Salvador 
    ##                            1                           21 
    ##                     Ethiopia                    Guatemala 
    ##                           44                          182 
    ##                        Haiti                     Honduras 
    ##                            6                           52 
    ##                        India                    Indonesia 
    ##                            1                           21 
    ##                        Japan                        Kenya 
    ##                            1                           25 
    ##                         Laos                       Malawi 
    ##                            3                           11 
    ##                    Mauritius                       Mexico 
    ##                            1                          236 
    ##                      Myanmar                    Nicaragua 
    ##                            8                           26 
    ##                       Panama             Papua New Guinea 
    ##                            4                            1 
    ##                         Peru                  Philippines 
    ##                           10                            5 
    ##                       Rwanda                       Taiwan 
    ##                            1                           75 
    ## Tanzania, United Republic Of                     Thailand 
    ##                           40                           32 
    ##                       Uganda       United States (Hawaii) 
    ##                           26                           73 
    ##  United States (Puerto Rico)                      Vietnam 
    ##                            4                            7 
    ##                       Zambia 
    ##                            1

After observing the **country_of_origin** again, we also need to clean
some of the following country names to make them more clear:

1.  Tanzania, United Republic Of \<- Tanzania
2.  United States (Hawaii) \<- Hawaii
3.  United States (Puerto Rico) \<- Puerto Rico

``` r
#Change the country names above
arabica_final_cols_sliced$country_of_origin[arabica_final_cols_sliced$country_of_origin == "Tanzania, United Republic Of"] <- "Tanzania"

arabica_final_cols_sliced$country_of_origin[arabica_final_cols_sliced$country_of_origin == "United States (Hawaii)"] <- "Hawaii"

arabica_final_cols_sliced$country_of_origin[arabica_final_cols_sliced$country_of_origin == "United States (Puerto Rico)"] <- "Puerto Rico"
               
#Check again the country_of_origin column again
table(arabica_final_cols_sliced$country_of_origin)
```

    ## 
    ##           Brazil          Burundi            China         Colombia 
    ##              132                2               16              190 
    ##       Costa Rica    Cote d?Ivoire          Ecuador      El Salvador 
    ##               51                1                1               21 
    ##         Ethiopia        Guatemala            Haiti           Hawaii 
    ##               44              182                6               73 
    ##         Honduras            India        Indonesia            Japan 
    ##               52                1               21                1 
    ##            Kenya             Laos           Malawi        Mauritius 
    ##               25                3               11                1 
    ##           Mexico          Myanmar        Nicaragua           Panama 
    ##              236                8               26                4 
    ## Papua New Guinea             Peru      Philippines      Puerto Rico 
    ##                1               10                5                4 
    ##           Rwanda           Taiwan         Tanzania         Thailand 
    ##                1               75               40               32 
    ##           Uganda          Vietnam           Zambia 
    ##               26                7                1

We will group up the data with the **country_of_origin** again to see if
the result will be changed or not.

``` r
arabica_quality_analysed_2 <- arabica_final_cols_sliced %>%
  group_by(country_of_origin) %>%
  summarise(across(total_cup_points, mean, na.rm = TRUE))

final_answer_2 <- arabica_quality_analysed_2 %>% 
  arrange(desc(total_cup_points))

colnames(final_answer_2) <- c("country", "average_cupping_scores")

head(final_answer_2, n = 3)
```

    ## # A tibble: 3 x 2
    ##   country          average_cupping_scores
    ##   <chr>                             <dbl>
    ## 1 Papua New Guinea                   85.8
    ## 2 Ethiopia                           85.5
    ## 3 Japan                              84.7

The result has changed! We still need to keep in mind that some
countries sent in only 1 sample so we will add every country’s max score
in order to compare with their average scores and make it less bias.

``` r
arabica_quality_analysed_3 <- arabica_final_cols_sliced %>%
  group_by(country_of_origin) %>%
  summarise(average_cupping_scores = mean(total_cup_points, na.rm = TRUE), max_cupping_scores = max(total_cup_points, na.rm = TRUE))

final_answer_3 <- arabica_quality_analysed_3 %>% 
  arrange(country_of_origin)

colnames(final_answer_3)[1] <- "country"

#Round the numberof average_cupping_scores
final_answer_3$average_cupping_scores <- round(final_answer_3$average_cupping_scores, 2)

head(final_answer_3, n = 5)
```

    ## # A tibble: 5 x 3
    ##   country    average_cupping_scores max_cupping_scores
    ##   <chr>                       <dbl>              <dbl>
    ## 1 Brazil                       82.4               88.8
    ## 2 Burundi                      81.8               83.3
    ## 3 China                        82.9               87.2
    ## 4 Colombia                     83.2               87.9
    ## 5 Costa Rica                   82.8               87.2

Last step is to export this dataframe into a CSV file in order to
visualise it using Tableau.

``` r
write.csv(final_answer_3, file = 'C:\\Users\\medse\\OneDrive\\Desktop\\Google Data Analytics Cert\\Sample_Data\\Case_study_data\\2017_world_coffee_socres_analysed.csv', row.names = FALSE)
```
