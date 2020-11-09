Writing Functions
================

## Do something simple – z scores

``` r
x_vec = rnorm(25, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.62360465  0.90251703 -0.91937416  1.19853659  0.62013060 -0.26439792
    ##  [7]  0.76514786  0.30811645  1.34472027  1.70666098 -0.31491956 -1.34820929
    ## [13] -0.01038397  0.14715021  1.19913987 -0.78329528 -1.43079193 -1.26791511
    ## [19] -0.19229546  0.22804699 -1.07245364  1.54474619  0.54006138 -0.76926069
    ## [25] -0.50807276

I want a function to compute z-scores.

``` r
z_scores = function(x) {
  
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } 
  
  else if (length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x_vec)
```

    ##  [1] -1.62360465  0.90251703 -0.91937416  1.19853659  0.62013060 -0.26439792
    ##  [7]  0.76514786  0.30811645  1.34472027  1.70666098 -0.31491956 -1.34820929
    ## [13] -0.01038397  0.14715021  1.19913987 -0.78329528 -1.43079193 -1.26791511
    ## [19] -0.19229546  0.22804699 -1.07245364  1.54474619  0.54006138 -0.76926069
    ## [25] -0.50807276

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is Jeff")
```

    ## Error in z_scores("my name is Jeff"): Argument x should be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Argument x should be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Argument x should be numeric

## Multiple outputs

``` r
mean_and_sd_output_list = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  list(mean = mean_x, 
       sd = sd_x)
}
```

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

Check that the function works.

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.40  3.09

## Multiple inputs

I’d like to do this with a function.

``` r
sim_data = 
  tibble(
  x = rnorm(30, mean = 2, sd = 3)
)

sim_data %>% 
  summarize(
    mu_hat = mean(x),
    sigma_hat = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   1.77      2.83

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4) {
  
  sim_data = 
    tibble(
    x = rnorm(n = samp_size, mean = mu, sd = sigma)
  )
  
  sim_data %>% 
    summarize(
      mu_hat = mean(x),
      sigma_hat = sd(x)
    )
}

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   5.94      2.87

``` r
sim_mean_sd(samp_size = 100, sigma = 3, mu = 6)
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   6.00      2.92

``` r
sim_mean_sd(samp_size = 1000)
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   2.72      4.08
