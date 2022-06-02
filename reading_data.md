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
