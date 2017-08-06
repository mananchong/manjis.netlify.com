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
3. 在 github 或其他的在线 git 仓库上创建一个新的 repo;
4. 把 my_site 目录里的内容 push 到步骤 3 中建立的 repo 中；
5. 在 netlify.com 上注册一个帐号，在deploy里面设置好与 github repo 的关联；
6. 修改默随机产生的站点名，即 xxx.netlify.com 中的 xxx 部分。