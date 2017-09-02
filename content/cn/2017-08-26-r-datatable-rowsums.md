---
title: Rdatatable中按行求和
author: 波
date: '2017-08-26'
slug: Rdatatable中按行求和
categories: []
tags: []
from_Rmd: yes
---

在实际工作中会遇到按行求和的情况，如下表所示，知道了门诊人次，急诊人次，想在最右边加一列门急诊合计人次。

```r
library(data.table)
menJiZhen <- data.table(keShi = c("内科", "外科", "骨科", 
    "妇产科", "儿科"), menZhenRenCi = c(555, 666, 777, 888, 
    999), jiZhenRenCi = c(55, 66, 77, 88, 99))
knitr::kable(menJiZhen)
```



|keShi  | menZhenRenCi| jiZhenRenCi|
|:------|------------:|-----------:|
|内科   |          555|          55|
|外科   |          666|          66|
|骨科   |          777|          77|
|妇产科 |          888|          88|
|儿科   |          999|          99|

## `rowSums`

```r
menJiZhen[, `:=`(heJi, rowSums(.SD)), .SDcols = c("menZhenRenCi", 
    "jiZhenRenCi")]
knitr::kable(menJiZhen)
```



|keShi  | menZhenRenCi| jiZhenRenCi| heJi|
|:------|------------:|-----------:|----:|
|内科   |          555|          55|  610|
|外科   |          666|          66|  732|
|骨科   |          777|          77|  854|
|妇产科 |          888|          88|  976|
|儿科   |          999|          99| 1098|



## `Reduce`加`+`
