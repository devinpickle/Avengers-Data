---
title: "Avengers"
author: "Devin Picklesimer"
date: "November 30, 2021"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    code_folding: hide
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---






```r
# Use this R-Chunk to import all your datasets!
avengers <- read.csv("avengers_csv.csv")
```

## A Little Background

I really like Marvel superheroes and I wanted to practice using R for visualizations. When looking for interesting data sets and questions to answer, I found this Avengers data set.



```r
# Use this R-Chunk to clean & wrangle your data!
top25 <- avengers %>% 
  arrange(desc(appearances)) %>% 
  head(25)

avengers_dead <- avengers %>% 
  group_by(death1) %>% 
  count()

# Avengers to have died 5 times
# avengers %>% 
#   group_by(death5) %>% 
#   count()

top10 <- avengers %>% 
  arrange(desc(appearances)) %>% 
  head(10)

# Number of male and femal avengers initiated each year
additions <- avengers %>% 
  filter(year != 1900) %>% 
  group_by(gender, year) %>%
  count()

death1_revival <- avengers %>% 
  select(-c(notes, url)) %>% 
  filter(death1 == "yes") %>% 
  group_by(return1) %>% 
  count()
```

## Visuals


```r
# Use this R-Chunk to plot & visualize your data!
ggplot(top25) +
  geom_point(aes(x = year, y = appearances, color = death1)) +
  geom_text_repel(aes(x = year, y = appearances, label = name.alias)) +
  labs(
    title = "25 Avengers with the Most Appearances",
    x = "Year Initiated",
    y = "# of Appearances",
    color = "Has Died Before"
  )
```

![](avengers_project_files/figure-html/plot_data-1.png)<!-- -->

```r
ggplot(additions) +
  geom_point(aes(x = year, y = n, group = gender, color = gender)) +
  geom_line(aes(x = year, y = n, group = gender, color = gender)) +
  labs(
    title = "Male and Female Avengers Initiated By Year",
    x = "Year Initiated",
    y = "# of Avengers"
  )
```

![](avengers_project_files/figure-html/plot_data-2.png)<!-- -->

```r
ggplot(avengers_dead) +
  geom_col(aes(x = death1, y = n, fill = death1)) +
  labs(
    title = "Avengers That Have Died",
    x = "Have Died",
    y = "# of Avengers"
  )
```

![](avengers_project_files/figure-html/plot_data-3.png)<!-- -->

```r
ggplot(death1_revival) +
  geom_col(aes(x = return1, y = n, fill = return1)) +
  labs(
    title = "Avengers Brought Back to Life After Dying Once",
    x = "Revived?",
    y = "# of Avengers"
  )
```

![](avengers_project_files/figure-html/plot_data-4.png)<!-- -->

