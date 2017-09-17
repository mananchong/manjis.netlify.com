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
menJiZhen <- data.table(
  keShi = c("内科", NA, NA, "外科", NA, NA, NA, "儿科", NA, NA), 
  menZhenRenCi = runif(length(keShi), min = 100, max = 1000), 
  jiZhenRenCi = runif(length(keShi), min = 100, max = 1000))
knitr::kable(menJiZhen)
```
