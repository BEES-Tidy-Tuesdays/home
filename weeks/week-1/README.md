# Tidy Tuesdays Week 1: U.S. Measles Vaccination Data

To start please visit: https://github.com/BEES-Tidy-Tuesdays/home

You will find a link to this collaborative document called "Week 1 notes".

This is a collaborative markdown document: feel free to add, change, and improve it. We will upload the final document to github after this Tidy Tuesday session and use parts of it as a template for future sessions.

If something is unclear or doesn't make sense, fix it, or make a comment.


### A. Preparation (~5-10 minutes)

#### A1. Make sure R and R studio are installed and running

#### A2. Access and explore the data
Please access the data [here](https://github.com/WSJ/measles-data). Download and extract to a specified folder. We encourage you to start working by creating a new Rproject, and use best practices for file management.

To clone the data set from github using git in RStudio:
1. Select "New Project"
2. Select "Version control"
3. Select "Git"
4. Paste "https://github.com/WSJ/measles-data" as the URL, and select wher you want to clone the files to on your computer

N.B. You need git installed on your computer, you can download it here:
- https://gitforwindows.org/ (Windows)
- https://git-scm.com/download/mac (Mac)


### B. List questions we can ask from the data (10 minutes)

Talk to the people nearest you and brainstorm some questions we can ask with this dataset. What could this data tell us? What are some interesting questions we could ask? How do you plan to visualise it?

#### Q1.
Options:

#### Pre-processing

1. Is the data in a 'tidy' format?

- All of the data (individual states, state overviews, and all measles rate) are **tidy**.
- However, the `state-overviews.csv`, and  the `all-measles-rate.csv` don't have the geographical location information.

3. What are the steps to make this data ready to use?
- What does -1 mean?
    - Sometimes datasets use 
- Do we need to filter out schools with missing data?

What do the variables mean?


### C. Some plots we've done (30 minutes)

#### Preprocessing

#### Read all data from the individual-states
```
library(tidyverse)

ind_stats <-
    list.files(path = "individual-states/", # locate the folder
    pattern = "*.csv", # identify the type of files we want to read
    full.names = T) %>% # tell R to give us the whole directory
  map_df(~read_csv(., col_types = cols(.default = "c")))
  
View(ind_stats)
```

#### Can we map the where the missing data are?

```
ind_stats %>% 
  sample_frac(0.2) %>% # random sample just 20% of the data
naniar::vis_miss() # map the missing data
```


#### Q1 Does x correlate with y?
##### Answer

```
library (ggplot2)

ggplot(data, aes(x = x, y = y)) +
geom_point()+ #plot each of the data points
goem_smooth(method = "lm") #plot th trend line using a linear model
```

#### Q2 Do vaccination rates go up over time? (Alex)
##### Answer 

```
ggplot(data_rates, aes(year, mmr))+
  geom_point()
```
Hmm, doesn't seem to show much. Only 3 time points.

#### Q3 Let's see the vaccination rates in the different states (Gian)
##### Answer 

```
ggplot(state_overviews, aes(fct_reorder(state, mmr), mmr)) +
  geom_boxplot() +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) # rotate x axis label
```

#### Q4 Does the type of school have an effect on vaccination rates

```
ggplot(ind_stats, aes(x = type, y = log10(100-mmr))) +
geom_boxplot()
```


#### note that I had a problem with question 4 plotting because mmr was a character variable - need to change this when reading code (see last line):

```
ind_stats <-
    list.files(path = "individual-states/",
    pattern = "*.csv", full.names = T) %>%
  map_df(~read_csv(., col_types = cols(.default = "c"))) %>%
  mutate(mmr=as.integer(mmr))
```

### D. Wrap up (10 minutes)

#### Cool R things I learnt this week

#### What could be improved from the next meeting?

 - get some fooood



---

### E Help! - ask questions about today's Tidy Tuesday here

#### Q1. How do I change the background?
A. Use theme


### F General R help - Stuck on something? Need advice on you current project? Can you help answer someone else's question?

(not this document is public, so don't include sensitive or private information)

#### Q. How do I use gganimate to transition between two 'sets' of columns? - Alex

Example columns I have: Species, Current.Temp, Future.Temp, Current.Rain, Future.Rain, Current.Risk, Future.Risk

Want something like:

```
ggplot(data, (x = Current.Temp, y = Current.Temp, colour = Current.Risk)) +
geom_point()
```
transitioning to:

```
ggplot(data, (x = Future.Temp, y = Future.Temp, colour = Future.Risk)) +
geom_point()
```

A.
