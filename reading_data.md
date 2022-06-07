reading_data
================
pika
2022-05-20

\##Scrape a table

I want the first table from [this
page](https://www.bestplaces.net/cost_of_living/city/new_york/new_york)

read in the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

Extrat the table(s)

``` r
table_marj =
  drug_use_html %>% 
  html_nodes(css= "table") %>% 
  first() %>% 
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

## star wars movies info

I want the data from [here](https://www.imdb.com/list/ls070150896/)

``` r
url= "https://www.imdb.com/list/ls070150896/"

swm_html= read_html(url)
```

Grab elements that I want

``` r
title_vec =
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()

gross_rev_vec=
  swm_html %>% 
  html_nodes(css = ".text-muted .ghost~ .text-muted+ span") %>% 
  html_text()
  
runtime_vec=
  swm_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

swm_df=
  tibble(
    title = title_vec,
    gross_rev = gross_rev_vec, 
    runtime = runtime_vec)
```

## Get some water data

this is coming from an API

``` r
nyc_water =
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content("parse")
```

    ## Rows: 43 Columns: 4

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
nyc_water =
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>%
  content("text") %>% 
  jsonlite::fromJSON() %>% 
  as_tibble()
```
