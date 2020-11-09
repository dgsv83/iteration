Simulations
================

## let’s simulate something

i have a function:

``` r
sim_mean_sd = function(n, mu = 2, sigma = 3) {
  
  sim_data = tibble(
    x = rnorm(n, mean = mu, sd = sigma),
  )
  
  sim_data %>% 
    summarize(
      mu_hat = mean(x),
      sigma_hat = sd(x)
    )
}
```

i can ‘simulate’ by running this line over and over.

``` r
sim_mean_sd(30)
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   1.51      2.91

## let’s simulate a lot.

let’s start with a for loop.

``` r
output = vector("list", 100)

for (i in 1:100) {
  output[[i]] = sim_mean_sd(30)
}

sim_results = bind_rows(output)
```

let’s use a loop function. (we don’t use map here because we don’t have
a input list in this case)

``` r
sim_results = 
  rerun(100, sim_mean_sd(n = 30)) %>% 
  bind_rows()
```

let’s look at results…

``` r
sim_results %>% 
  ggplot(aes(x = mu_hat)) + 
  geom_density()
```

<img src="simulaitons_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
sim_results %>% 
  summarize(
    avg_samp_mean = mean(mu_hat),
    sd_samp_mean = sd(mu_hat)
  )
```

    ## # A tibble: 1 x 2
    ##   avg_samp_mean sd_samp_mean
    ##           <dbl>        <dbl>
    ## 1          1.99        0.591

``` r
sim_results %>% 
  pivot_longer(
    mu_hat:sigma_hat,
    names_to = "parameter", 
    values_to = "estimate") %>% 
  group_by(parameter) %>% 
  summarize(
    emp_mean = mean(estimate),
    emp_sd = sd(estimate)) %>% 
  knitr::kable(digits = 3)
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

| parameter  | emp\_mean | emp\_sd |
| :--------- | --------: | ------: |
| mu\_hat    |     1.992 |   0.591 |
| sigma\_hat |     2.954 |   0.412 |
