Writing Functions
================

## Do something simple – z scores

``` r
x_vec = rnorm(25, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.94243403 -0.08140855 -0.65173967  0.01224872 -0.99033556 -0.12046304
    ##  [7]  0.03876035 -0.76266703  0.86281125  2.09162885  2.42058249  0.18717803
    ## [13]  1.80532613 -0.98300581  0.50448617  0.55837694 -0.52803323 -0.89870418
    ## [19] -0.41045975 -0.54751448  0.37737154  0.34145829 -0.29681853 -0.60053532
    ## [25] -0.38610957

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

    ##  [1] -1.94243403 -0.08140855 -0.65173967  0.01224872 -0.99033556 -0.12046304
    ##  [7]  0.03876035 -0.76266703  0.86281125  2.09162885  2.42058249  0.18717803
    ## [13]  1.80532613 -0.98300581  0.50448617  0.55837694 -0.52803323 -0.89870418
    ## [19] -0.41045975 -0.54751448  0.37737154  0.34145829 -0.29681853 -0.60053532
    ## [25] -0.38610957

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
    ## 1  4.20  3.02

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
    ## 1   2.28      2.41

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
    ## 1   6.13      3.05

``` r
sim_mean_sd(samp_size = 100, sigma = 3, mu = 6)
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   5.83      2.67

``` r
sim_mean_sd(samp_size = 1000)
```

    ## # A tibble: 1 x 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   3.18      3.95

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

what about the next page of reviews?

let’s turn that code into a function.

``` r
read_page_reviews = function(url) {
  
  html = read_html(url)
  
  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
  )
  
  reviews
}
```

let me try my function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                               stars text                               
    ##    <chr>                               <dbl> <chr>                              
    ##  1 Movie is still silly fun....amazon…     1 We are getting really frustrated w…
    ##  2 Brilliant and awkwardly funny.          5 I've watched this movie repeatedly…
    ##  3 Great purchase price for great mov…     5 Great movie and real good digital …
    ##  4 Movie for memories                      5 I've been looking for this movie t…
    ##  5 Love!                                   5 Love this movie. Great quality     
    ##  6 Hilarious!                              5 Such a funny movie, definitely bro…
    ##  7 napoleon dynamite                       5 cool movie                         
    ##  8 Top 5                                   5 Best MOVIE ever! Funny one liners …
    ##  9 👍                                      5 Exactly as described and came on t…
    ## 10 A top favorite movie !!                 5 Love this movie, needed to add it …

Let’s read a few pages of reviews.

``` r
url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
vec_urls = str_c(url_base, 1:5)

dynamite_reviews = bind_rows(
  read_page_reviews(vec_urls[1]),
  read_page_reviews(vec_urls[2]),
  read_page_reviews(vec_urls[3]),
  read_page_reviews(vec_urls[4]),
  read_page_reviews(vec_urls[5])
)

dynamite_reviews
```

    ## # A tibble: 50 x 3
    ##    title                  stars text                                            
    ##    <chr>                  <dbl> <chr>                                           
    ##  1 Just watch the freaki…     5 "Its a great movie, gosh!!"                     
    ##  2 Great Value                5 "Great Value"                                   
    ##  3 I LOVE THIS MOVIE          5 "THIS MOVIE IS SO FUNNY ONE OF MY FAVORITES"    
    ##  4 Don't you wish you co…     5 "Watch it 100 times. Never. Gets. Old."         
    ##  5 Stupid, but very funn…     5 "If you like stupidly funny '90s teenage movies…
    ##  6 The beat                   5 "The best"                                      
    ##  7 Hilarious                  5 "Super funny! Loved the online rental."         
    ##  8 Love this movie            5 "We love this product.  It came in a timely man…
    ##  9 Entertaining, limited…     4 "Entertainment level gets a 5 star but having p…
    ## 10 Boo                        1 "We rented this movie because our Adventure Dat…
    ## # … with 40 more rows

## Mean scoping example

``` r
f = function(x) {
  z = x + y
  z
}


x = 1
y = 2

f(x = y)
```

    ## [1] 4

## functions as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
  
}


x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.609755

``` r
median(x_vec)
```

    ## [1] 2.507016

``` r
my_summary(x_vec, mean)
```

    ## [1] 3.609755

``` r
my_summary(x_vec, median)
```

    ## [1] 2.507016

``` r
my_summary(x_vec, IQR)
```

    ## [1] 7.783722
