Andrew Whitman - Music Sales
================
Andrew Whitman
2022-10-04

This is an analysis of the music sales of *Andrew Whitman and the
Cousins* from the years 2019 through 2021. We are using data provided by
the music distribution company Tunecore. The same data is used by
Tunecore to calculate payments due to the band based on sales and
streams on a large number music platforms.

Required packages: tidyverse, countrycode, lubridate

## Business Task

- Determine the best time to release a new single to maximize streams
- Determine the most popular streaming service(s) for the music
- Determine the most popular countries for the music
- Use insights to choose the best time to release new single and where
  to focus ad dollars

## Data

We are using data provided by the music distribution company Tunecore.
The same data is used by Tunecore to calculate payments due to the band
based on sales and streams on a large number music platforms. The data
is reliable, comprehensive, and current since it comes directly from
Tunecore. This data is owned by me.

##### File Name

- AndrewWhitmanMusicSales_2019_to_2022.csv

This is a merge of 36 csv files containing monthly sales data. It was
merged using SheetGo.

## Process

I used R Studio and SheetGo to merge, clean, and aggregate the data. The
cleaning process included:

- Renaming columns
- Removing unnecessary columns
- Removing duplicate rows
- Removing null music sales
- Rename country code with full country name

## Setting up the environment

Add required packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(countrycode)
```

## STEP 1: COLLECT DATA

``` r
music_sales <- read_csv("AndrewWhitmanMusicSales_2019_to_2022.csv",
  col_types = cols(`Sales Period` = col_date(format = "%Y-%m-%d"), 
  `Posted Date` = col_date(format = "%Y-%m-%d")))
```

## STEP 2: CLEAN THE DATA

Check the structure of the data. Rename columns. Remove duplicates and
columns with null in “Artist” column.

``` r
str(music_sales)
```

    ## spec_tbl_df [22,540 × 21] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Sales Period      : Date[1:22540], format: "2019-01-01" "2019-01-01" ...
    ##  $ Posted Date       : Date[1:22540], format: "2019-08-02" "2019-08-02" ...
    ##  $ Store Name        : chr [1:22540] "WiMP" "WiMP" "WiMP" "WiMP" ...
    ##  $ Country Of Sale   : chr [1:22540] "AU" "DK" "NO" "NO" ...
    ##  $ Artist            : chr [1:22540] "Andrew Whitman" "Andrew Whitman" "Andrew Whitman" "Andrew Whitman" ...
    ##  $ Release Type      : chr [1:22540] "Song" "Song" "Song" "Song" ...
    ##  $ Release Title     : chr [1:22540] "The Beat of Life" "The Beat of Life" "The Beat of Life" "The Beat of Life" ...
    ##  $ Song Title        : chr [1:22540] "Sinner's Sonnet" "Sinner's Sonnet" "Sinner's Sonnet" "Sinner's Sonnet" ...
    ##  $ Label             : chr [1:22540] "Andrew Whitman" "Andrew Whitman" "Andrew Whitman" "Andrew Whitman" ...
    ##  $ UPC               : num [1:22540] 8.6e+11 8.6e+11 8.6e+11 8.6e+11 8.6e+11 ...
    ##  $ Optional UPC      : logi [1:22540] NA NA NA NA NA NA ...
    ##  $ TC Song ID        : chr [1:22540] "USTCZ0928423" "USTCZ0928423" "USTCZ0928423" "USTCZ0928423" ...
    ##  $ Optional ISRC     : logi [1:22540] NA NA NA NA NA NA ...
    ##  $ Sales Type        : chr [1:22540] "Streaming" "Streaming" "Streaming" "Streaming" ...
    ##  $ # Units Sold      : num [1:22540] 1 1 1 5 7 1 38 42 45 33 ...
    ##  $ Per Unit Price    : num [1:22540] 0.0118 0.0066 0.0165 0.0118 0.0121 ...
    ##  $ Net Sales         : num [1:22540] 0.0118 0.0066 0.0165 0.0591 0.0846 ...
    ##  $ Net Sales Currency: chr [1:22540] "USD" "USD" "USD" "USD" ...
    ##  $ Exchange Rate     : num [1:22540] NA NA NA NA NA NA NA NA NA NA ...
    ##  $ Total Earned      : num [1:22540] 0.0118 0.0066 0.0165 0.0591 0.0846 ...
    ##  $ Currency          : chr [1:22540] "USD" "USD" "USD" "USD" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   `Sales Period` = col_date(format = "%Y-%m-%d"),
    ##   ..   `Posted Date` = col_date(format = "%Y-%m-%d"),
    ##   ..   `Store Name` = col_character(),
    ##   ..   `Country Of Sale` = col_character(),
    ##   ..   Artist = col_character(),
    ##   ..   `Release Type` = col_character(),
    ##   ..   `Release Title` = col_character(),
    ##   ..   `Song Title` = col_character(),
    ##   ..   Label = col_character(),
    ##   ..   UPC = col_double(),
    ##   ..   `Optional UPC` = col_logical(),
    ##   ..   `TC Song ID` = col_character(),
    ##   ..   `Optional ISRC` = col_logical(),
    ##   ..   `Sales Type` = col_character(),
    ##   ..   `# Units Sold` = col_double(),
    ##   ..   `Per Unit Price` = col_double(),
    ##   ..   `Net Sales` = col_double(),
    ##   ..   `Net Sales Currency` = col_character(),
    ##   ..   `Exchange Rate` = col_double(),
    ##   ..   `Total Earned` = col_double(),
    ##   ..   Currency = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

Rename columns

``` r
music_sales <- music_sales %>% 
  rename(Sales_Period = "Sales Period", Posted_Date = "Posted Date",
         Store_Name = "Store Name", Country_of_Sale = "Country Of Sale",
         Release_Type = "Release Type", Release_Title = "Release Title",
         Song_Title = "Song Title", Sales_Type = "Sales Type", 
         num_units_sold = "# Units Sold", Per_Unit_Price = "Per Unit Price",
         Net_Sales = "Net Sales", Net_Sales_Curreny = "Net Sales Currency",
         Exchange_Rate = "Exchange Rate", Total_Earned = "Total Earned")
```

Remove duplicates and nulls

``` r
music_sales <- music_sales %>% 
  distinct() %>% 
  filter(!is.na(Artist))
```

Rename countries

``` r
music_sales$Country_of_Sale <- countrycode(music_sales$Country_of_Sale, "iso2c", "country.name")
```

    ## Warning in countrycode_convert(sourcevar = sourcevar, origin = origin, destination = dest, : Some values were not matched unambiguously: XK

## STEP 3: AGGREGATE DATA

1.  Create data frame with data aggregated by month and total sales.
2.  Create data frame with data aggregated by store and total sales.

``` r
monthly_totals <- music_sales %>% 
  group_by(Sales_Period) %>% 
  summarize(sum_of_sales = sum(Total_Earned))
```

``` r
store_totals <- music_sales %>% 
  group_by(Store_Name) %>% 
  summarize(sum_of_sales = sum(Total_Earned))
```

## STEP 4: ANALYSIS

**Determine most popular month for streaming. The late spring and early
summer months get the most streams.**

``` r
music_sales %>% 
  group_by(Sales_Period) %>% 
  summarize(total_songs = sum(num_units_sold)) %>% 
  ggplot() + geom_col(mapping = aes(x = month
  (Sales_Period, label = TRUE, abb = TRUE), y = total_songs)) +
  labs(title = "Most Popular Months", x = "Month", y = "Streams") +
  theme(plot.title = element_text(hjust = 0.5)) +
  guides(fill=guide_legend(title="Song Titles"))
```

![](whitman_music_sales_2019_to_2022_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

**Determine the store with the most streams. Amazon Unlimited, Spotify,
and Amazon Cloud are have streamed the most during this period**.

``` r
store_totals %>% 
  filter(sum_of_sales > 20) %>% 
  ggplot() + geom_col(mapping = aes(x = Store_Name, y = sum_of_sales)) +
  labs(title = "Income Per Store", x = "Store", y = "Income - US $") +
  theme_bw() +
  theme(axis.title.x = element_blank(), 
        axis.text.x = element_text(angle = 30, vjust = 1, hjust = 1),
        plot.title = element_text(hjust = 0.5))
```

![](whitman_music_sales_2019_to_2022_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

**Determine the most popular streaming countries. Taiwan has 20,000 more
streams than the second country (Canada)**.

``` r
music_sales %>% 
  group_by(Country_of_Sale) %>% 
  summarize(total_streams = sum(num_units_sold)) %>% 
  arrange(desc(total_streams)) %>% 
  filter(total_streams > 5000) %>% 
  mutate(average_monthly_streams = total_streams/36)
```

    ## # A tibble: 7 × 3
    ##   Country_of_Sale total_streams average_monthly_streams
    ##   <chr>                   <dbl>                   <dbl>
    ## 1 United States          222646                   6185.
    ## 2 Taiwan                  34100                    947.
    ## 3 Canada                  12490                    347.
    ## 4 Australia                7781                    216.
    ## 5 Sweden                   6379                    177.
    ## 6 United Kingdom           5615                    156.
    ## 7 Germany                  5120                    142.

## Conclusions and Recommendations

- There is bump in streaming at the end of spring and beginning of
  summer. This would be the best time to release a new single

- The most songs are streamed on Amazon and Spotify in the US. Ad money
  should focus on those two streaming services. Outside of the US,
  Taiwan had 34,100 streams during the three year period (947 per
  month). I recommend using ad money directed toward Taiwan to attempt
  to take advantage of the high demand there.
