---
title: 博客的建立過程
author: bo
date: '2017-08-05'
---

多年之後，又嘗試建立博客。這次用到了[`blogdown`](https://github.com/rstudio/blogdown)這個R包。

第一次建立的具體步驟如下：

1. 安装`blogdown`包：  

    ```
    devtools::install_github('rstudio/blogdown')
    ```
2. 建立站点：

    ```
    library(blogdown)
    mkdir("~/my_site")
    new_site()
    ```
    用上面的命令建立的站點，是默認的樣子。