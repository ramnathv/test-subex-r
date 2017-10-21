---
title       : Chapter 1
description : This is a test chapter
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:8004c53525
## A really bad movie

Have a look at the plot that showed up in the viewer to the right. Which type of movie has the worst rating assigned to it?

*** =instructions
- Adventure
- Action
- Animation
- Comedy

*** =hint
Have a look at the plot. Which color does the point with the lowest rating have?

*** =pre_exercise_code
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer

movies <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/introduction_to_r/movies.csv")

library(ggplot2)

ggplot(movies, aes(x = runtime, y = rating, col = genre)) + geom_point()
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

msg_bad <- "That is not correct!"
msg_success <- "Exactly! There seems to be a very bad action movie in the dataset."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:795e7ff71c
## More movies

In the previous exercise, you saw a dataset about movies. In this exercise, we'll have a look at yet another dataset about movies!

A dataset with a selection of movies, `movie_selection`, is available in the workspace.

*** =instructions
- Check out the structure of `movie_selection`.
- Select movies with a rating of 5 or higher. Assign the result to `good_movies`.
- Use `plot()` to  plot `good_movies$Run` on the x-axis, `good_movies$Rating` on the y-axis and set `col` to `good_movies$Genre`.

*** =hint
- Use `str()` for the first instruction.
- For the second instruction, you should use `...[movie_selection$Rating >= 5, ]`.
- For the plot, use `plot(x = ..., y = ..., col = ...)`.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code
load(url("https://s3.amazonaws.com/assets.datacamp.com/course/teach/movies.RData"))
movie_selection <- Movies[Movies$Genre %in% c("action", "animated", "comedy"), c("Genre", "Rating", "Run")]

# Clean up the environment
rm(Movies)
```

*** =sample_code
```{r}
# movie_selection is available in your workspace

# Check out the structure of movie_selection


# Select movies that have a rating of 5 or higher: good_movies


# Plot Run (i.e. run time) on the x axis, Rating on the y axis, and set the color using Genre

```

*** =solution
```{r}
# movie_selection is available in your workspace

# Check out the structure of movie_selection
str(movie_selection)

# Select movies that have a rating of 5 or higher: good_movies
good_movies <- movie_selection[movie_selection$Rating >= 5, ]

# Plot Run (i.e. run time) on the x axis, Rating on the y axis, and set the color using Genre
plot(good_movies$Run, good_movies$Rating, col = good_movies$Genre)
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_function("str", args = "object",
              not_called_msg = "You didn't call `str()`!",
              incorrect_msg = "You didn't call `str(object = ...)` with the correct argument, `object`.")

test_object("good_movies")

test_function("plot", args = "x")
test_function("plot", args = "y")
test_function("plot", args = "col")

test_error()

success_msg("Good work!")
```

--- type:BulletExercise lang:r xp:150 key:79e232200d

## Building a plot!

*** =pre_exercise_code
```{r}
library(ggplot2)
```

*** =sample_code
```{r}
ggplot(mtcars, aes(x = mpg, y = wt))
```

*** =type1:NormalExercise
*** =xp1: 30
*** =key1: 1db0bdc483


*** =instructions1
Use `geom_point` to add a scatterplot

*** =hint1
Submit the query!

*** =solution1
```{r}
ggplot(mtcars, aes(x = mpg, y = wt)) +
  geom_point()
```

*** =type2:NormalExercise
*** =key2: 555a6267d7
*** =xp2: 30

*** =instructions2
Use `geom_smooth` with `method` set to `lm` to add a regression line

*** =hint2
Submit the query!

*** =solution2
```{r}
ggplot(mtcars, aes(x = mpg, y = wt)) +
  geom_point() +
  geom_smooth(method = 'lm', se = F)
```

--- type:TabExercise lang:r xp:100 key:dbbca81c55

## Advanced Group By Exercises

By now you've learned the fundamentals of `dplyr`: the five data manipulation verbs and the additional `group_by()` function to discover interesting group-wise statistics. This exercise brings together these concepts and provides you with an opportunity to combine them to answer some interesting questions.

Let us suppose we want to find the most visited destination for each carrier. Before reading ahead, please spend a couple of minutes thinking about how you might go about solving this exercise using dplyr.

As this is the first time you are combining multiple dplyr concepts, we have broken this exercise down into smaller steps. Each step will allow you to focus on a specific concept.

*** =pre_exercise_code

```{r}
library(dplyr)
library(hflights)
```

*** =sample_code

```{r}
hflights %>%
  
```


*** =type1:NormalExercise 
*** =key1: 421e55e2ec

*** =xp1: 30

*** =instructions1

Compute for every carrier, the aggregate number of visits to each destination.

*** =solution1

```{r}
hflights %>% 
  group_by(UniqueCarrier, Dest) %>%
  summarise(n = n())
```

*** =type2:NormalExercise 
*** =key2: 6eb56c5f40

*** =xp2: 30

*** =instructions2

Rank the aggregate number of visits for every carrier.

*** =solution2

```{r}
hflights %>% 
  group_by(UniqueCarrier, Dest) %>%
  summarise(n = n()) %>%
  mutate(rank = rank(desc(n)))
```


*** =type3:NormalExercise 
*** =key3: be3d94b96c

*** =xp3: 30

*** =instructions3

Filter the results to only return the top ranked destination for every carrier.

*** =solution3

```{r}
hflights %>% 
  group_by(UniqueCarrier, Dest) %>%
  summarise(n = n()) %>%
  mutate(rank = rank(desc(n))) %>%
  filter(rank == 1)
```

--- type:BulletExercise lang:r key:5189de74d5

## Selecting groups or parts of groups

The previous exercise illustrated how you can manually set a key via `setkey(DT, A, B)`. `setkey()` sorts the data by the columns that you specify and changes the table by reference. Having set a key will allow you to use it, for example, as a super-charged row name when doing selections. Arguments like `mult` and `nomatch` then help you to select only particular parts of groups.

For this exercise, we have already created a new data table named `DT` with keys set to `A` and `B`.

```r
# The 'keyed' data.table DT
DT <- data.table(
  A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
  B = c(5, 4, 1, 9, 8, 8, 6), 
  C = 6:12
)
setkey(DT, A, B)
```

*** =pre_exercise_code

```{r}
library(data.table)
DT <- data.table(
  A = letters[c(2, 1, 2, 3, 1, 2, 3)], 
  B = c(5, 4, 1, 9, 8, 8, 6), 
  C = 6:12
)
setkey(DT, A, B)
```

*** =sample_code

```{r}
# enter your code here
```


*** =type1:NormalExercise
*** =key1: 641a9fce9f

*** =xp1:30
*** =instructions1

Select the "b" group without using ==.

*** =solution1

```{r}
# Select the "b" group
DT["b"]
```
*** =type2:NormalExercise
*** =key2: b8a220e53f

*** =xp2:30

*** =instructions2

Select the "b" and "c" groups, without using ==.

*** =solution2

```{r}
# Select the "b" and "c" groups
DT[c("b", "c")]
```


*** =type3:NormalExercise
*** =key3: 7fa55e05d7

*** =xp3:30
*** =instructions3

Select the first row of the "b" and "c" groups using `mult`.

*** =solution3

```{r}
# The first row of the "b" and "c" groups
DT[c("b", "c"), mult = "first"]
```

*** =type4:NormalExercise
*** =key4: 4652f8cfbb

*** =xp4:30
*** =instructions4

Select the first and last row of the "b" and "c" groups. You will need to use `by = .EACHI` and `.SD` 

*** =solution4

```{r}
# First and last row of the "b" and "c" groups
DT[c("b", "c"), .SD[c(1, .N)], by = .EACHI]
```
*** =type5:NormalExercise
*** =key5: 334e27b0c6

*** =xp5:30

*** =instructions5

Select the first and last row of the "b" and "c" groups and print out the group before returning the tables.

*** =hint5

You can use curly brackets to include two separate instructions inside the `j` argument.

*** =solution5

```{r}
# First and last row of the "b" and "c" groups
DT[c("b", "c"), {print(.SD); .SD[c(1, .N)]}, by = .EACHI]
```

--- type:RStudioMultipleChoiceExercise xp:50 skills:1

## Let's get you set up!

In order for you to use the RStudio IDE on your own computer, you will need to install the appropriate items. Garrett just explained how to do this for Mac, PC and Linux users.

Which website should you visit to download a copy of the R language?

*** =instructions
- rstudio.com/download
- cran.r-project.org
- You do not need to download a copy of the R language

*** =hint
Garrett showed you two websites you need to visit to download the appropriate software.

*** =sct
```{r,eval=FALSE}
msg1 <- "Not quite! You can visit rstudio.com/download[(https://www.rstudio.com/products/rstudio/download/)] to download the RStudio IDE, but not a copy of the R language itself."
msg2 <- "Nice! Let's start taking a look at all the cool stuff you just downloaded."
msg3 <- "Try again! It is important to download a copy of the R language in order to use the RStudio IDE and perform data analysis on your desktop or server."

test_mc(2, feedback_msgs = c(msg1, msg2, msg3))
```

