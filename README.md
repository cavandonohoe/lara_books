
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Tracking Lara’s Books

<!-- badges: start -->
<!-- badges: end -->

We have a lot to cover, so some of these will just come to mind

## Cumulative Sum of Books Read Over Time

``` r
lara_books_cleaned %>%
  group_by(year_month) %>%
  count %>%
  ungroup() %>%
  mutate(cum_sum = cumsum(n)) %>%
  ggplot(aes(x = year_month, y = cum_sum)) +
  geom_line() +
  xlab("Date") + ylab("Cumulative Sum") + ggtitle("Cumulative Sum of Books Read Over Time")
```

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

## Cumulative Sum of Pages Read Over Time

``` r
lara_books_cleaned %>%
  group_by(year_month) %>%
  summarise(pages_read = sum(`Page Count`)) %>%
  ungroup() %>%
  mutate(cum_sum = cumsum(pages_read)) %>%
  ggplot(aes(x = year_month, y = cum_sum)) +
  geom_line() +
  xlab("Date") + ylab("Cumulative Sum") + ggtitle("Cumulative Sum of Pages Read Over Time")
```

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Types of Books Read

Small Note here:

There will be double/triple/etc counting since some books have multiple
genres

### Per Month

``` r
lara_books_cleaned %>%
  group_by(Genre, year_month) %>%
  count %>% ungroup() %>%
  separate_rows(Genre, sep = ", ") %>%
  group_by(Genre, year_month) %>% summarise(n = sum(n)) %>%
  ungroup() %>%
  ggplot(aes(x = year_month, y = n, fill = Genre)) +
  geom_col() +
  xlab("Date") + ylab("Books Read") +
  ggtitle("Type of Books Read Per Month")
#> `summarise()` has grouped output by 'Genre'. You can override using the
#> `.groups` argument.
```

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

### Overall

``` r
lara_books_cleaned %>%
  group_by(Genre) %>%
  count %>% ungroup() %>%
  separate_rows(Genre, sep = ", ") %>%
  group_by(Genre) %>% summarise(n = sum(n)) %>%
  ungroup() %>%
  dplyr::arrange(desc(n)) %>%
  dplyr::mutate(Genre = factor(Genre, levels = Genre)) %>%
  ggplot(aes(y = Genre, x = n)) +
  geom_col(fill = "#FF5733") +
  xlab("Date") + ylab("Genre") +
  ggtitle("Types of Books Read Overall")
```

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
