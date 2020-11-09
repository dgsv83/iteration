Iteration and listcols
================

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_char = c("My", "name", "is", "Jeff"),
  vec_logical = c(TRUE, TRUE, TRUE, FALSE, TRUE, TRUE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[[1]][1:3]
```

    ## [1] 5 6 7

## ‘for’ loop

create a new list.

``` r
list_norms = 
  list(
    a = rnorm(20, 3, 1),
    b = rnorm(30, 0, 5),
    c = rnorm(40, 10, .2),
    d = rnorm(20, -3, 1)
  )

is.list(list_norms)
```

    ## [1] TRUE

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.17  1.03

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0116  5.09

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.197

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.65 0.948

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norms[[i]])

}
```

## Let’s try map\!

``` r
output = map(list_norms, mean_and_sd)
```

what if you want a different function..?

``` r
output = map(list_norms, median)
output = map(list_norms, IQR)
```

``` r
output = map_dbl(list_norms, median)
```

``` r
output = map_df(list_norms, mean_and_sd, .id = "input")
```

## List columns\!

``` r
listcol_df = 
  tibble(
    name = c("a","b","c","d"),
    samp = list_norms
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 4.0317811 3.6773079 2.3940488 4.9235070 2.2167676 2.7276463 3.5723113
    ##  [8] 3.2474498 0.4013121 2.2287926 3.4166403 2.9344297 2.7993621 3.7920211
    ## [15] 3.3375072 2.4734346 3.1411245 4.9489139 2.9235379 4.2735525
    ## 
    ## $b
    ##  [1]  -0.2783903  -1.8471117   2.0676547  -1.2119882   4.7374223   7.1266062
    ##  [7]  -8.7290475   2.8115838  -2.6450479   2.0473179   2.6101464   2.4176631
    ## [13]   1.8817518  -0.8521401  -2.3096088   3.7511041 -11.8728810  -4.0521502
    ## [19]   7.3345405   2.3421746  -0.3179236  -5.6151006   6.4582140   1.2240091
    ## [25] -12.5842025   3.8469925   6.4921567   0.1350233  -5.3384489   0.7191378
    ## 
    ## $c
    ##  [1]  9.937638 10.161862 10.211082 10.078276 10.189441 10.042204  9.578111
    ##  [8] 10.286862 10.008766 10.261320  9.882078 10.301097 10.162242  9.700962
    ## [15] 10.066045  9.799748 10.097983 10.326935 10.006898  9.961918  9.847364
    ## [22] 10.052845 10.191767 10.319745  9.891466  9.917973 10.271402 10.015647
    ## [29]  9.933841  9.733160 10.144950  9.826421  9.967276  9.627734  9.716128
    ## [36] 10.045833 10.042855 10.305929 10.046283  9.952487
    ## 
    ## $d
    ##  [1] -3.315647 -4.572149 -4.284433 -1.976692 -3.321144 -4.907636 -3.484640
    ##  [8] -5.582540 -2.907047 -4.874451 -2.171057 -3.721806 -3.554340 -3.448368
    ## [15] -3.338464 -2.743361 -3.112357 -4.232850 -4.625808 -2.829103

``` r
listcol_df %>%
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

let’s try some operations.

``` r
listcol_df$samp[[1]]
```

    ##  [1] 4.0317811 3.6773079 2.3940488 4.9235070 2.2167676 2.7276463 3.5723113
    ##  [8] 3.2474498 0.4013121 2.2287926 3.4166403 2.9344297 2.7993621 3.7920211
    ## [15] 3.3375072 2.4734346 3.1411245 4.9489139 2.9235379 4.2735525

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.17  1.03

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0116  5.09

can i just …map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.17  1.03
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0116  5.09
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.197
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.65 0.948

so can i add a list column? (in this case, to store the output)

``` r
listcol_df = 
  listcol_df %>% 
  mutate(summary = map(samp, mean_and_sd),
         medians = map_dbl(samp, median))

listcol_df
```

    ## # A tibble: 4 x 4
    ##   name  samp         summary          medians
    ##   <chr> <named list> <named list>       <dbl>
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>   3.19 
    ## 2 b     <dbl [30]>   <tibble [1 × 2]>   0.972
    ## 3 c     <dbl [40]>   <tibble [1 × 2]>  10.0  
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>  -3.47
