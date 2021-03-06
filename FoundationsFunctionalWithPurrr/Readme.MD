# Foundations of Functional Programming with purrr
## Auriel Fournier

# Simplifying Iteration and Lists With purrr
- Class is about purrr package.
- For loops are powerful but is where lots of typos show up.
- The *purrr* package puts the iteration into a single function.
- The function `map()` takes two arguments:
  1. `.x` which is a list or vector.
  2. `.f` which is the function to apply to.
- Lists require a different grammar than vectors.
- Lists can store different data types.
- When you use the variants of `map` that include the type, then you can get vectors back.
- For example, `map_dbl()` will return a vector of numbers.


# More Complex Iterations
- The `purrr` package works well with pipes `%>%`.
- You can always set names by using the function `names(<list>)`.
- `map()` makes simulating data simple.
```
bird_measurements %>%
   map_df(~ data.frame(weight=.x[["weight"]],
                       wing_length = .x[["wing length"]]))
```
- When we have to check against multiple datasets, then we can use functions `pmap()` or `map2()`.
```
# Map over the master list
list_of_files_pmap <- pmap(pmapinputs,
  function(sites, means, sigma, means2, sigma2)
    data.frame(sites = sites,
        a = rnorm(mean = means, n = 200, sd = sigma),
        b = rnorm(mean = means2, n = 200, sd = sigma2)))
```
- I know all this already.


# Troubleshooting Lists with purrr
- When you have large lists to iterate over, errors can be hard to manage.
- You can use the function `safely()` to deal with those situations.
- You can pass the parameter `otherwise` to the function `safely()` to control how it behaves with those issues.
```
# Map safely over log
a <- list(-10, 1, 10, 0) %>%
      map( safely(log, otherwise = NA_real_)) %>%
    # Transpose the result
      transpose()
```
- You can find the successes in the item `result`.
- You can find the failures in the item `error`.
- You can tell `safely()` to print errors as it finds them by passing the parameter `quiet = FALSE`.
- Once you have diagnosed the issues with `safely()` you can then use the function `possibly()` to only return results.
- One excellent use of the function `walk()` is to assist with cleaner output to look through.
- You can also build a set of figures and print all of them using `walk( plist, print)`.
```
# Load the gap_split data
data(gap_split)

# Map over the first 10 elements of gap_split
plots <- map2(gap_split[1:10],
              names(gap_split[1:10]),
              ~ ggplot(.x, aes(year, lifeExp)) +
                geom_line() +
                labs(title = .y))

# Object name, then function name
walk(plots, print)
```

# Problem Solving with purrr
- We'll now explore adding this to our workflow.
```
# Determine who has the most public repositories
map_int(gh_users, ~.x[["public_repos"]]) %>%
      set_names(map_chr(gh_users, "name")) %>%
    sort()
```
- You can use the function `map_df()` to iterate over a data frame.
```
bird_measurements %>%
  map_df(~ data_frame(
  weight = .x[["weight"]],
  wing_length = .x[["wing length"]],
  taxa = "bird")) %>%
     select_if(is.numeric)  %>%
     summary(.x)
```
```
birddf <- bird_measurements %>%
 map_df(~ data_frame(
            wing_length = .x[["wing length"]],
            weight = .x[["weight"]])) %>%
    ggplot(aes(x = weight,
            y = wing_length))+
    geom_point()
```


# Research:

# Reference:
