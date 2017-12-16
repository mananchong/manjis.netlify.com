---
title: Rdatatable中tstrsplit的用法
author: 波
date: '2017-08-16'
output: html_document
from_Rmd: yes
---

[data.table](http://rdatatable.com) 包中的 `tstrsplit` 函数可以用来把 data.frame 中的一列拆分成两列或多列。

## 事先知道要拆成几列的情况
平时会看到把年龄保存为数字加单位的形式，比如 "12Y", "23D", "6M". 这样的形式不利于过滤。此时就可以利用刚才提到的 'tstrsplit' 来把年龄列拆分成数值列和单位列，再进行下一步处理。

首先建立一个示例用的 `data.table` : 

```r
library(data.table)
library(kableExtra)
dtAge <- data.table(name = c("张三", "李四", "王五"), age = c("15Y", 
    "10M", "23D"))
kable_styling(knitr::kable(dtAge))
```

```
## Currently generic markdown table using pandoc is not supported.
```



|name |age |
|:----|:---|
|张三 |15Y |
|李四 |10M |
|王五 |23D |

然后用 `gsub` 把数值和单位之间加上 `@` 符号：

```r
dtAge[, `:=`(ageNew, gsub("(\\d+)", "\\1@", age))]
knitr::kable(dtAge)
```



|name |age |ageNew |
|:----|:---|:------|
|张三 |15Y |15@Y   |
|李四 |10M |10@M   |
|王五 |23D |23@D   |

最后按照 `@` 拆分即可：

```r
dtAge[, `:=`(c("ageValue", "ageUnit"), tstrsplit(ageNew, "@"))]
knitr::kable(dtAge)
```



|name |age |ageNew |ageValue |ageUnit |
|:----|:---|:------|:--------|:-------|
|张三 |15Y |15@Y   |15       |Y       |
|李四 |10M |10@M   |10       |M       |
|王五 |23D |23@D   |23       |D       |

## 事先不知道要拆成几列的情况

前几天刚好遇到这样的情况，是关于病人按部位做CT的人次的表格。第一列是做的部位，胸部，腹部，脊柱等，这里存在一人同时做几个部位的情况，每个部位都要计算。第二列是扫描方式，是平扫还是增强。第三列是人次。

1. 示例数据^[不一定符合临床实际情况；]如下：
    
    ```r
    library(data.table)
    dtCT <- data.table(buWei = c("胸部,颈部", "腹部,腰椎,颈部", "关节,胸部", 
        "颅脑"), fangShi = c("平扫", "平扫", "增强", "增强"), renCi = c(11, 
        22, 33, 44))
    knitr::kable(dtCT)
    ```
    
    
    
    |buWei          |fangShi | renCi|
    |:--------------|:-------|-----:|
    |胸部,颈部      |平扫    |    11|
    |腹部,腰椎,颈部 |平扫    |    22|
    |关节,胸部      |增强    |    33|
    |颅脑           |增强    |    44|

2. 计算出最多需要拆成几列：
    
    ```r
    maxCol <- max(sapply(strsplit(dtCT$buWei, ","), length))
    maxCol
    ```
    
    ```
    ## [1] 3
    ```

3. 拆成 3 列：
    
    ```r
    dtCT[, `:=`(paste0("buWei_", 1:maxCol), tstrsplit(buWei, ","))][, 
        `:=`(buWei, NULL)]
    knitr::kable(dtCT)
    ```
    
    
    
    |fangShi | renCi|buWei_1 |buWei_2 |buWei_3 |
    |:-------|-----:|:-------|:-------|:-------|
    |平扫    |    11|胸部    |颈部    |NA      |
    |平扫    |    22|腹部    |腰椎    |颈部    |
    |增强    |    33|关节    |胸部    |NA      |
    |增强    |    44|颅脑    |NA      |NA      |

4. 用 `melt` 函数将部位转成一列：
    
    ```r
    ans01 <- melt(dtCT, id.vars = c("fangShi", "renCi"))[!is.na(value)]
    ans01[, `:=`(variable, NULL)]
    setnames(ans01, "value", "newBuWei")
    setcolorder(ans01, c("newBuWei", "fangShi", "renCi"))
    knitr::kable(ans01)
    ```
    
    
    
    |newBuWei |fangShi | renCi|
    |:--------|:-------|-----:|
    |胸部     |平扫    |    11|
    |腹部     |平扫    |    22|
    |关节     |增强    |    33|
    |颅脑     |增强    |    44|
    |颈部     |平扫    |    11|
    |腰椎     |平扫    |    22|
    |胸部     |增强    |    33|
    |颈部     |平扫    |    22|

5. 按拆分后的部位和扫描方式汇总：
    
    ```r
    ans02 <- ans01[, .(heJi = sum(renCi)), keyby = .(newBuWei, fangShi)]
    knitr::kable(ans02)
    ```
    
    
    
    |newBuWei |fangShi | heJi|
    |:--------|:-------|----:|
    |腹部     |平扫    |   22|
    |关节     |增强    |   33|
    |颈部     |平扫    |   33|
    |颅脑     |增强    |   44|
    |胸部     |增强    |   33|
    |胸部     |平扫    |   11|
    |腰椎     |平扫    |   22|

上面 2, 3 两步，可以用 `splitstackshape` 包中的 `cSplit` 来实现，只需一步：

```r
library(splitstackshape)
dtCT <- data.table(buWei = c("胸部,颈部", "腹部,腰椎,颈部", "关节,胸部", 
    "颅脑"), fangShi = c("平扫", "平扫", "增强", "增强"), renCi = c(11, 
    22, 33, 44))
knitr::kable(cSplit(dtCT, "buWei", ","))
```



|fangShi | renCi|buWei_1 |buWei_2 |buWei_3 |
|:-------|-----:|:-------|:-------|:-------|
|平扫    |    11|胸部    |颈部    |NA      |
|平扫    |    22|腹部    |腰椎    |颈部    |
|增强    |    33|关节    |胸部    |NA      |
|增强    |    44|颅脑    |NA      |NA      |

