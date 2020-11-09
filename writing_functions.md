Writing Functions
================

## Do something simple – z scores

``` r
x_vec = rnorm(25, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.1149085 -0.9042752 -0.4444011  0.6758065 -1.4900510  0.4909582
    ##  [7]  0.2041542  0.2446691 -1.2722144  2.0961573 -0.8073393  1.1867804
    ## [13]  0.5222066 -1.1963087 -1.0183822  0.5369664  1.0808776 -1.0360235
    ## [19] -1.1291839  0.4672429  0.9003305  0.4747716 -0.6099903 -0.8265648
    ## [25]  1.7389045

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

    ##  [1]  0.1149085 -0.9042752 -0.4444011  0.6758065 -1.4900510  0.4909582
    ##  [7]  0.2041542  0.2446691 -1.2722144  2.0961573 -0.8073393  1.1867804
    ## [13]  0.5222066 -1.1963087 -1.0183822  0.5369664  1.0808776 -1.0360235
    ## [19] -1.1291839  0.4672429  0.9003305  0.4747716 -0.6099903 -0.8265648
    ## [25]  1.7389045

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
