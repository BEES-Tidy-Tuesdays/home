# Week 0 notes

This was our test file for week 0.

## Access the data

The main data: https://github.com/rfordatascience/tidytuesday/tree/master/data/2020/2020-02-18


## Relevant questions we can answer with the data

Let's gather some questions;how you're gonna visualise it; put your name if you are working on the question

1. What type of food contributes the highes GHG emission?
     - cumulative plots (Dony) example
        -  example: https://stackoverflow.com/questions/15844919/cumulative-plot-using-ggplot2


## Some plots we've done


1. What type of food contributes the highes GHG emission?
    - geom_jitter (Alex)
``` {r}
clean_table %>% 
  ggplot(aes(x = fct_reorder(food_category, consumption), 
             y = consumption, 
             color = country)) +
  geom_jitter() +
  theme(legend.position = "none") +
  coord_flip()
```
![](https://i.imgur.com/TkLVrnZ.png)



## Help! - ask question here

Q. How do I change the background?
A. Use theme
Q

## Open R questions

## Cool R things I learnt this week

## Useful resources

## Feedback

What we can improve for the next tidy-tuesdays?

Took too long

Too fast

Too slow



