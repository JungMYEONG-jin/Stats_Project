---
title: "practice"
author: "JMJ"
date: '2021 3 17 '
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Pipe

hello

```{r cars}
summary(cars)
```

## Including Plots

You can also embed plots, for example:

```{r}
plot(cars)
```



## nycflights13

연습



```{r}
library(nycflights13)

```
```{r}
flights[1:5,]
```

## Filter rows with filter

```{r warning=FALSE}
library(tidyverse)


flights %>% filter(month==10 & day==10) %>% head(3)



flights %>% filter(month==1, day==1) %>% filter(dest=="ORD") %>%head()
```

```{r warning=FALSE}

flights %>% filter(month==1 & day==1 & dest=="ORD") %>% head()
```


```{r}
# %in% meaning or
flights %>% filter(month==1 & day==1 & dest %in% c("ORD", "LAS")) %>% head()


```


## Arrange rows with arrange()

```{r}

arrange(flights,year, desc(month), day) %>% head()


arrange(flights, desc(dep_delay)) %>% head()



```


## Select columns with select()

```{r}
select(flights, year, month, day) %>%head()



select(flights, year:day) %>% head()


select(flights, -(year:day)) %>% head()
```



## Helper functions

- ### starts_with("abc"): abc로 시작하는 모든걸 매칭해줌
- ### ends_with("xyz"): xyz로 끝나는 모든걸 매칭해줌
- ### contains("ijk"): ijk를 포함하는 모든걸 매칭해줌
- ### num_range("x", 1:3): x1, x2, x3를 매치해줌

```{r}

select(flights, starts_with("arr")) %>% head()


select(flights, contains("arr")) %>% head()


select(flights, ends_with("time")) %>% head()


select(flights, time_hour, air_time, everything()) %>% head()

```

### Rename the variable with rename()

```{r}

rename(flights, tail_num=tailnum) %>% head()

```


## Add new variables with mutate()

```{r}

flights %>% select(year:day, ends_with("dealy"), distance, air_time) %>% head()


flights %>% select(year:day, ends_with("delay"), distance, air_time) %>%
  mutate(gain=dep_delay-arr_delay,
         hours = air_time/60,
         gain_per_hour = gain/hours) %>% head()

```

## summarise를 이용해 그룹 결과 return

```{r}

flights %>% group_by(year, month, day) %>% summarise(delay = mean(dep_delay, na.rm=TRUE)) %>% head()





flights %>% group_by(origin) %>% summarise(delay = mean(dep_delay, na.rm=TRUE), sd_delay = sd(dep_delay, na.rm=TRUE)) %>% head()



flights %>% group_by(dest) %>%
  summarise(count=n(), dist=mean(distance, na.rm=TRUE), delay=mean(arr_delay, na.rm=TRUE)) %>% filter(count>20 & dest!="HNL") %>% head()

```


### 거리가 작을수록 지연이 늘어난다 왜그러지? 그래프로 표현해보자

```{r}

flights %>% group_by(dest) %>% summarise(count = n(), dist = mean(distance, na.rm=TRUE), delay = mean(arr_delay, na.rm=TRUE)) %>% filter(count>20 & dest!="HNL") %>% ggplot(mapping=aes(x=dist, y=delay)) + geom_point(aes(size=count), alpha=1/3) + geom_smooth(se=FALSE)



```

## ggplot is best!

```{r}

ggplot(data=mpg) + geom_point(mapping=aes(displ, hwy))

```


declare global mapping inform in ggplot()

```{r}

ggplot(data=mpg, mapping=aes(displ ,hwy)) + geom_point()

```

### layer추가

```{r}

ggplot(data=mpg, mapping=aes(displ, hwy)) + geom_point() + geom_line()



ggplot(data = mpg, mapping=aes(displ, hwy)) + geom_point() + geom_line() + theme_bw()


ggplot(data=mpg, mapping=aes(x=displ, y=hwy, color=class)) + geom_point()


ggplot(data=mpg, mapping=aes(x=displ, y=hwy, size=class)) + geom_point()


ggplot(data=mpg, mapping=aes(displ, hwy, alpha=class))+geom_point()



ggplot(data=mpg, mapping=aes(displ , hwy, shape=class)) + geom_point()

```

## Facet

```{r}

ggplot(data=mpg) + geom_point(mapping=aes(x=displ, y=hwy))+facet_wrap(~class, nrow=2)



ggplot(data=mpg) + geom_point(mapping = aes(x=displ, y=hwy))+facet_grid(drv~cyl)




```

## Theme

```{r}

ggplot(data=mpg) + geom_point(mapping=aes(x=displ, y=hwy)) +theme_bw()+theme(axis.line = element_line(size=.8, colour = "black"),panel.grid.minor = element_blank(), panel.border = element_blank(),text = element_text(size=25))










ggplot(data=mpg) + theme_bw() + geom_point(mapping=aes(x=displ, y=hwy))+facet_grid(drv ~ cyl) + theme(axis.line=element_line(size=.8, colour="black"), panel.grid.minor = element_blank(), text=element_text(size=25))




```


