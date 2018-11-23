Blackfriday
================
Abhishek Ajay (aa4266)
November 23, 2018

``` r
urls = read_html("https://www.macys.com/shop/black-friday-sale/black-friday-doorbusters/Pageindex/1?id=62204")

allbrands = 
  urls %>% 
  html_nodes(".facet_name , .listbox") %>% 
  html_text() %>% 
  as.tibble()

allbrands = allbrands[6,1]

allbrands_list = 
  allbrands %>% 
  as.tibble() %>% 
  as.character()
  
  
allbrands_list = strsplit(allbrands_list, ")")

allbrands_list = 
  allbrands_list[[1]] %>% 
  as.tibble()
  
allbrands_list = allbrands_list[c(1:966),] %>% 
  separate(value, into = c("brand", "count"), sep = "[(]")
```

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 1 rows
    ## [966].

``` r
allbrands_list %>% 
  as.tibble() %>% 
  filter(count != "x", brand != "ist",!is.na(count)) %>% 
  rbind(c("2(X)ist", "3")) %>%
  #manually edited for the brand 2(X)ist with 3 products on sale
  mutate(count = as.numeric(count)) %>% 
  arrange(desc(count)) %>% 
  head() %>% 
  knitr::kable()
```

| brand                      |  count|
|:---------------------------|------:|
| Macy's                     |   4294|
| Surya                      |   3076|
| INC International Concepts |   1927|
| Trademark Global           |   1875|
| Tommy Hilfiger             |   1815|
| Calvin Klein               |   1639|

``` r
#row_count = seq(from = 2, to = 120, by = 2)

#products_page_3 = products_page_3[row_count,]

#x <- c(as = "asfef", qu = "qwerty", "yuiop[", "b", "stuff.blah.yech")
# split x on the letter e
#strsplit(x, "e")
```
