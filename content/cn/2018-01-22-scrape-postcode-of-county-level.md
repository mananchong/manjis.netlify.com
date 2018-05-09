---
title: "抓取2016年省市县行政区划数据库"
author: "波"
date: '2018-01-22'
slug: scrape-postcode-of-county-level
tags: webscrape
categories: []
---

上报某项数据时，需要根据邮政编码来判断省份，于是在网上找了下，虽然不合要求，倒是可以拿来练习网页数据的抓取。


```r
library(rvest)
doc <- read_html('http://www.cnblogs.com/jiqing9006/p/5849874.html')
```

在浏览器的开发者工具里查看之后，发现需要的内容都在`<code>`这个`CSS`标签里，这样就可以取出需要的文本。


```r
postCode <- html_node(doc, css = 'code')
postCode <- html_text(postCode)
```

查看上面`postCode`里的内容，发现里面有很多`\n`，于是可以依此将文本分行。


```r
library(stringi)
postCodeLines <- stri_split_lines(postCode)
postCodeLines <- postCodeLines[[1]]
head(postCodeLines, n = 40)
```

```
##  [1] "/*"                                                              
##  [2] "Navicat MySQL Data Transfer"                                     
##  [3] ""                                                                
##  [4] "Source Server         : 246"                                     
##  [5] "Source Server Version : 50617"                                   
##  [6] "Source Host           : 192.168.1.246:3306"                      
##  [7] "Source Database       : storehelper"                             
##  [8] ""                                                                
##  [9] "Target Server Type    : MYSQL"                                   
## [10] "Target Server Version : 50617"                                   
## [11] "File Encoding         : 65001"                                   
## [12] ""                                                                
## [13] "Date: 2016-09-07 16:07:57"                                       
## [14] "*/"                                                              
## [15] ""                                                                
## [16] "SET FOREIGN_KEY_CHECKS=0;"                                       
## [17] ""                                                                
## [18] "-- ----------------------------"                                 
## [19] "-- Table structure for sh_area"                                  
## [20] "-- ----------------------------"                                 
## [21] "DROP TABLE IF EXISTS `sh_area`;"                                 
## [22] "CREATE TABLE `sh_area` ("                                        
## [23] "  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'ID',"            
## [24] "  `pid` int(11) DEFAULT NULL COMMENT '父id',"                    
## [25] "  `shortname` varchar(100) DEFAULT NULL COMMENT '简称',"         
## [26] "  `name` varchar(100) DEFAULT NULL COMMENT '名称',"              
## [27] "  `merger_name` varchar(255) DEFAULT NULL COMMENT '全称',"       
## [28] "  `level` tinyint(4) DEFAULT NULL COMMENT '层级 0 1 2 省市区县',"
## [29] "  `pinyin` varchar(100) DEFAULT NULL COMMENT '拼音',"            
## [30] "  `code` varchar(100) DEFAULT NULL COMMENT '长途区号',"          
## [31] "  `zip_code` varchar(100) DEFAULT NULL COMMENT '邮编',"          
## [32] "  `first` varchar(50) DEFAULT NULL COMMENT '首字母',"            
## [33] "  `lng` varchar(100) DEFAULT NULL COMMENT '经度',"               
## [34] "  `lat` varchar(100) DEFAULT NULL COMMENT '纬度',"               
## [35] "  PRIMARY KEY (`id`)"                                            
## [36] ") ENGINE=InnoDB AUTO_INCREMENT=3750 DEFAULT CHARSET=utf8;"       
## [37] ""                                                                
## [38] "-- ----------------------------"                                 
## [39] "-- Records of sh_area"                                           
## [40] "-- ----------------------------"
```

```r
tail(postCodeLines)
```

```
## [1] "INSERT INTO `sh_area` VALUES ('3745', '3738', '氹仔岛', '氹仔岛', '中国,澳门特别行政区,氹仔岛', '2', 'taipa', '00853', '999078', null, '113.577669', '22.156838');"                                            
## [2] "INSERT INTO `sh_area` VALUES ('3746', '3745', '嘉模堂区', '嘉模堂区', '中国,澳门特别行政区,氹仔岛,嘉模堂区', '3', 'ourladyofcarmel\\'sparish', '00853', '999078', 'J', '113.565303', '22.149029');"            
## [3] "INSERT INTO `sh_area` VALUES ('3747', '3738', '路环岛', '路环岛', '中国,澳门特别行政区,路环岛', '2', 'coloane', '00853', '999078', 'L', '113.564857', '22.116226');"                                           
## [4] "INSERT INTO `sh_area` VALUES ('3748', '3747', '圣方济各堂区', '圣方济各堂区', '中国,澳门特别行政区,路环岛,圣方济各堂区', '3', 'stfrancisxavier\\'sparish', '00853', '999078', 'S', '113.559954', '22.123486');"
## [5] "INSERT INTO `sh_area` VALUES ('3749', '0', '钓鱼岛', '钓鱼岛', '中国,钓鱼岛', '1', 'diaoyudao', '', '', 'D', '123.478088', '25.742385');"                                                                      
## [6] ""
```

从上面的输出可以看到，第23到34行可以提取出字段的英文名和中文名，第41到倒数第2行是各个县区的具体内容。

先提取字段名


```r
header <- postCodeLines[23:34]
headerEn <- stri_extract_first_regex(header, "(?<=`).*(?=`)")
headerCn <- stri_extract_first_regex(header, "(?<=').*(?=')")
```

再提取具体内容


```r
counties <- postCodeLines[41:(length(postCodeLines)-2)]
counties <- stri_extract_first_regex(counties, "(?<=\\().*(?=\\))")
```

合并为数据框


```r
library(data.table)
counties_dt <- as.data.table(counties)
```

