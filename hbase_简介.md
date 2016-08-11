#HBase 简介

* HBase 是分布式的，列式存储的数据库

* 有主键（ROW_KEY），列族

* api很简单，基本只有get，put，scan

* 按字典顺序排序，自动去重

* 有列族的概念，一个列族可以有很多列，如c1:a, c1:b, c2:a, c2:b, c2:c

* 系统会自动负载均衡，划分region

* 稀疏，null值不占空间