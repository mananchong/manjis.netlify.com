---
title: 博客的建立过程
author: 波
date: '2017-08-05'
---

多年之后，又尝试建立博客。这次用到了 [`blogdown`](https://github.com/rstudio/blogdown) 这个 R 包。

第一次建立的具体步骤如下：

1. 安装 `blogdown` 包：  

    ```r
    devtools::install_github('rstudio/blogdown')
    ```
2. 建立站点：

    ```r
    library(blogdown)
    mkdir("~/my_site")
    new_site()
    ```
    用上面的命令建立的站点，是默认的样子。