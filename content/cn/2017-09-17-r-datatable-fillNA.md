---
title: datatable中空值的填充
author: 波
date: '2017-09-17'
slug: datatable-fillna
categories: []
tags: []
from_Rmd: yes
---

从 EXCEL 读入到 R 里的数据，经常会存在空值。比较常见的情况如下所示


```r
library(data.table)
ke_shi = c("内科", NA, NA, "外科", NA, NA, NA, "儿科", NA, NA)
set.seed(123)
men_zhen_ren_ci = sample(100:1000, length(ke_shi))
ji_zhen_ren_ci = sample(100:1000, length(ke_shi))
men_ji_zhen <- data.table(ke_shi, men_zhen_ren_ci, ji_zhen_ren_ci)
knitr::kable(men_ji_zhen)
```



|ke_shi | men_zhen_ren_ci| ji_zhen_ren_ci|
|:------|---------------:|--------------:|
|内科   |             514|            113|
|NA     |             562|            473|
|NA     |             278|            764|
|外科   |             625|            701|
|NA     |             294|            702|
|NA     |             917|            867|
|NA     |             217|            808|
|儿科   |             398|            190|
|NA     |             328|            447|
|NA     |             343|            748|

## NA替换为 0

## 开发版中新增了相关的函数

## `na.locf`

## `base R`
