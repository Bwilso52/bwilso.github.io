Capstone Project
================
Brandon Wilson
10/31/2021

## Capstone Project

For my Capstone Project, I took the role of a Data Analyst working for a
fictional bike-share company called Cyclistic. Cyclistic was looking to
convert there casual users into members but needed to find out how. I
was assigned the task of figuring out how casual users usage of bikes
differed from member usage.

## Data Cleaning and Preparation

To prepare my data for analysis, I began by installing the necessary
packages:

``` r
install.packages("tidyverse")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.1'
    ## (as 'lib' is unspecified)

``` r
install.packages("lubridate")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.1'
    ## (as 'lib' is unspecified)

``` r
install.packages("ggplot2")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.1'
    ## (as 'lib' is unspecified)

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.5     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
    ## ✓ readr   2.0.2     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(ggplot2)
```

$$\\\\\[.1in\]$$
I then moved forward with uploading my data sets:
$$\\\\\[.1in\]$$

``` r
q3_2019 <- read_csv("Divvy_Trips_2019_Q3.csv")
```

    ## Rows: 1640718 Columns: 12

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (4): from_station_name, to_station_name, usertype, gender
    ## dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
    ## dttm (2): start_time, end_time

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
q4_2019 <- read_csv("Divvy_Trips_2019_Q4.csv")
```

    ## Rows: 704054 Columns: 12

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (4): from_station_name, to_station_name, usertype, gender
    ## dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
    ## dttm (2): start_time, end_time

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
q1_2020 <- read_csv("Divvy_Trips_2020_Q1.csv")
```

    ## Rows: 426887 Columns: 13

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

$$\\\\\[.1in\]$$
After analyzing the current data sets I had to rename some of the Q3 and
Q4 column names in order to match with the Q1 column names:
$$\\\\\[.1in\]$$

``` r
(q3_2019 <- rename(q3_2019, ride_id = trip_id, rideable_type = bikeid, started_at = start_time, ended_at = end_time, start_station_name = from_station_name, start_station_id = from_station_id, end_station_name = to_station_name, end_station_id = to_station_id, member_casual = usertype))
```

    ## # A tibble: 1,640,718 × 12
    ##     ride_id started_at          ended_at            rideable_type tripduration
    ##       <dbl> <dttm>              <dttm>                      <dbl>        <dbl>
    ##  1 23479388 2019-07-01 00:00:27 2019-07-01 00:20:41          3591         1214
    ##  2 23479389 2019-07-01 00:01:16 2019-07-01 00:18:44          5353         1048
    ##  3 23479390 2019-07-01 00:01:48 2019-07-01 00:27:42          6180         1554
    ##  4 23479391 2019-07-01 00:02:07 2019-07-01 00:27:10          5540         1503
    ##  5 23479392 2019-07-01 00:02:13 2019-07-01 00:22:26          6014         1213
    ##  6 23479393 2019-07-01 00:02:21 2019-07-01 00:07:31          4941          310
    ##  7 23479394 2019-07-01 00:02:24 2019-07-01 00:23:12          3770         1248
    ##  8 23479395 2019-07-01 00:02:26 2019-07-01 00:28:16          5442         1550
    ##  9 23479396 2019-07-01 00:02:34 2019-07-01 00:28:57          2957         1583
    ## 10 23479397 2019-07-01 00:02:45 2019-07-01 00:29:14          6091         1589
    ## # … with 1,640,708 more rows, and 7 more variables: start_station_id <dbl>,
    ## #   start_station_name <chr>, end_station_id <dbl>, end_station_name <chr>,
    ## #   member_casual <chr>, gender <chr>, birthyear <dbl>

``` r
(q4_2019 <- rename(q4_2019, ride_id = trip_id, rideable_type = bikeid, started_at = start_time, ended_at = end_time, start_station_name = from_station_name, start_station_id = from_station_id, end_station_name = to_station_name, end_station_id = to_station_id, member_casual = usertype))
```

    ## # A tibble: 704,054 × 12
    ##     ride_id started_at          ended_at            rideable_type tripduration
    ##       <dbl> <dttm>              <dttm>                      <dbl>        <dbl>
    ##  1 25223640 2019-10-01 00:01:39 2019-10-01 00:17:20          2215          940
    ##  2 25223641 2019-10-01 00:02:16 2019-10-01 00:06:34          6328          258
    ##  3 25223642 2019-10-01 00:04:32 2019-10-01 00:18:43          3003          850
    ##  4 25223643 2019-10-01 00:04:32 2019-10-01 00:43:43          3275         2350
    ##  5 25223644 2019-10-01 00:04:34 2019-10-01 00:35:42          5294         1867
    ##  6 25223645 2019-10-01 00:04:38 2019-10-01 00:10:51          1891          373
    ##  7 25223646 2019-10-01 00:04:52 2019-10-01 00:22:45          1061         1072
    ##  8 25223647 2019-10-01 00:04:57 2019-10-01 00:29:16          1274         1458
    ##  9 25223648 2019-10-01 00:05:20 2019-10-01 00:29:18          6011         1437
    ## 10 25223649 2019-10-01 00:05:20 2019-10-01 02:23:46          2957         8306
    ## # … with 704,044 more rows, and 7 more variables: start_station_id <dbl>,
    ## #   start_station_name <chr>, end_station_id <dbl>, end_station_name <chr>,
    ## #   member_casual <chr>, gender <chr>, birthyear <dbl>

$$\\\\\[.1in\]$$
I checked the tables for any further issues and had to convert ride\_id
and rideable\_type to character so they could be combined correctly:
$$\\\\\[.1in\]$$

``` r
str(q3_2019)
```

    ## spec_tbl_df [1,640,718 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ ride_id           : num [1:1640718] 23479388 23479389 23479390 23479391 23479392 ...
    ##  $ started_at        : POSIXct[1:1640718], format: "2019-07-01 00:00:27" "2019-07-01 00:01:16" ...
    ##  $ ended_at          : POSIXct[1:1640718], format: "2019-07-01 00:20:41" "2019-07-01 00:18:44" ...
    ##  $ rideable_type     : num [1:1640718] 3591 5353 6180 5540 6014 ...
    ##  $ tripduration      : num [1:1640718] 1214 1048 1554 1503 1213 ...
    ##  $ start_station_id  : num [1:1640718] 117 381 313 313 168 300 168 313 43 43 ...
    ##  $ start_station_name: chr [1:1640718] "Wilton Ave & Belmont Ave" "Western Ave & Monroe St" "Lakeview Ave & Fullerton Pkwy" "Lakeview Ave & Fullerton Pkwy" ...
    ##  $ end_station_id    : num [1:1640718] 497 203 144 144 62 232 62 144 195 195 ...
    ##  $ end_station_name  : chr [1:1640718] "Kimball Ave & Belmont Ave" "Western Ave & 21st St" "Larrabee St & Webster Ave" "Larrabee St & Webster Ave" ...
    ##  $ member_casual     : chr [1:1640718] "Subscriber" "Customer" "Customer" "Customer" ...
    ##  $ gender            : chr [1:1640718] "Male" NA NA NA ...
    ##  $ birthyear         : num [1:1640718] 1992 NA NA NA NA ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   trip_id = col_double(),
    ##   ..   start_time = col_datetime(format = ""),
    ##   ..   end_time = col_datetime(format = ""),
    ##   ..   bikeid = col_double(),
    ##   ..   tripduration = col_number(),
    ##   ..   from_station_id = col_double(),
    ##   ..   from_station_name = col_character(),
    ##   ..   to_station_id = col_double(),
    ##   ..   to_station_name = col_character(),
    ##   ..   usertype = col_character(),
    ##   ..   gender = col_character(),
    ##   ..   birthyear = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(q4_2019)
```

    ## spec_tbl_df [704,054 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ ride_id           : num [1:704054] 25223640 25223641 25223642 25223643 25223644 ...
    ##  $ started_at        : POSIXct[1:704054], format: "2019-10-01 00:01:39" "2019-10-01 00:02:16" ...
    ##  $ ended_at          : POSIXct[1:704054], format: "2019-10-01 00:17:20" "2019-10-01 00:06:34" ...
    ##  $ rideable_type     : num [1:704054] 2215 6328 3003 3275 5294 ...
    ##  $ tripduration      : num [1:704054] 940 258 850 2350 1867 ...
    ##  $ start_station_id  : num [1:704054] 20 19 84 313 210 156 84 156 156 336 ...
    ##  $ start_station_name: chr [1:704054] "Sheffield Ave & Kingsbury St" "Throop (Loomis) St & Taylor St" "Milwaukee Ave & Grand Ave" "Lakeview Ave & Fullerton Pkwy" ...
    ##  $ end_station_id    : num [1:704054] 309 241 199 290 382 226 142 463 463 336 ...
    ##  $ end_station_name  : chr [1:704054] "Leavitt St & Armitage Ave" "Morgan St & Polk St" "Wabash Ave & Grand Ave" "Kedzie Ave & Palmer Ct" ...
    ##  $ member_casual     : chr [1:704054] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
    ##  $ gender            : chr [1:704054] "Male" "Male" "Female" "Male" ...
    ##  $ birthyear         : num [1:704054] 1987 1998 1991 1990 1987 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   trip_id = col_double(),
    ##   ..   start_time = col_datetime(format = ""),
    ##   ..   end_time = col_datetime(format = ""),
    ##   ..   bikeid = col_double(),
    ##   ..   tripduration = col_number(),
    ##   ..   from_station_id = col_double(),
    ##   ..   from_station_name = col_character(),
    ##   ..   to_station_id = col_double(),
    ##   ..   to_station_name = col_character(),
    ##   ..   usertype = col_character(),
    ##   ..   gender = col_character(),
    ##   ..   birthyear = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(q1_2020)
```

    ## spec_tbl_df [426,887 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:426887] "EACB19130B0CDA4A" "8FED874C809DC021" "789F3C21E472CA96" "C9A388DAC6ABF313" ...
    ##  $ rideable_type     : chr [1:426887] "docked_bike" "docked_bike" "docked_bike" "docked_bike" ...
    ##  $ started_at        : POSIXct[1:426887], format: "2020-01-21 20:06:59" "2020-01-30 14:22:39" ...
    ##  $ ended_at          : POSIXct[1:426887], format: "2020-01-21 20:14:30" "2020-01-30 14:26:22" ...
    ##  $ start_station_name: chr [1:426887] "Western Ave & Leland Ave" "Clark St & Montrose Ave" "Broadway & Belmont Ave" "Clark St & Randolph St" ...
    ##  $ start_station_id  : num [1:426887] 239 234 296 51 66 212 96 96 212 38 ...
    ##  $ end_station_name  : chr [1:426887] "Clark St & Leland Ave" "Southport Ave & Irving Park Rd" "Wilton Ave & Belmont Ave" "Fairbanks Ct & Grand Ave" ...
    ##  $ end_station_id    : num [1:426887] 326 318 117 24 212 96 212 212 96 100 ...
    ##  $ start_lat         : num [1:426887] 42 42 41.9 41.9 41.9 ...
    ##  $ start_lng         : num [1:426887] -87.7 -87.7 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num [1:426887] 42 42 41.9 41.9 41.9 ...
    ##  $ end_lng           : num [1:426887] -87.7 -87.7 -87.7 -87.6 -87.6 ...
    ##  $ member_casual     : chr [1:426887] "member" "member" "member" "member" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   ride_id = col_character(),
    ##   ..   rideable_type = col_character(),
    ##   ..   started_at = col_datetime(format = ""),
    ##   ..   ended_at = col_datetime(format = ""),
    ##   ..   start_station_name = col_character(),
    ##   ..   start_station_id = col_double(),
    ##   ..   end_station_name = col_character(),
    ##   ..   end_station_id = col_double(),
    ##   ..   start_lat = col_double(),
    ##   ..   start_lng = col_double(),
    ##   ..   end_lat = col_double(),
    ##   ..   end_lng = col_double(),
    ##   ..   member_casual = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
q3_2019 <- mutate(q3_2019, ride_id = as.character(ride_id), rideable_type = as.character(rideable_type))
```

``` r
q4_2019 <- mutate(q4_2019, ride_id = as.character(ride_id), rideable_type = as.character(rideable_type))
```

$$\\\\\[.1in\]$$
I could then combine the tables into one and get rid of unneccesary
columns:
$$\\\\\[.1in\]$$

``` r
all_trips <- bind_rows(q1_2020, q3_2019, q4_2019)
```

``` r
all_trips <- all_trips %>%
  select(-c(start_lat, start_lng, end_lat, end_lng, birthyear, gender, "tripduration"))
```

$$\\\\\[.1in\]$$
The member\_casual column had two different names for members and
casuals so they had to be changed to be consistent:
$$\\\\\[.1in\]$$

``` r
all_trips <- all_trips%>%
  mutate(member_casual = recode(member_casual, "Subscriber" = "member", "Customer" = "casual"))
```

$$\\\\\[.1in\]$$
I then added new columns to provide a more detailed analyses:
$$\\\\\[.1in\]$$

``` r
all_trips$date <- as.Date(all_trips$started_at)
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")
all_trips$ride_length <- difftime(all_trips$ended_at, all_trips$started_at)
```

$$\\\\\[.1in\]$$
Up next was converting my ride\_length calculation to a numeric:
$$\\\\\[.1in\]$$

``` r
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))
is.numeric(all_trips$ride_length)
```

    ## [1] TRUE

$$\\\\\[.1in\]$$
Next, I removed bad data and converted the data into a new clean table:
$$\\\\\[.1in\]$$

``` r
 all_trips_v2 <- all_trips[!(all_trips$start_station_name == "HQ QR" | all_trips$ride_length<0),]
```

//\[.1*i**n*\]

## Analysis

$$\\\\\[.1in\]$$
I began my analysis by looking at a summary of my new table:
$$\\\\\[.1in\]$$

``` r
summary(all_trips_v2)
```

    ##    ride_id          rideable_type        started_at                 
    ##  Length:2767879     Length:2767879     Min.   :2019-07-01 00:00:27  
    ##  Class :character   Class :character   1st Qu.:2019-08-07 14:34:39  
    ##  Mode  :character   Mode  :character   Median :2019-09-14 17:14:25  
    ##                                        Mean   :2019-10-02 19:49:26  
    ##                                        3rd Qu.:2019-11-08 21:39:14  
    ##                                        Max.   :2020-03-31 23:51:34  
    ##     ended_at                   start_station_name start_station_id
    ##  Min.   :2019-07-01 00:07:31   Length:2767879     Min.   :  2.0   
    ##  1st Qu.:2019-08-07 15:08:08   Class :character   1st Qu.: 77.0   
    ##  Median :2019-09-14 17:49:59   Mode  :character   Median :174.0   
    ##  Mean   :2019-10-02 20:15:06                      Mean   :203.3   
    ##  3rd Qu.:2019-11-08 22:09:54                      3rd Qu.:291.0   
    ##  Max.   :2020-05-19 20:10:34                      Max.   :673.0   
    ##  end_station_name   end_station_id  member_casual           date           
    ##  Length:2767879     Min.   :  2.0   Length:2767879     Min.   :2019-07-01  
    ##  Class :character   1st Qu.: 77.0   Class :character   1st Qu.:2019-08-07  
    ##  Mode  :character   Median :174.0   Mode  :character   Median :2019-09-14  
    ##                     Mean   :204.1                      Mean   :2019-10-02  
    ##                     3rd Qu.:291.0                      3rd Qu.:2019-11-08  
    ##                     Max.   :675.0                      Max.   :2020-03-31  
    ##     month               day                year           day_of_week       
    ##  Length:2767879     Length:2767879     Length:2767879     Length:2767879    
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##   ride_length     
    ##  Min.   :      1  
    ##  1st Qu.:    406  
    ##  Median :    701  
    ##  Mean   :   1540  
    ##  3rd Qu.:   1265  
    ##  Max.   :9387024

$$\\\\\[.1in\]$$
Next I looked at some comparisons between casual and member usage:
$$\\\\\[.1in\]$$

``` r
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = mean)
```

    ##   all_trips_v2$member_casual all_trips_v2$ride_length
    ## 1                     casual                3812.0090
    ## 2                     member                 852.9544

``` r
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
```

    ##    all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
    ## 1                      casual                   Friday                4170.0055
    ## 2                      member                   Friday                 823.7175
    ## 3                      casual                   Monday                3632.0641
    ## 4                      member                   Monday                 843.2026
    ## 5                      casual                 Saturday                3460.4535
    ## 6                      member                 Saturday                 991.1996
    ## 7                      casual                   Sunday                3871.9759
    ## 8                      member                   Sunday                 910.9079
    ## 9                      casual                 Thursday                3928.9726
    ## 10                     member                 Thursday                 824.2099
    ## 11                     casual                  Tuesday                3872.3017
    ## 12                     member                  Tuesday                 837.5502
    ## 13                     casual                Wednesday                4007.4516
    ## 14                     member                Wednesday                 822.4819

$$\\\\\[.1in\]$$
The days of the week were not in order so I rearanged them:
$$\\\\\[.1in\]$$

``` r
all_trips_v2$day_of_week <- ordered(all_trips_v2$day_of_week, levels= c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```

``` r
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = median)
```

    ##    all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
    ## 1                      casual                   Sunday                     1573
    ## 2                      member                   Sunday                      629
    ## 3                      casual                   Monday                     1514
    ## 4                      member                   Monday                      574
    ## 5                      casual                  Tuesday                     1374
    ## 6                      member                  Tuesday                      574
    ## 7                      casual                Wednesday                     1384
    ## 8                      member                Wednesday                      576
    ## 9                      casual                 Thursday                     1413
    ## 10                     member                 Thursday                      577
    ## 11                     casual                   Friday                     1466
    ## 12                     member                   Friday                      567
    ## 13                     casual                 Saturday                     1619
    ## 14                     member                 Saturday                      628

$$\\\\\[.1in\]$$
I then analyzed ridership data by type and weekday:
$$\\\\\[.1in\]$$

``` r
all_trips_v2 %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday)
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.

    ## # A tibble: 14 × 4
    ## # Groups:   member_casual [2]
    ##    member_casual weekday number_of_rides average_duration
    ##    <chr>         <ord>             <int>            <dbl>
    ##  1 casual        Sun              125961            3872.
    ##  2 casual        Mon               75719            3632.
    ##  3 casual        Tue               64944            3872.
    ##  4 casual        Wed               67556            4007.
    ##  5 casual        Thu               77908            3929.
    ##  6 casual        Fri               85796            4170.
    ##  7 casual        Sat              144712            3460.
    ##  8 member        Sun              192315             911.
    ##  9 member        Mon              343330             843.
    ## 10 member        Tue              368924             838.
    ## 11 member        Wed              355026             822.
    ## 12 member        Thu              346228             824.
    ## 13 member        Fri              317386             824.
    ## 14 member        Sat              202074             991.

$$\\\\\[.1in\]$$
Number of riders by ride day:
$$\\\\\[.1in\]$$

``` r
all_trips_v2 %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_ride = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = number_of_ride, fill = member_casual)) + geom_col (position = "dodge")
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.

![](Capstone-Project-markdown_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->
$$\\\\\[.1in\]$$
And lastly I created a visualization for average duration:
$$\\\\\[.1in\]$$

``` r
all_trips_v2 %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = average_duration, fill = member_casual)) + geom_col(postion = "dodge")
```

    ## `summarise()` has grouped output by 'member_casual'. You can override using the `.groups` argument.

    ## Warning: Ignoring unknown parameters: postion

![](Capstone-Project-markdown_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->
$$\\\\\[.1in\]$$

## Conclusion

$$\\\\\[.1in\]$$
Through analysis of the data I was able to find multiple insights that
would help Cyclistics convert casual users into members. By looking at
the graph depicting the number of rides by day of the week, it was clear
that casual riders were more likely to use the bikes over the weekend
while members were more likey to ride during the week. It is possible
the members are using bikes as transportation to work while casual
members are using them for leisure activities over the weekend. Looking
at the graph depicting the average duration of rides per day of the week
it was clear that casual members were riding much longer than members as
well. In conclusion, the best way for Cyclistics to convert casual
riders to members is to add advantages for members with longer ride
durations as well as benefits for over the weekend use. By focusing on
how casual riders use the bikes, understanding what would appeal to them
becomes much easier.
