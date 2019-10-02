---
title: "Fall 2018 R Workshops - Session 2: GGPLOT"
output: 
  slidy_presentation:
    keep_md: true

---



## Overview

Welcome back everyone! This is our second R Workshop of the Fall 2018 semester. Our goals for today will be to:

- make sure everyone understands the R Markdown environment
- to learn and play with the ``ggplot2()`` package for making professional graphs



## R Markdown

- a relatively new framework for editing syntax (code) as an alternative to traditional scripts
- based on ["Markdown"](https://daringfireball.net/projects/markdown/syntax)
- a plain-text coding "language" designed to be easy to read and to quicly convert to html, or PDF or Word with "extenders"
- ["R Markdown"](https://rmarkdown.rstudio.com/lesson-1.html) is just such an extender, build into R Studio, that focuses on integrating R code with Markdown
- it's also just an easier way of adding comments to your code, without using #'s all the time

## Chunky

- You add R code to a .Rmd document by inserting "chunks"
- the convention is:

`` ```{r title, eval=T, echo=T}``

``5+3``

`` ``` ``

- ``r`` means that the coding language being used is ``R``

- ``title`` allows you to name your chunk so that it's indexed in your ToC, it's the only part that's strictly necessary
- ``eval=T/F`` sets whether you want the code to be run and the output displayed
- ``echo=T/F`` sets whether you want the code to be displayed in your output
-you can also set defaults via the "cogs" in R Studio

## Knitting

- you can run chunks of R code line by line, chunk by chunk, or for the entire document at once
- "knitting" is the process by which you run all of the R code and compile all the text in a .Rmd document into a .html or other version for presentation
- these slides are the result of "knitting" this .Rmd document
- you can create templates to customize formatting, insert bibliographic references, etc...


## Why R Markdown

- provides a better way of taking good notes on what code you are running and why (think of it like a lab notebook!)
- provides a way of weaving together data analysis, text, and citations for professional quality manuscripts and presentations

## GGPLOT

- part of a universe of packages by a guy named [Hadley Wickham](http://hadley.nz)
- collectively known as the ["Tidyverse"](https://www.tidyverse.org)
- one of R's supposed relative strengths is its graphics
- ``ggplot2()`` is a major reason why

## DW-NOMINATE Data

Let's load up the DW-NOMINATE data the POLS 641 students analyzed this week:


```r
congress<-read.csv("http://qss.princeton.press/student-files/MEASUREMENT/congress.csv")
rep80 <- subset(congress, subset = (congress == 80 & party == "Republican"))
dem80 <- subset(congress, subset = (congress == 80 & party == "Democrat"))
```

## Progression of Plotting in Base R

```r
plot(dem80$dwnom1, dem80$dwnom2)
```

![](GGPLOT_wkshop_files/figure-slidy/80th Congress 1-1.png)<!-- -->

## We Made it Prettier

```r
Demplot<-plot(dem80$dwnom1, dem80$dwnom2, pch = 16, col = "blue", 
     xlim = c(-1.5,1.5), ylim = c(-1.5,1.5),
     xlab = "Economic liberalism/conservatism",
     ylab = "Racial liberalism/conservatism",
     main = "80th Congress")
```

![](GGPLOT_wkshop_files/figure-slidy/80th Congress 2-1.png)<!-- -->

## We added the Republicans
This slide has only the code (no graph)

```r
#P.S. See what I did here with eval and echo? 
plot(dem80$dwnom1, dem80$dwnom2, pch = 16, col = "blue", 
     xlim = c(-1.5,1.5), ylim = c(-1.5,1.5),
     xlab = "Economic liber alism/conservatism", 
     ylab = "Racial liberalism/conservatism",
     main = "80th Congress")
points(rep80$dwnom1, rep80$dwnom2, pch = 17, col = "red")
text(-0.75, 1, "Democrats")
text(1, -1, "Republicans")
```
And it was annoying because we have to send all commands to console at once

## But it was pretty
This slide has only the graph (no code)
![](GGPLOT_wkshop_files/figure-slidy/Base R Scatterplot Plot-1.png)<!-- -->

## Let's do the same thing in ``ggplot2()``


```r
library(ggplot2)
Dem80Plot<-ggplot(dem80, aes(x = dwnom1, y = dwnom2)) +
  geom_point()
Dem80Plot
```

![](GGPLOT_wkshop_files/figure-slidy/80th Dems in GGPLOT-1.png)<!-- -->

## Let's Make it Pretty

```r
Dem80Plot<- ggplot(dem80, aes(x = dwnom1, y = dwnom2)) +
  geom_point(color="blue", pch=16) +
  scale_y_continuous("racial liberalism/conservatism",
                     limits = c(-1.5, 1.5)) +
  scale_x_continuous("economic liberalism/conservatism",
                     limits = c(-1.5, 1.5)) +
  ggtitle("Polarization in the 80th Congress")+
  theme_bw()
```

## It IS Pretty

```r
Dem80Plot
```

![](GGPLOT_wkshop_files/figure-slidy/Pretty Dems Plot-1.png)<!-- -->

## Add the GOP

```r
Cong80Plot<- Dem80Plot +
  geom_point(data=rep80, aes(x = dwnom1, y = dwnom2), color="red", pch=17)
Cong80Plot
```

![](GGPLOT_wkshop_files/figure-slidy/Adding GOP-1.png)<!-- -->

## Play around with Style

- Check out this [resource](http://www.sthda.com/english/wiki/ggplot2-themes-and-background-colors-the-3-elements)
- And the ``ggplot2()`` ["Cheat Sheet"](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf)
- Play!

## One more cool thing -- Multiple Plots

```r
CongTSPlot<- ggplot(congress, aes(x = dwnom1, y = dwnom2, color=party)) +
  geom_point() +
  facet_wrap(~ congress) +
  scale_y_continuous("racial liberalism/conservatism",
                     limits = c(-1.5, 1.5)) +
  scale_x_continuous("economic liberalism/conservatism",
                     limits = c(-1.5, 1.5)) +
  scale_color_manual(values = c(Democrat = "blue",
                                 Republican = "red",
                                 Other = "green"))+
  ggtitle("Polarization in Congress Over Time")+
  theme_bw()
```

## Ta-Da!

```
## Warning: Removed 2 rows containing missing values (geom_point).
```

![](GGPLOT_wkshop_files/figure-slidy/Facet Wrap Plot-1.png)<!-- -->

*Can you make this a more effective visualization?*
