---
title       : Chapter 1
description : This is a test chapter
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf
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

--- type:RStudioMultipleChoiceExercise xp:50 skills:1 key:a5dc9ea6f9

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

--- type:TabExercise lang:r xp:90 key:cb0d9a46a3

## Tab Exercise

In this exercise we'll take a look at a more subtle example of defining and using linear models. ggplot2 and the Vocab data frame are already loaded for you.

*** =pre_exercise_code

```{r}
library(ggplot2)
theme_set(theme_gray())
library(car)
library(RColorBrewer)
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
# !   DON'T REMOVE THIS SUBSAMPLING PLEASE, BACKEND CAN'T HANDLE BIGGER DATASET TO PLOT   !
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
set.seed(1)
Vocab <- car::Vocab
Vocab <- Vocab[sample(1:nrow(Vocab), nrow(Vocab)*0.1),]
```


*** =sample_code

```{r}
ggplot(Vocab, aes(x = education, y = vocabulary)) +
  geom_jitter(alpha = 0.2)
```

*** =type1:NormalExercise
*** =key1: 33c3544039
*** =xp1: 30

*** =instructions1

Add a `stat_smooth()` layer with method set to "lm" and `se = FALSE` to this jittered plot of `vocabulary` against `education`.

*** =solution1

```{r}
# Plot 1: Jittered scatter plot, add a linear model (lm) smooth
ggplot(Vocab, aes(x = education, y = vocabulary)) +
  geom_jitter(alpha = 0.2) +
  stat_smooth(method = "lm", se = F)
```

*** =type2:NormalExercise
*** =key2: 636df4c77e
*** =xp2: 30

*** =instructions2

Update the plot so that points are colored by `year`. You should remove the `geom_jitter` layer from the plot.


*** =solution2

```{r}
# Plot 2: Only lm, colored by year
ggplot(Vocab, aes(x = education, y = vocabulary, col = factor(year))) +
  stat_smooth(method = "lm", se = F)
```


*** =type3:NormalExercise
*** =key3: adabd6e0ef
*** =xp3: 30

*** =instructions3

Use a sequential color palette to make the plot prettier. This can be done by setting `col = year` and `group = factor(year)` in the `ggplot` call. Use `scale_color_gradientn` with colors drawn from a ColorBrewer palette.

*** =solution3

```{r}
# Plot 4: Change col and group, specify alpha, size and geom, and 
# add scale_color_gradient
ggplot(Vocab, aes(x = education, y = vocabulary, col = year, group = factor(year))) +
  stat_smooth(method = "lm", se = F, alpha = 0.6, size = 2) +
  scale_color_gradientn(colors = brewer.pal(9,"YlOrRd"))
```





--- type:NormalExercise lang:r xp:100 skills:1 key:3279d9b09b

## Base Graphics vs. Ggplot2 - Part 1

To better appreciate `ggplot2` and understand how it works differently from base package, let us create a scatterplot of `hp` (horsepower) vs `drat` (rear axle ration) colored by `cyl` (number of cylinders).

```{r}
# Base Graphics
plot(mtcars$hp, mtcars$drat, col = as.factor(mtcars$cyl))

# Ggplot2
ggplot(mtcars, aes(x = wt, y = mpg, col = as.factor(cyl))) +
  geom_point()
```

Note that in both cases, we convert `cyl` to a factor as it represents discrete categories.


*** =instructions

Create a scatterplot of `mpg` vs `wt` colored by `gear` 

- Use base graphics
- Use ggplot2

*** =hint

*** =pre_exercise_code
```{r}
library(ggplot2)
```

*** =sample_code
```{r}
# Base Graphics


# Ggplot2


```


*** =solution
```{r}

```

*** =sct
```{r}

```



--- type:NormalExercise lang:r xp:100 skills:1 key:564fc332a9

## Base Graphics vs. ggplot2 (Part 2)

Now suppose that we want to add a linear model to the scatterplot of `hp` vs `drat` colored by `cyl`. 

We can do that in base graphics by (1) plotting the base scatterplot, (2) fitting a linear model to each subset of data (split by number of cylinders), and then (3) calling the function `abline` to plot the regression lines. You can copy-paste this code into the console to try it out.

```{r}
plot(mtcars$hp, mtcars$drat, col = as.factor(mtcars$cyl))
for (cy in unique(mtcars$cyl)){
  lm_mod <- lm(drat ~ hp, data = mtcars, subset = (cyl == cy))
  abline(lm_mod, lty = 2)
}
```


*** =instructions

Use base graphics to

- Create a scatterplot of `mpg` vs `wt` colored by `gear`
- Add a regression line for each value of `gear`.


*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# Use base graphics




# Use ggplot2



```

*** =solution
```{r}

```

*** =sct
```{r}

```



--- type:NormalExercise lang:r xp:100 skills:1 key:9cf0b8a51b

## Base Graphics vs. ggplot2 (Part 3)

The same scatterplot of `hp` vs `drat` colored by `cyl`, with a fitted regression line, can be achieved in `ggplot2` in a much more concise way by adding a `geom_smooth` layer

```{r}
ggplot(mtcars, aes(x = hp, y = drat, col = as.factor(cyl))) +
  geom_point() +
  geom_smooth(method = "lm", se = F)
```

Note how `ggplot2` automatically fits multiple regression lines based on the number of cylinders.


*** =instructions

Use ggplot2 to

- Create a scatterplot of `mpg` vs `wt` colored by `gear`
- Add a regression line for each value of `gear`.

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}

```
