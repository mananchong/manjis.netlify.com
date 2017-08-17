---
title: Rdatatable中tstrsplit的用法
author: 波
date: '2017-08-16'
output: html_document
from_Rmd: yes
---

[data.table](http://rdatatable.com) 包中的 `tstrsplit` 函数可以用来把 data.frame 中的一列拆分成两列或多列。

平时会看到把年龄保存为数字加单位的形式，比如 "12Y", "23D", "6M". 这样的形式不利于过滤。此时就可以利用刚才提到的 'tstrsplit' 来把年龄列拆分成数值列和单位列，再进行下一步处理。

首先建立一个示例用的 `data.table` : 

```r
library(data.table)
DT <- data.table(name = c("张三", "李四", "王五"), age = c("15Y", 
    "10M", "23D"))
knitr::kable(DT)
```



|name |age |
|:----|:---|
|张三 |15Y |
|李四 |10M |
|王五 |23D |

然后用 `gsub` 把数值和单位之间加上空格：

```r
DT[, `:=`(age_new, gsub("(\\d+)", "\\1 ", age))]
knitr::kable(DT)
```



|name |age |age_new |
|:----|:---|:-------|
|张三 |15Y |15 Y    |
|李四 |10M |10 M    |
|王五 |23D |23 D    |

最后按照空格拆分即可：

```r
DT[, `:=`(c("age_value", "age_unit"), tstrsplit(age_new, " "))]
knitr::kable(DT)
```



|name |age |age_new |age_value |age_unit |
|:----|:---|:-------|:---------|:--------|
|张三 |15Y |15 Y    |15        |Y        |
|李四 |10M |10 M    |10        |M        |
|王五 |23D |23 D    |23        |D        |

