---
title: data.table 中删除列的方法
author: 波
date: '2017-09-18'
slug: data.tablek中删除列的方法
categories: []
tags: []
from_Rmd: yes
from_Rmd: yes
---

从 EXCEL 读入到 R 里的数据，经常会存在多余的列。比如下表中的以 `X` 开头的列：


```r
library(data.table)
keShi = c("内科", NA, NA, "外科", NA, NA, NA, "儿科", NA, NA)
set.seed(123)
RenCi = sample(100:1000, length(keShi))
X_1 = rep("NA", length(keShi))
X_2 = rep("NA", length(keShi))
menJiZhen <- data.table(keShi, RenCi, X_1, X_2)
knitr::kable(menJiZhen)
```



|keShi | RenCi|X_1 |X_2 |
|:-----|-----:|:---|:---|
|内科  |   359|NA  |NA  |
|NA    |   809|NA  |NA  |
|NA    |   467|NA  |NA  |
|外科  |   892|NA  |NA  |
|NA    |   943|NA  |NA  |
|NA    |   140|NA  |NA  |
|NA    |   572|NA  |NA  |
|儿科  |   897|NA  |NA  |
|NA    |   592|NA  |NA  |
|NA    |   507|NA  |NA  |

包作者推荐的删除多余列的方式是这样的：

```r
colsToDelete <- c("X_1", "X_2")
menJiZhen[, `:=`((colsToDelete), NULL)]
knitr::kable(menJiZhen)
```



|keShi | RenCi|
|:-----|-----:|
|内科  |   359|
|NA    |   809|
|NA    |   467|
|外科  |   892|
|NA    |   943|
|NA    |   140|
|NA    |   572|
|儿科  |   897|
|NA    |   592|
|NA    |   507|

