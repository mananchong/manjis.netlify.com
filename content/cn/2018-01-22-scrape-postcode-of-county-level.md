---
title: 抓取2016年省市县行政区划数据库
author: 波
date: '2018-01-22'
slug: scrape-postcode-of-county-level
categories: []
tags:
  - webscrape
from_Rmd: yes
---

上报某项数据时，需要根据邮政编码来判断省份，于是在网上找了下，虽然不合要求，倒是可以拿来练习网页数据的抓取。


```r
library(rvest)
```

```
## Loading required package: xml2
```

```r
doc <- read_html("http://www.cnblogs.com/jiqing9006/p/5849874.html")
```
