---
title: 一些sql题目
date: 2020-01-12 15:10:53
categories: sql
tags: [mysql,sql]
---

*为方便观看表都是尽可能简单*

*[推荐测试网站](http://sqlfiddle.com/)*

## 题目1

### 描述

给定一个表，输出每个人观看次数最多的电影

| movie  | name   | id   |
| :----- | :----- | :--- |
| 可以的 | 史锴源 | 1    |
| 好的   | 史锴源 | 2    |
| 可以的 | 史锴源 | 3    |
| 好的   | 陈小琳 | 4    |
| 好的   | 陈小琳 | 5    |
| 可以的 | 陈小琳 | 6    |


```sql
CREATE TABLE `test` (
  `movie` varchar(255) DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  `id` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `test` VALUES ('可以的', '史锴源', '1');
INSERT INTO `test` VALUES ('好的', '史锴源', '2');
INSERT INTO `test` VALUES ('可以的', '史锴源', '3');
INSERT INTO `test` VALUES ('好的', '陈小琳', '4');
INSERT INTO `test` VALUES ('好的', '陈小琳', '5');
INSERT INTO `test` VALUES ('可以的', '陈小琳', '6');
```

### 解答

```sql
SELECT c.movie, c.name, c.visit_times 
FROM (SELECT movie, name, count( * ) AS visit_times 
      FROM test 
      GROUP BY name, movie) c
INNER JOIN (SELECT name, MAX(visit_times) AS max_visits
      FROM (SELECT movie, name, count( * ) AS visit_times 
            FROM test 
            GROUP BY name, movie ) c
      GROUP BY name) m ON m.name = c.name AND m.max_visits = c.visit_times
```

| movie  | name   | visit_times |
| :----- | :----- | :---------- |
| 可以的 | 史锴源 | 2           |
| 好的   | 陈小琳 | 2           |

## 题目二
[stackoverflow](https://stackoverflow.com/questions/12113699/get-top-n-records-for-each-group-of-grouped-results)

