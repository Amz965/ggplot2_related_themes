---
title: "ggplot2_themes_summary"
author: "Yangjw"
date: "2020年12月12日"
output: 
  html_document:
    keep_md: true
---


## 0. ggplot2 default.

```r
suppressPackageStartupMessages(library(ggplot2))
suppressPackageStartupMessages(library(patchwork))
suppressPackageStartupMessages(library(purrr))
df <- data.frame( x=1:50, y=rnorm(50,mean=5,sd=1), z=sample(x=LETTERS[1:3],50,replace = T) )

scale.colors <- sample(c("brewer","discrete","hue","ordinal","viridis_d"), size = 10, replace = T)
element.names <- c("bw","classic","dark","gray","grey","light","linedraw","minimal","test","void")

map.ggplot2 <- map2( element.names, scale.colors, ~{
  # input.colors <- ifelse( exists(paste0("scale_color_",.)), paste0("scale_color_",.),"scale_color_brewer" )
  ggplot(df,aes(x,y,color=z))+geom_point()+
    eval(parse(text=paste0("scale_color_",.y,"()")))+
    eval(parse(text=paste0("theme_",.x,"()")))+
    labs(title=paste0("Themes: ",.x,"\ncolors: ",.y)) 
})

# get, set, update and replace
## get - return current theme settings
# theme_get()$plot.margin 
## set - set the current theme
p <- map.ggplot2[[10]]
old <- theme_set(theme_bw())
## update
theme_set(old)
theme_update(panel.grid.minor = element_line(colour = "red"))
map.ggplot2[[11]] <- p+old+labs(title = "Usage: update")
## replace
theme_set(old)
theme_replace(panel.grid.minor = element_line(colour = "blue"))
map.ggplot2[[12]] <- p+old+labs(title = "Usage: replace")

map.ggplot2[[13]] <- map.ggplot2[[12]]+theme_grey() %+replace%
                        theme(text = element_text(family = "mono"))+labs(title = "Usage: %+replace%")

wrap_plots( map.ggplot2, ncol = 4 )
```

![](ggthemes_present_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

## 1. ggthemes

```r
suppressPackageStartupMessages(library(ggthemes))
suppressPackageStartupMessages(library(ggplot2))
suppressPackageStartupMessages(library(patchwork))
suppressPackageStartupMessages(library(purrr))

df <- data.frame( x=1:50, y=rnorm(50,mean=5,sd=1), z=sample(x=LETTERS[1:3],50,replace = T) )

element.names <- c("base","calc","clean","economist","excel","excel_new","few","fivethirtyeight","foundation","gdocs","hc","igray","map","pander","par","solarized","solid","stata","tufte","wsj")

map.ggthemes <- map( element.names, ~{
  input.colors <- ifelse( exists(paste0("scale_color_",.)), paste0("scale_color_",.),"scale_color_brewer" )
  ggplot(df,aes(x,y,color=z))+geom_point()+eval(parse(text=paste0(input.colors,"()")))+ eval(parse(text=paste0("theme_",.,"()")))+labs(title=paste0("Themes: ",.,"\ncolors: ",input.colors)) 
})

wrap_plots( map.ggthemes, ncol = 5 )
```

![](ggthemes_present_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

## 2. ggpubr

```r
suppressPackageStartupMessages(library(ggpubr))
suppressPackageStartupMessages(library(ggplot2))
suppressPackageStartupMessages(library(patchwork))
suppressPackageStartupMessages(library(purrr))

df <- data.frame( x=1:50, y=rnorm(50,mean=5,sd=1), z=sample(x=LETTERS[1:3],50,replace = T) )
element.names <- c("pubr","transparent")

map.ggpubr <- map( element.names, ~{
  ggplot(df,aes(x,y,color=z))+geom_point()+ eval(parse(text=paste0("theme_",.,"()")))+labs(title=paste0("Themes: ",.))
})

wrap_plots( map.ggpubr, ncol = 2 )
```

![](ggthemes_present_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

## 3. hrbrthemes

```r
suppressPackageStartupMessages(library(ggsci))
suppressPackageStartupMessages(library(hrbrthemes))
suppressPackageStartupMessages(library(patchwork))
suppressPackageStartupMessages(library(purrr))

df <- data.frame( x=1:50, y=rnorm(50,mean=5,sd=1), z=sample(x=LETTERS[1:3],50,replace = T) )

scale.color <- sample(c("ipsum","ft"), size = 8, replace = T)
element.names <- c("ipsum","ipsum_es","ipsum_rc","ipsum_ps","ipsum_pub","ipsum_tw","modern_rc","ft_rc")

map.hrbrthemes <- map2( element.names, scale.color, ~{
  ggplot(df,aes(x,y,color=z))+geom_point()+eval(parse(text=paste0("scale_color_",.y,"()")))+ eval(parse(text=paste0("theme_",.x,"()")))+labs(title=paste0("Themes: ",.x,"\ncolors: ",.y)) 
})

wrap_plots( map.hrbrthemes, ncol = 4 )
```

![](ggthemes_present_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
