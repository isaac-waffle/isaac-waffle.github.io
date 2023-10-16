---
date: 2023-11-22 12:26:40
layout: post
title: Human Mortality!
subtitle: Let's start!
description: UMMMMMM
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559821647/theme6_qeeojf.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559821647/theme6_qeeojf.jpg
category: blog
tags:
  - crazy
  - story
author: mranderson
---

\##Before we start

We used Switzerland data.

We need to download some packages for graph.

And then apply it.

    lapply(c('tidyverse','ggplot2','gganimate','gifski','directlabels','png','transformr'), require, character.only = TRUE) 

Also, we need to load our ‘Switzerland data’

change eunhokim into your Admin name and put our file into desktop.

This path is for Mac, so if you use Windows,

use file.path(path.expand(‘~’),‘Desktop’) and change path.

    life_table<- read.table('/Users/eunhokim/Desktop/swiss_mltper.txt',
                   header = FALSE,col.names = c('Year','Age','mx' ,'qx','ax','lx','dx','Lx','Tx','ex')
                   ,stringsAsFactors=FALSE)

Now, this data is written by character so we convert this to numerical
data and omit NA values.

    life_table<-life_table[-1,]
    life_table<- as.data.frame(apply(life_table, 2, as.numeric))

    ## Warning in apply(life_table, 2, as.numeric): NAs introduced by coercion

    life_table<-na.omit(life_table)

## Question 2

\#a: *μ**x**t* the force of mortality of an (x) year old in year t,
versus x, for different selected time periods t

Below graph, we can find mortality rate of age in 2021

    with(subset(life_table,life_table$Year == 2021),
         plot(Age,log(mx),
              main='force of mortality of an (x) year old in 2021',
              xlab='age x',
              ylab=expression(paste('log mortality rate')),
              type='l'))

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-4-1.png)

If we want to compare mortality rate with 10 year period:

    k<-seq(1876,2021,10)
    b<-subset(life_table,life_table$Year==k)

    ## Warning in life_table$Year == k: longer object length is not a multiple of
    ## shorter object length

    ggplot(data = b)+
      geom_line(mapping = aes(x = Age, y = log(mx)))+
      facet_wrap(~Year,nrow=4)

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-5-1.png)

If we want to compare from 1886 until 2021:

    life_table_a = life_table[life_table$mx > 0,]
    Moving_Graph <- ggplot(
      life_table_a, 
      aes(x = Age, y=log(mx))) +
      geom_line(show.legend = FALSE, alpha = 10) +
      labs(x = "Age", y = "log mortality rate")+
      scale_y_continuous(limits = c(-15,0), breaks = seq(-15,0,5))
    Moving_Graph + transition_time(Year) +
      labs(title = "Year: {frame_time}")

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-6-1.gif)

\#b: *μ**x*,*t* versus t, for different selected ages x

Below graph, we can find mortality rate of Year in age 20

    with(subset(life_table,life_table$Age==20),
         plot(Year,log(mx),
              main='force of mortality of an 20 year old at Year t',
              xlab='Year',
              ylab=expression(paste('log mortality rate')),
              type='l'))

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-7-1.png)

Below graph, we can find mortality rate of Year in age 50

    with(subset(life_table,life_table$Age==50),
         plot(Year,log(mx),
              main='force of mortality of an 50 year old at Year t',
              xlab='Year',
              ylab=expression(paste('log mortality rate')),
              type='l'))

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-8-1.png)

If we want to compare mortality rate with 10 year period from 0 to 80
year:

    l<-c(0,10,20,30,40,50,60,70,80)
    c<-subset(life_table,life_table$Age==l)

    ## Warning in life_table$Age == l: longer object length is not a multiple of
    ## shorter object length

    ggplot(data = c)+
      geom_line(mapping = aes(x = Year, y = log(mx)))+
      facet_wrap(~Age,nrow=3)

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-9-1.png)

If we want to compare from 1886 until 2020

    life_table_b = life_table[life_table$mx > 0,]
    Moving_Graph <- ggplot(
      life_table_b, 
      aes(x = Year, y=log(mx))) +
      geom_line(show.legend = FALSE, alpha = 10) +
      labs(x = "Year", y = "log mortality rate")+
      scale_y_continuous(limits = c(-15,0), breaks = seq(-15,0,2.5))
    Moving_Graph + transition_time(Age) +
      labs(title = "Age: {frame_time}")

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-10-1.gif)

\#c: The survival function S0,t(x) of a newborn, when using data from
different selected time periods t

Survival function in 2010

    with(subset(life_table,life_table$Year == 2010),
         plot(Age,lx/lx[1],
              main='survival function S_0_t(x) of a newborn in 2010',
              xlab='Age',
              ylab=expression(paste('S_0_t(x)')),
              type='l'))

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-11-1.png)

If we want to compare survival function with 10 year period:

    k<-seq(1876,2021,10)
    b<-subset(life_table,life_table$Year==k)

    ## Warning in life_table$Year == k: longer object length is not a multiple of
    ## shorter object length

    ggplot(data = b)+
      geom_line(mapping = aes(x = Age, y =lx/lx[1]))+
      facet_wrap(~Year,nrow=4)

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-12-1.png)

If we want to compare from 1886 until 2020

    life_table_a = life_table[life_table$mx > 0,]
    Moving_Graph <- ggplot(
      life_table_a, 
      aes(x = Age, y=lx/lx[1])) +
      geom_line(show.legend = FALSE, alpha = 10) +
      labs(x = "Age", y = "Survival function S_0")+
      scale_y_continuous(limits = c(0,1), breaks = seq(0,1,0.25))
    Moving_Graph + transition_time(Year) +
      labs(title = "Year: {frame_time}")

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-13-1.gif)

\#d: my own graph

I want to compute life expectancy versus age with diffrent Year

    life_table_a = life_table[life_table$mx > 0,]
    Moving_Graph <- ggplot(
      life_table_a, 
      aes(x = Age, y=ex)) +
      geom_line(show.legend = FALSE, alpha = 10) +
      labs(x = "Age", y = "life expectancy")+
      scale_y_continuous(limits = c(0,100), breaks = seq(0,100,20))
    Moving_Graph + transition_time(Year) +
      labs(title = "Year: {frame_time}")

![](group47_LM_assignment2_files/figure-markdown_strict/unnamed-chunk-14-1.gif)

Above graph, we can find that the life expectancy has positive
relationship with Year.

Also, if age increases, expectancy decreases.
