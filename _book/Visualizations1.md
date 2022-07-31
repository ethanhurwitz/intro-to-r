# (PART\*) Visualizations {-}

# Intro to ggplot2




```r
library(tidyverse)
#> Warning: package 'dplyr' was built under R version 4.0.5
library(palmerpenguins)
```

<!-- <a href="exercises/Exercise_Viz1.Rmd" download>Exercise Sheet</a> -->

> "The greatest value of a picture is when it forces us to notice what we never expected to see."<br>— John Tukey

>"There is no single statistical tool that is as powerful as a well‐chosen graph." (Chambers et al. 1983)

<span style="font-size:14.0pt"><b><u>Why Do We Care About Visualizing Our Data?</u></b></span>

Consider the following scenario:

There are two different datasets, each comprised of individuals measured on two outcomes (say height and weight). The mean and standard deviation of height and of weight are the same in both datasets. The correlation between height and weight is the same in both datasets as well. By every statistical metric, these datasets are the same. So, they must ***look*** similar, right?

Click the button below:

<button class="btn btn-primary" data-toggle="collapse" data-target="#BlockName"> Show/Hide </button>  
<div id="BlockName" class="collapse">  

<img src="figures/DinoSequential.gif" width="100%" />

<img src="figures/datasaurus_dozen.jpeg" width="100%" />
</div>
<br>

This collection of data is called the [datasauRus](https://github.com/jumpingrivers/datasauRus), and more information about how these were generated can be found in  [this](https://www.autodesk.com/research/publications/same-stats-different-graphs) technical paper. This is a modern version of the famous Anscombe's Quartet:

![](figures/anscombes-quartet.jpeg){Width=100%}

All four of these datasets have matching n's, means, standard deviations, and correlations. This means that the slope and intercept, and corresponding statistical tests, will all be equivalent. The data **clearly** look different though. (These data are built into R in an object called `anscombe`, which you can look at to test this for yourself!)

The key point here is that the summary statistics of some variables and their linear relationships inherently overlook some aspects of the data. This highlights the critical importance of visualizing your data, and not just relying on summary statistics alone. The point of a visualization, just like a summary statistic, is to understand a relationship or pattern in your data. However, by looking at the raw data itself, you do not run the risk of missing things the way you do by relying on summary statistics alone.

## When Visualizing Go Wrong

It is easy enough to just say, "Look at your raw data! Create a visualization!" However, a bad visualization is often times worse than no visualization at all. One of the things that the Covid-19 pandemic brought with it originally was a plethora of data visualizations. Below are a few that were observed out in the real world over the last 2 years:

![](figures/bad_visualizations/badviz3.jpeg){width=100%}

![](figures/bad_visualizations/badviz4.jpeg){width=100%}

![](figures/bad_visualizations/badviz2.png){width=100%}

![](figures/bad_visualizations/badviz1.png){width=100%}

So, it is not enough to just make *some* visualization, it is important to also consider the principles that make a ***GOOD*** visualization. Over the next few lessons, both will be covered.



## ggplot2

Data visualization is one of the things that sets R apart from other programming languages that can be used for statistics, like Python. R still has the best data visualization capabilities, and it is one of the primary reasons it is used over Python in Social Sciences. This is also one of the first times you will get to see what coding can be all about. You are actually going to be creating stuff with your code!

In this class, the `ggplot2` package will be used to create visualizations. Graphs are constructed by mapping data to geometric objects (lines, bars, points, etc.) according to some aesthetic attributes (color, shape, size, etc.). `ggplot2` uses this to inform its grammar.
		
![](figures/ggplot2_masterpiece.png){width=100%}
<p style="font-size:6pt">Artwork by @allison_horst</p>

### Meet the Penguins

To create visualizations, you need some data to visualize! The `palmerpenguins` dataset from [Alison Horst](https://allisonhorst.github.io/palmerpenguins/) will be used to create examples throughout.

![](figures/palmerpenguins_logo.png){width=207px}

This dataset contains measurements from 344 different penguins. Measurements include things like bill length and depth, sex, among others.

![](figures/palmerpenguins_bills.png){width=100%}
<p style="font-size:6pt">Artwork by @allison_horst</p>
<br>

There are 3 different species of penguins in this dataset, collected from 3 islands in the Palmer Archipelago, Antarctica.

![](figures/palmerpenguins_species.png){width=100%}
<p style="font-size:6pt">Artwork by @allison_horst</p>

Use the `library(palmerpenguins)` call to load in the dataset.

## A Basic Graph

The most sensible place to start is building a basic graph. There are going to be <u>**LOTS**<u> of exercise breaks throughout.

<p class="text-info"> **<u>Note:</u> As you move forward with building your first graph, each new piece of code added will be accompanied by a literate programming portion, where you will describe in words what the code is doing.**</p>

All ggplot graphs are build using the `ggplot()` call. The first thing it needs is some data. The `%>%` will be used to pass in the `penguins` dataframe.


```r
penguins %>%
  ggplot()
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-5-1.png" width="672" />

> **Start by telling ggplot to use the `penguins` dataframe for the data**

Woo! This actually created something. A grey rectangle! Of course, the code above alone will not do anything. You have to tell ggplot what you want to plot. Hmm... As an example, consider how a penguin's `bill_length_mm` is related to its `flipper_length_mm`. You might imagine that bigger penguins would tend to have longer bills and flippers, but maybe not? That is why you create visualizations, to try and help answer questions you may have about your data (which consist of samples thought to be representative of the world at large)!

ggplot needs to be told what variables from your data should be mapped to the aesthetics you want to render on your graph. You will do that with the `aes()` call, and start with specifying what to display on each axis.


```r
penguins %>%
  ggplot(mapping = aes(y = flipper_length_mm,
                       x = bill_length_mm))
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-6-1.png" width="672" />

>"Start by telling ggplot to use the `penguins` dataframe for the data, **map flipper length to the y-axis and bill length to the x-axis.**"

Above, it was specified that `flipper_length_mm` should be on the y-axis, and `bill_length_mm` on the x-axis (it is good practice to specify *y* first and *x* second). All aesthetic mappings must be separated by a comma, and it does not matter the order you list them (though it is good practice to start with the axes first). As you can see, ggplot figured out how to label the axes on its own. 

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 1**</div>
  <div class="panel-body">Create your own graph using the `penguins` data and pick 2 of the following to put on your x and y axes: `bill_length_mm`, `bill_depth_mm`, `flipper_length_mm`, or `body_mass_g`.</div>
</div>
<br>
ggplot was told what the aesthetic mappings are (what data source to use), but not what it should render from those mappings! You want ggplot to use those mappings to construct some **geom**etric object, and the way you do that is by adding a *geom*, aptly named. There are a number of different geoms, which have the general syntax of `geom_X()`, where X usually refers to the specific geometric object you want to render. Here, you want a point on the graph for each penguin, so `geom_point()` will be used.

<p class="text-info"> **<u>Note:</u> There are a TON of different geoms. You will see many more later.**</p>


```r
penguins %>%
  ggplot(mapping = aes(y = flipper_length_mm,
                       x = bill_length_mm)) +
  geom_point()
#> Warning: Removed 2 rows containing missing values
#> (geom_point).
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-7-1.png" width="672" />

> "Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. **Represent each observation with a point.**"

Okay, **NOW** we are talking! This looks like an actual real graph. Look at your very first ggplot masterpiece!

There are two important things to note here:

1. The `+` was used instead of `%>%` to add on a new function. The ggplot system is additive/layered, which is a very powerful idea that will be explained later. For now, it is enough to know that the `geom_point()` was being added to the code. You do not want to take the `penguins %>% ggplot(mapping = aes(y = flipper_length_mm, x = bill_length_mm))` code and use it as the argument for the `geom_point()` function, which is what would happen if a `%>%` was used to pipe it! You want to just add the points on top of the existing base that code created, which is why the `+` is used.

2. When running this code, in addition to creating the graph (in the "Plots" section of RStudio or inline under your code), the following showed up in the console: 

<p style="color:#A79BF0"> **Warning message:<br>Removed 2 rows containing missing values (geom_point).**</p>

Looking at your warnings and errors is always important, but especially so when creating visualizations! This says that it removed 2 rows of data, which may not seem like a big deal but you do not want your visualizations to be misleading. Especially when you start visualizing summary statistics (like means). If you look at your actual data though, you can verify that there are two penguins that do not have a measurement value for flipper length or bill length. So obviously without either or both of those, it cannot be included on the graph. In this case, this is okay, but it is important to always verify!

::: {.rmdtip}
<p style="font-size:10pt"><u>**NOTE:**</u> *moving forward, for pedagogical purposes this warning message will be hidden so the output from subsequent code is cleaner. However, those 2 rows are still being removed!*</p>
:::

Above, it was noted that the goal of a visualization is to understand a relationship, or pattern, in your data. For every visualization you make, a verbal description of the pattern seen (should one exist!) will be provided underneath. This will help develop an intuitive **graph literacy**. Being able to quickly and accurately interpret visualizations is an important skill, and one of the pillars of this course. 

::: {.rmdimportant}
<p style="font-size:10pt">*Things related to graph literacy and comprehension will appear in boxes like this moving forward!*</p>

In the visualization it can be seen that, generally speaking, penguins with greater flipper length tend to have greater bill length as well. There seems to be a positive linear relationship between bill length and flipper length such that, as flipper length increases, bill length increases in turn.
:::

<p class="text-info"> **<u>Note:</u> You have to use a lot of tentative language and include many qualifiers here. No formal statistical analyses have been done to afford the ability to state any relationship or effect definitively.**</p>

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 2**</div>
  <div class="panel-body">Update your graph above by telling ggplot you want it to render a point for each observation in the penguins dataset. Describe the pattern/relationship you see.</div>
</div>

# Aesthetics

## Color

In looking at the graph created above, it kind of seems like there may be different groups or clusters of penguins in this data. When you notice a pattern like this, it is worth looking into further and considering what variables you have in our data that could be related.

One way the penguins seem to be grouped is with a cluster at the top and another at the bottom:

<img src="Visualizations1_files/figure-html/unnamed-chunk-8-1.png" width="672" />

This might correspond to the `sex` variable in the dataset! Maybe it is the case that the bigger penguins were males, and the smaller ones were females?

<img src="Visualizations1_files/figure-html/unnamed-chunk-9-1.png" width="672" />

This idea could be explored if there was some way to visually indicate which of the observations on the graph were from male penguins and which were from female. Specifically, you want the *color* of the points to different based on the value for that observation's `sex`. In fact... you can do just that! One of the best parts about coding is, you can always just try to run some code and see what happens!

::: {.rmdwarning}
<p style="font-size:10pt">It is often helpful to declare formal predictions. To do so, you specify two mutually exclusive alternatives that you *could* see when exploring some idea.</p>

* "If it is the case that the differences in penguin size could be explained by what sex they are (e.g., bigger penguins were males and the smaller ones were females), then all the points in the cluster at the top would be one color and all the points in the cluster at the bottom would be a different color. If it is not the case that the differences in penguin size could be explained by what sex they are, the points in each of the two clusters would not have distinct colors."
:::


```r
penguins %>%
  ggplot(mapping = aes(y = flipper_length_mm,
                       x = bill_length_mm),
         color = sex) +
  geom_point()
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-10-1.png" width="672" />

Uh... okay. So, that did not work. OH! Remember from above that `aes()` is used to tell ggplot what variables from the data should be *mapped* to the aesthetics you want to render on your graph? Color needs to be passed as another argument to the aesthetics of the plot via `aes()`. 


```r
penguins %>%
  ggplot(mapping = aes(y = flipper_length_mm,
                       x = bill_length_mm,
                       color = sex)) +
  geom_point()
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-11-1.png" width="672" />

> "Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, **and map sex to the color of each point.**"

That is more like it! This does not really look like what you may have thought it might though. It is consistent with the other possible outcome, where sex does not explain the differences in penguin size. However, after looking at this more carefully, it actually kind of looks like there are 3 distinct clusters, not 2.

<img src="Visualizations1_files/figure-html/unnamed-chunk-12-1.png" width="672" />

This might correspond to the `species` variable in the dataset! Maybe it is the case that different penguin species differ in size? That actually makes a lot more sense. This idea can be tested as well!

::: {.rmdwarning}
* "If it is the case that the differences in penguin size could be explained by what species they are, then all the points in each cluster would have distinct colors. If it is not the case that the differences in penguin size could be explained by what species they are, the points in each of the clusters would not have distinct colors."
:::


```r
penguins %>%
  ggplot(mapping = aes(y = flipper_length_mm,
                       x = bill_length_mm,
                       color = species)) +
  geom_point()
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-13-1.png" width="672" />

> "Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, **and map *species* to the color of each point.**"

::: {.rmdimportant}
In the visualization it can be seen that, generally speaking, penguins with greater flipper length tend to have greater bill length as well. There seems to be a positive linear relationship between bill length and flipper length such that, as flipper length increases, bill length increases in turn. **Additionally, penguins of the same species tend to have similar flipper and bill lengths, which are distinct from other species. Adelie penguins tend to have the shortest lengths, and Gentoo the longest. Chinstrap penguins seem to have shorter flippers but longer bills.**
:::

Hot dang, looks like this could be a promising explanation for how penguins differ in size! There would obviously need to be some kind of formal statistical analysis to know for sure, but visualizing your raw data in ways like this allow you to quickly get insights into different questions you may want to use your data to answer.

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 1**</div>
  <div class="panel-body">Think about what patterns you notice in the visualization you created, and test whether `sex` or `species` might explain those patterns. Make a prediction, test that prediction, and then describe what you see.</div>
</div>


### Global vs Local Aesthetics

When first describing This visualization, one of the things noted was that, *"There seems to be a positive linear relationship between bill length and flipper length such that, as flipper length increases, bill length increases in turn."* It can be helpful to add the line of best fit to actually see this linear relationship. You can do this by using `geom_smooth()`. A few arguments need to be set within `geom_smooth()`, but you do not have to worry much about those.

The code from above will be copy/pasted and the new `geom_smooth()` call can be added directly to it. By doing so the best fitting line for all of the data will be displayed.


```r
penguins %>%
  ggplot(mapping = aes(y = flipper_length_mm,
                       x = bill_length_mm,
                       color = species)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-14-1.png" width="672" />

>"Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, and map *species* to the color of each point. **Add a line of best fit for the data.**"

Hey, wait, that is not quite what was expected... Instead of there being one line for all of the data, it looks like there is a line for each species. What is happening here is highlighting the difference between a **<u>global</u>** aesthetic (those put in the `ggplot()` call and apply to ALL added parts of your graph) and a **<u>local</u>** aesthetic (those put in individual geoms and apply only to those individual ones).

Color should be moved into the `geom_point()` call, because that is the only thing that should be mapped to the values of `species` in the data.


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species)) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-15-1.png" width="672" />

Alright! The points are still colored by species, but there is just the one line for all the data, as was originally intended. You will also notice that a `mapping=` was not specified in `geom_point()`, and it was removed from the `ggplot()` as well! It actually is not necessary to specify that. Once you have the hang of things, you can leave that part out.

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 2**</div>
  <div class="panel-body">Add in the line of best fit to your graph with `geom_smooth()`, making sure that your code produces a single line that is fit to ***all*** your data.</div>
</div>


### Setting vs Mapping Aesthetics

Above, to change the color of your points (and inadvertently our line of best fit), the `color` argument was specified in the `aes()` call to map the values to the `species` variable in the data. What if you did not want to map something to a variable in your data? What if you just want to set the value of something yourself? You might think, "maybe I just take it out of the `mapping = aes()` part?" And you would be right, because you are smart! Look at the example below that does just that to try and change all the points to orange (so they have a good contrast with the line).


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(color = orange) +
  geom_smooth(method = "lm", se = FALSE)
#> Error in layer(data = data, mapping = mapping, stat = stat, geom = GeomPoint, : object 'orange' not found
```

Well, okay, this obviously is not right because it results in an error:

<p style="color:#A79BF0"> **Error in layer(data = data, mapping = mapping, stat = stat, geom = GeomPoint, : object 'orange' not found**</p>

By writing `orange` like this, R thinks it is a variable in the `penguins` dataset that was piped in to ggplot! To tell R that this **is NOT** a variable/object that is defined, and instead just the color orange, you use quotes.


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(color = "orange") +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-17-1.png" width="672" />

Much better! You can do this for any geom:


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(color = "orange") +
  geom_smooth(method = "lm", se = FALSE, color = "purple")
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-18-1.png" width="672" />

<p class="text-info"> **<u>Note:</u> Aesthetic settings can only be set locally, not globally. Aesthetic mappings can be local OR global.**</p>


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm),
         color = "orange") +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-19-1.png" width="672" />

### Setting Colors

When setting a color, this can be done by name, as was done above, or by hexcode:


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(color = "#9FE2BF") +
  geom_smooth(method = "lm", se = FALSE, color = "#FFBF00")
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-20-1.png" width="672" />

A neat thing you can do is to set your colors based on some logical operation! For example, if you wanted to color penguins with flipper lengths *greater* than 200 as one color, and those with flipper lengths *not great* than 200 another color, you could do the following:


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = flipper_length_mm > 200)) +
  geom_smooth(method = "lm", se = FALSE, color = "#FFBF00")
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-21-1.png" width="672" />

Notice that the color was just set to be equal to a simple logical test! This is still a mapped aesthetic, because it will be based on values of a variable in your data.

<!-- You can also set colors by using palettes from different packages. We will talk more about this later, but one package we will use often is `viridis`. -->

<!-- ```{r warning=FALSE, message=FALSE} -->
<!-- penguins %>% -->
<!--   ggplot(aes(y = flipper_length_mm, -->
<!--              x = bill_length_mm)) + -->
<!--   geom_point(aes(color = species)) + -->
<!--   geom_smooth(method = "lm", se = FALSE)+ -->
<!--   scale_color_viridis_d() -->
<!-- ``` -->

<!-- >"Start by telling ggplot to use the `penguins` dataframe for our data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, and map species to the color of each point. Add a line of best fit for our data, **and use the viridis color palette for our discrete mapped variable.**" -->

There are tons of good color guides you can find on google. Here are couple:<br> 
* [one](figures/ggplot_colors.png)
* [two](https://github.com/EarlGlynn/colorchart/raw/master/ColorChart.pdf)
* [three](figures/ggplot_colors2.png)<br>

More on colors will be covered in a later lesson.

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 3**</div>
  <div class="panel-body">Use R code to recreate the graphs below:
<br>
A. Make a visualization to investigate the relationship between bill length (on the y-axis) and bill depth (on the x-axis). 




B.

<img src="Visualizations1_files/figure-html/unnamed-chunk-23-1.png" width="672" />


C. Make a visualization to investigate the relationship between between body mass and flipper length. In doing so, make the geometric figure rendering the observations appear in blue.



D.

<img src="Visualizations1_files/figure-html/unnamed-chunk-25-1.png" width="672" />

E.

<img src="Visualizations1_files/figure-html/unnamed-chunk-26-1.png" width="672" />
</div></div>

## Shape

### Mapping

Color is one of the aesthetics you can change, but there are many others too! Another one is shape. Like color, shape can be mapped to a variable in your data. In addition to changing the color of the points by `species`, the code below will change their shape too!


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species,
                 shape = species)) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-27-1.png" width="672" />

>"Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, and map species to the color of each point. Add a line of best fit for the data **and change the shape of each point to be mapped to species.**"

However, you do not have to map the same aesthetic. The shape could be mapped to a different aesthetic too! Consider what happens when mapping it to `island` instead.


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species,
                 shape = island)) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-28-1.png" width="672" />

You will now notice instead of each coloring having one shape, as when you mapped them to the same variable, now the colors can have multiple shapes! Mapping another variable to a different aesthetic visualizes even more data and patterns in your dataset! It can quickly become overwhelming and difficult to interpret though, so it is often best to try to just communicate one main relationship or pattern in your visualizations. For practice though, you can go nuts!

### Setting

There are a number of different shapes you can choose from when setting yourself. These can be selected by number:

![](figures/ggplot_shapes_num.png){width=100%}

or by name:

![](figures/ggplot_shapes_name.png){width=100%}
<p style="font-size:8pt">Source: [ggplot documentation](https://ggplot2.tidyverse.org/articles/ggplot2-specs.html#point)</p>

You will notice that for several shapes, there appear to be different versions. For example, there appears to be 3 different kinds of triangles. Their differences are compared below:

* 17 or 'triangle'
    + Solid color shape
    

```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species),
             shape = 17) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-29-1.png" width="672" />


* 2 or 'triangle open'
    + Colored shape outline with hollow center
    

```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species),
             shape = 2) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-30-1.png" width="672" />


* 24 or 'triangle filled'
    + Colored shape outline with center that can be filled with another color
    

```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species),
             shape = 24,
             fill = 'darkgrey') +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-31-1.png" width="672" />

This introduces another way to change colors in ggplot! When changing the color of something, some geoms have a 'color' argument, some have a 'fill' argument, and some, like here, have both! 'color' often refers to the outline/outside, while 'fill' often refers to... well, the fill!

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 4**</div>
  <div class="panel-body"><u>Part 1:</u><br>Take the code you produced for Exercise 2 and 1. Change the shape to be solid color squares mapped to the `species` variable. 2. Change the shape to be hollow squares mapped to the `species` variable. 3. Change the shape to be filled squares mapped to the `species` variable, and fill with a color of your choosing.
  <br>
  <br>
  <u>Part 2:</u><br>
  Use R code to recreate the graphs below:
  <br>
1. 

<img src="Visualizations1_files/figure-html/unnamed-chunk-32-1.png" width="672" />

2. 

<img src="Visualizations1_files/figure-html/unnamed-chunk-33-1.png" width="672" />

3. 

<img src="Visualizations1_files/figure-html/unnamed-chunk-34-1.png" width="672" />
</div></div>

## Size

### Mapping

So, it was actually kind of hard to see some of those shape differences, wasn't it? The points on the graph were just too small. It would be helpful if the *size* could be changed. Wouldn't you know it, `size` happens to be another aesthetic! Below is an example of mapping *size*:


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species,
                 shape = species,
                 size = species)) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-35-1.png" width="672" />

>"Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, and map species to the color of each point. Add a line of best fit for the data, change the shape of each point to be mapped to species **and do the same with corresponding size changes.**"

Well, this looks pretty shitty, but it gets the point across! One other thing this does is demonstrate what is meant by saying, **"ggplot is additive/layered"**.`geom_smooth()` was the last geom added here. That means, quite literally, it is added *on top* of the graph rendered by the previous code. It gets put on top, which is why it runs over and covers up some of the observations. 

Think about what the graph would look like if you were to have added the `geom_smooth()` first and `geom_point()` second, then click the button below to find out:

<button class="btn btn-primary" data-toggle="collapse" data-target="#BlockName"> Show/Hide </button>  
<div id="BlockName" class="collapse">  

Since the points were added last, they were added on top of the line. Parts of the line are thus covered by the points on top of it!
<img src="Visualizations1_files/figure-html/unnamed-chunk-36-1.png" width="672" />

</div>
<br>
This concept is very important to keep in mind when creating your visualizations. **Order matters!**

### Setting

Maybe instead of mapping this... you should just change the size yourself. For example, you could take one of the graphs from above and make the shapes a little bit larger.


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species),
             shape = 24,
             fill = 'darkgrey',
             size = 4) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-37-1.png" width="672" />

Wow, okay, this shows the fill a lot better than before! You can actually do some pretty neat things by playing around with different size values. For example:



```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species)) +
  geom_point(aes(color = species),
             size = 4,
             shape = 6) +  
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-38-1.png" width="672" />

<div class="panel panel-warning">
  <div class="panel-heading">**BONUS CHALLENGE:**</div>
  <div class="panel-body">See if you can think of a way to use what we demonstrated above to take your code from the Exercise 4, part 2, question 3, and give each observation a thicker outside border. The result should look like the graph below:</div>
  <br>
<img src="Visualizations1_files/figure-html/unnamed-chunk-39-1.png" width="672" />
</div>
<br>

## Alpha

### Mapping

The alpha aesthetic changes how translucent vs opaque something is.


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species,
                 shape = species,
                 size = species,
                 alpha = species)) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-40-1.png" width="672" />

>"Start by telling ggplot to use the `penguins` dataframe for the data, map flipper length to the y-axis and bill length to the x-axis. Represent each observation with a point, and map species to the color of each point. Add a line of best fit for the data, change the shape of each point to be mapped to species, and do the same with corresponding size changes **and alpha changes.**"

As you can see, using alpha as a mapped aesthetic is not particularly useful. There are few, if any, instances where you would want to do this.

### Setting

More often, you will want to set the alpha levels of different elements of your graphs yourself. Alpha values range from 0-1, with 0 being completely transparent and 1 being completely opaque.

Compare the two graphs below and note their alpha levels:


```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species,
                 shape = species),
             size = 3,
             alpha = 0.8) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-41-1.png" width="672" />



```r
penguins %>%
  ggplot(aes(y = flipper_length_mm,
             x = bill_length_mm)) +
  geom_point(aes(color = species,
                 shape = species),
             size = 3,
             alpha = 0.3) +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="Visualizations1_files/figure-html/unnamed-chunk-42-1.png" width="672" />

While you are working with a (relatively) small dataset here, you may already see how alpha changes could be extremely useful when working with larger datasets:

<img src="Visualizations1_files/figure-html/unnamed-chunk-43-1.png" width="672" />

Looking at the observations highlighted with red circles, you can notice that they are darker in color than other observations from the same species. This means there are multiple observations with the same values, being rendered in the same spot on your graph. When the observations are completely opaque (no alpha), some may be obscured by others occupying the same space! 

This is even more clear when looking at an example from the built in `diamonds` dataset:

<img src="Visualizations1_files/figure-html/unnamed-chunk-44-1.png" width="672" />

Even with a very low alpha (look how faint the points outside the center cluster are), you can see just how many observations are overlapping! You will see better ways to visualize data like this later.

<div class="panel panel-success">
  <div class="panel-heading">**EXERCISE 5**</div>
  <div class="panel-body">Update the main graph you have been working with throughout the exercises to make the observations more translucent. First try doing so by mapping it to `island`. Then, create another graph where you set the alpha yourself. Try to pick the value you think makes the graph most legible and effective.</div>
</div>

## References:

Horst AM, Hill AP, Gorman KB (2020). palmerpenguins: Palmer Archipelago (Antarctica) penguin data. R package version 0.1.0. https://allisonhorst.github.io/palmerpenguins/
