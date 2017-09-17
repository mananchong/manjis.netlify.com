---
title: dataframe中空值的处理
author: 波
date: '2017-09-17'
slug: dataframe中空值的处理
categories: []
tags: []
from_Rmd: yes
from_Rmd: yes
---

从 EXCEL 读入到 R 里的数据，经常会存在空值。比较常见的情况如下所示


```r
library(data.table)
keShi = c("内科", NA, NA, "外科", NA, NA, NA, "儿科", NA, 
    NA)
set.seed(123)
menZhenRenCi = sample(100:1000, length(keShi))
jiZhenRenCi = sample(100:1000, length(keShi))
menJiZhen <- data.table(keShi, menZhenRenCi, jiZhenRenCi)
knitr::kable(menJiZhen)
```



|keShi | menZhenRenCi| jiZhenRenCi|
|:-----|------------:|-----------:|
|内科  |          359|         962|
|NA    |          809|         508|
|NA    |          467|         709|
|外科  |          892|         614|
|NA    |          943|         192|
|NA    |          140|         906|
|NA    |          572|         320|
|儿科  |          897|         137|
|NA    |          592|         392|
|NA    |          507|         951|

## NA替换为 0

## `na.locf`

## `base R`
