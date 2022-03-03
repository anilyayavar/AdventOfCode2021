``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.5     v dplyr   1.0.7
    ## v tidyr   1.2.0     v stringr 1.4.0
    ## v readr   2.0.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(digest)
library(unglue)
```

### Day 1

### [Not Quite Lisp](https://adventofcode.com/2015/day/1)

#### Part-1

``` r
input <- readLines('input1.txt')
```

    ## Warning in readLines("input1.txt"): incomplete final line found on 'input1.txt'

``` r
# part-1
answer1_1 <- function(input){
  input_vec <- input %>% str_split('') %>% unlist
  floors <- c(-1, 1)[1+(input_vec == '(')]
  sum(floors)
}

answer1_1(input)
```

    ## [1] 74

#### part-2

``` r
answer1_2 <- function(input){
  input_vec <- input %>% str_split('') %>% unlist
  floors <- c(-1, 1)[1+(input_vec == '(')]
  # answer
  which.max(cumsum(floors)<0)
}

answer1_2(input)
```

    ## [1] 1795

### Day2

### [I Was Told There Would Be No Math](https://adventofcode.com/2015/day/2)

#### Part-1

``` r
input <- readLines('input2.txt')

#part-1
answer2_1 <- function(input){
  input_list <- input %>% 
    str_split('x') %>% 
    map(as.integer)
  
  
  l <- map_int(input_list, ~ sort(.x)[1])
  b <- map_int(input_list, ~ sort(.x)[2])
  h <- map_int(input_list, ~ sort(.x)[3])
  #answer
  sum(3*l*b + 2*l*h + 2*b*h)
}

answer2_1(input)
```

    ## [1] 1606483

#### Part-2

``` r
answer2_2 <- function(input){
  input_list <- input %>% 
    str_split('x') %>% 
    map(as.integer)
  
  
  l <- map_int(input_list, ~ sort(.x)[1])
  b <- map_int(input_list, ~ sort(.x)[2])
  h <- map_int(input_list, ~ sort(.x)[3])
  #answer
  sum(2*(l+b)+l*b*h)
}

answer2_2(input)
```

    ## [1] 3842356

### Day 3

### [Perfectly Spherical Houses in a Vacuum](https://adventofcode.com/2015/day/3)

#### Part-1

``` r
input <- readLines('input3.txt')
```

    ## Warning in readLines("input3.txt"): incomplete final line found on 'input3.txt'

``` r
## part-1
answer3_1 <- function(input){
  input_vec <- input %>% 
    str_split('') %>% 
    unlist
  
  input_vec <- ifelse(input_vec== '>', 1, 
                      ifelse(input_vec == '<', -1, 
                             ifelse(input_vec == '^', 1i, -1i)))
  
  input_vec <- c(0+0i, input_vec)
  # answer
  length(unique(cumsum(input_vec)))
}

answer3_1(input)
```

    ## [1] 2572

#### Part-2

``` r
answer3_2 <- function(input){
  input_vec <- input %>% 
    str_split('') %>% 
    unlist
  
  input_vec <- ifelse(input_vec== '>', 1, 
                      ifelse(input_vec == '<', -1, 
                             ifelse(input_vec == '^', 1i, -1i)))
  #answer
  split(input_vec, seq_along(input_vec) %% 2) %>% 
    map(~ c(0+0i, .x) %>% 
          cumsum()) %>% 
    reduce(union) %>% 
    length
}

answer3_2(input)
```

    ## [1] 2631

### Day-4

### [The Ideal Stocking Stuffer](https://adventofcode.com/2015/day/4)

#### Part-1

``` r
answer4_1 <- function(first_part){
  second_part <- 1
  while(!startsWith(digest(paste0(first_part, second_part), algo = 'md5', serialize = FALSE), '00000')){
    second_part <- second_part + 1
  }
  second_part
}

answer4_1('ckczppom')
```

    ## [1] 117946

#### Part-2

``` r
answer4_2 <- function(first_part){
  second_part <- 1
  while(!startsWith(digest(paste0(first_part, second_part), algo = 'md5', serialize = FALSE), '000000')){
    second_part <- second_part + 1
  }
  second_part
}

answer4_2('ckczppom')
```

    ## [1] 3938038

### Day 5

### [Doesn’t He Have Intern-Elves For This?](https://adventofcode.com/2015/day/5)

#### Part-1

``` r
input <- readLines('input5.txt')
answer5_1 <- function(input){
  # vowels
  rule1_st <- c('a', 'e', 'i', 'o', 'u')
  # check for at least 3 vowels
  rule1 <- map_lgl(str_split(input, '') , ~ sum(.x %in% rule1_st) >= 3)
  # check for repeated alphabets
  rule2 <- map_lgl(str_split(input, ''), function(.a)any(map_lgl(seq(length(.a)-1), ~.a[.x]==.a[.x+1])))
  # rule 3 exclusion strings
  rule3_st <- c('ab', 'cd', 'pq', 'xy')
  # check for exclusions
  rule3 <- !map_lgl(input, function(.a)any(map_lgl(rule3_st, ~str_detect(.a, .x))))
  # answer (nice strings)
  sum(rule1 & rule2 & rule3)
}

answer5_1(input)
```

    ## [1] 258

#### Part-2

### Day 6

### [Day 6: Probably a Fire Hazard](https://adventofcode.com/2015/day/6)

#### Part-1

``` r
input <- readLines('input6.txt')

answer6_1 <- function(input){
  
  # first define a helper function
  my_fun <- function(.x, .y){
    if (.y$ins == 'turn on'){
      .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] <- TRUE
      .x
    } else if (.y$ins == 'turn off'){
      .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] <- FALSE
      .x
    } else {
      .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] <- !(.x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)])
      .x
    }
  }
  
  # read the input properly
  patterns <- '{ins} {x1=\\d+},{y1=\\d+} through {x2=\\d+},{y2=\\d+}'
  input_inst <- unglue::unglue(input, patterns, convert = TRUE)
  
  #prepare initial configuration of lights
  mat <- matrix(FALSE, nrow = 1000, ncol=1000)
  
  #iteration
  output <- reduce(input_inst, .init = mat,
       my_fun)
  
  # answer
  sum(output)
  
}

answer6_1(input)
```

    ## [1] 400410

#### Part-2

It has good usage of r’s `pmax` function

``` r
input <- readLines('input6.txt')

answer6_2 <- function(input){
  
  # first define a helper function
  my_fun <- function(.x, .y){
    if (.y$ins == 'turn on'){
      .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] <- .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] + 1L
      .x
    } else if (.y$ins == 'turn off'){
      .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] <- pmax(0, .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] -1L)
      .x
    } else {
      .x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)] <- (.x[seq(.y$x1 +1, .y$x2 +1, 1), seq(.y$y1 +1, .y$y2 +1, 1)]) + 2L
      .x
    }
  }
  
  # read the input properly
  patterns <- '{ins} {x1=\\d+},{y1=\\d+} through {x2=\\d+},{y2=\\d+}'
  input_inst <- unglue::unglue(input, patterns, convert = TRUE)
  
  #prepare initial configuration of lights
  mat <- matrix(FALSE, nrow = 1000, ncol=1000)
  
  #iteration
  output <- reduce(input_inst, .init = mat,
       my_fun)
  
  # answer
  sum(output)
}

answer6_2(input)
```

    ## [1] 15343601

### Day-7

### [Day 7: Some Assembly Required](https://adventofcode.com/2015/day/7)

#### Part-1

### Day-8

### Day-9

### Day-10

### [— Day 10: Elves Look, Elves Say —](https://adventofcode.com/2015/day/10)

#### Part-1

``` r
answer10_1 <- function(input){
  nchar(reduce(1:40, .init = input, ~ .x %>% str_split('') %>%                # used purrr::reduce instead of for loop just for personal liking  
  unlist %>% 
  rle %>%                                                                     # used base r's rle function
  {c(rbind(as.character(.$lengths), .$values))} %>%                           # interleaved two vectors by c and rbind
    paste0(collapse = '')))                                                   # pasted them back
}

answer10_1('1113122113')
```

    ## [1] 360154

#### Part-II

``` r
answer10_2 <- function(input){
  nchar(reduce(1:50, .init = input, ~ .x %>% str_split('') %>% 
  unlist %>% 
  rle %>% 
  {c(rbind(as.character(.$lengths), .$values))} %>% 
    paste0(collapse = '')))
}

answer10_2('1113122113')
```

    ## [1] 5103798

### Day-11

### [— Day 11: Corporate Policy —](https://adventofcode.com/2015/day/11)

#### Part-1

### Day-12

### [— Day 12: JSAbacusFramework.io —](https://adventofcode.com/2015/day/12)

#### Part-1

``` r
input <- readLines('input11.txt')

answer12_1 <- function(input){
  str_extract_all(input, pattern = '[+-]?\\d+(?:\\.\\d+)?') %>%                # capture all numbers through regex
  unlist %>% 
  as.numeric() %>% 
  sum()
}

answer12_1(input)
```

    ## [1] 111754

#### Part-2

### Day-13

### [— Day 13: Knights of the Dinner Table —](https://adventofcode.com/2015/day/13)

#### Part-1

``` r
input <- readLines('input13.txt')

answer13_1 <- function(input){
  
  patterns = '{V1} would {op} {happiness} happiness units by sitting next to {V2}.'      # Pattern vector for unglue
  
  dat <- unglue_data(input, patterns = patterns, convert = TRUE) %>% 
    mutate(happiness = ifelse(op == 'lose', happiness*-1, happiness))
  
  x <- unique(dat$V1)[-1]                                                                # since seating arrangements are circular assume Alice sitting first
  
  
  h_list = list()
  counter <- 1
  
  for(i1 in x){                                                                         # seven for loops initiated
    x1 <- setdiff(x, i1)
    for(i2 in x1){
      x2 <- setdiff(x1, i2)
      for(i3 in x2){
        x3 <- setdiff(x2, i3)
        for(i4 in x3){
          x4 <- setdiff(x3, i4)
          for(i5 in x4){
            x5 <- setdiff(x4, i5)
            for(i6 in x5){
              i7 <- setdiff(x5, i6)
              h_list[[counter]] <- vector()
              h_list[[counter]]['Alice'] <- sum(dat[(dat$V1 == 'Alice' & dat$V2 == i1) | (dat$V1== 'Alice' & dat$V2==i7), 'happiness']) 
              h_list[[counter]][i1] <- sum(dat[(dat$V1 == i1 & dat$V2 == i2) | (dat$V1==i1 & dat$V2=='Alice'), 'happiness'])
              h_list[[counter]][i2] <- sum(dat[(dat$V1 == i2 & dat$V2 == i3) | (dat$V1==i2 & dat$V2==i1), 'happiness'])
              h_list[[counter]][i3] <- sum(dat[(dat$V1 == i3 & dat$V2 == i4) | (dat$V1==i3 & dat$V2==i2), 'happiness'])
              h_list[[counter]][i4] <- sum(dat[(dat$V1 == i4 & dat$V2 == i5) | (dat$V1==i4 & dat$V2==i3), 'happiness'])
              h_list[[counter]][i5] <- sum(dat[(dat$V1 == i5 & dat$V2 == i6) | (dat$V1==i5 & dat$V2==i4), 'happiness'])
              h_list[[counter]][i6] <- sum(dat[(dat$V1 == i6 & dat$V2 == i7) | (dat$V1==i6 & dat$V2==i5), 'happiness'])
              h_list[[counter]][i7] <- sum(dat[(dat$V1 == i7 & dat$V2 == 'Alice') | (dat$V1==i7 & dat$V2==i6), 'happiness'])
              
              counter <- counter +1
              
            }
          }
        }
      }
    }
  }
  
  
  max(map_dbl(h_list, sum))
}

answer13_1(input)
```

    ## [1] 664

###$ Part-2

``` r
answer13_2 <- function(input){
  
  patterns = '{V1} would {op} {happiness} happiness units by sitting next to {V2}.'  # Pattern vector for unglue
  
  dat <- unglue_data(input, patterns = patterns, convert = TRUE) %>% 
    mutate(happiness = ifelse(op == 'lose', happiness*-1, happiness))
  
  x <- unique(dat$V1)                                                                # This time considering myself at initial point of selection
  
  x <- unique(dat$V1)
  h_list = list()
  counter <- 1
  
  for(i1 in x){                                                                     # initiating 8 for loops this time
    x1 <- setdiff(x, i1)
    for(i2 in x1){
      x2 <- setdiff(x1, i2)
      for(i3 in x2){
        x3 <- setdiff(x2, i3)
        for(i4 in x3){
          x4 <- setdiff(x3, i4)
          for(i5 in x4){
            x5 <- setdiff(x4, i5)
            for(i6 in x5){
              x6 <- setdiff(x5, i6)
              for(i7 in x6){
                
                i8 <- setdiff(x6, i7)
                h_list[[counter]] <- vector()
                h_list[[counter]][i1] <- sum(dat[(dat$V1 == i1 & dat$V2 == i2), 'happiness'])
                h_list[[counter]][i2] <- sum(dat[(dat$V1 == i2 & dat$V2 == i3) | (dat$V1==i2 & dat$V2==i1), 'happiness'])
                h_list[[counter]][i3] <- sum(dat[(dat$V1 == i3 & dat$V2 == i4) | (dat$V1==i3 & dat$V2==i2), 'happiness'])
                h_list[[counter]][i4] <- sum(dat[(dat$V1 == i4 & dat$V2 == i5) | (dat$V1==i4 & dat$V2==i3), 'happiness'])
                h_list[[counter]][i5] <- sum(dat[(dat$V1 == i5 & dat$V2 == i6) | (dat$V1==i5 & dat$V2==i4), 'happiness'])
                h_list[[counter]][i6] <- sum(dat[(dat$V1 == i6 & dat$V2 == i7) | (dat$V1==i6 & dat$V2==i5), 'happiness'])
                h_list[[counter]][i7] <- sum(dat[(dat$V1 == i7 & dat$V2 == i8) | (dat$V1==i7 & dat$V2==i6), 'happiness'])
                h_list[[counter]][i8] <- sum(dat[(dat$V1 == i8 & dat$V2 == i7), 'happiness'])
                
                counter <- counter +1
                
              }
            }
          }
        }
      }
    }
  }
  
  max(map_dbl(h_list, sum))
  
}

answer13_2(input)
```

    ## [1] 640

### Day-14

### [— Day 14: Reindeer Olympics —](https://adventofcode.com/2015/day/13)

#### Part-1

``` r
input <- readLines('input14.txt')

answer14_1 <- function(input, time=2503L){
  
  patterns = '{name} can fly {fly} km/s for {time_fly} seconds, but then must rest for {time_rest} seconds.'
  dat <- unglue::unglue_data(input, patterns = patterns, convert = TRUE)
  # let's create a helper function
  my_fun <- function(.x){
    x <- c(rep(dat$fly[.x], dat$time_fly[.x]), rep(0, dat$time_rest[.x]))
    y <- (time %/% (dat$time_fly[.x]+dat$time_rest[.x]))+1
    z <- rep(x, y)
    
    cumsum(z)[time]
    
  }
  
  max(map_dbl(seq(nrow(dat)), ~my_fun(.x)))
}

answer14_1(input)
```

    ## [1] 2660

#### Part-2

``` r
answer14_2 <- function(input, time=2503L){
  
  patterns = '{name} can fly {fly} km/s for {time_fly} seconds, but then must rest for {time_rest} seconds.'
  dat <- unglue::unglue_data(input, patterns = patterns, convert = TRUE)
  # let's create a helper function
  mat_gen <- function(reindeer, step){
    x <- c(rep(dat$fly[reindeer], dat$time_fly[reindeer]), rep(0, dat$time_rest[reindeer]))
    y <- (time %/% (dat$time_fly[reindeer]+dat$time_rest[reindeer]))+1
    z <- rep(x, y)[seq_len(time)]
    z[step]
  }
  # create a matrix for steps taken after each second
  my_mat <- outer(seq(nrow(dat)), seq(time), Vectorize(mat_gen))
  
  # cumulative positive
  pos <- t(apply(my_mat, 1, cumsum))
  
  # final answer
  max(t(apply(apply(pos, 2, function(x) x == max(x)), 1, cumsum))[, time])
}

answer14_2(input)
```

    ## [1] 1256

### Day-15

### [— Day 15: Science for Hungry People —](https://adventofcode.com/2015/day/15)

#### Part-1

#### Part-2

### Day-16

### [— Day 16: Aunt Sue —](https://adventofcode.com/2015/day/16)

#### Part-1

#### Part-2

### Day-17

### [— Day 17: No Such Thing as Too Much —](https://adventofcode.com/2015/day/17)

#### Part-1

#### Part-2

### Day-18

### [— Day 18: Like a GIF For Your Yard —](https://adventofcode.com/2015/day/18)

#### Part-1

#### Part-2