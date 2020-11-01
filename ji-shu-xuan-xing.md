---
description: 暂定系统实现需要的语言、框架、数据产品等（开发过程完善）
---

# 技术选型



{% hint style="info" %}
目前的技术方案都是根据经验和喜好可能用到的，后续肯定会有增加一些其他工具的使用，慢慢补充。
{% endhint %}

#### 1. 编程语言

**Java**。对Java比较感兴趣，现在主修Java，对Java较熟悉。后端开发程序员。

#### 2. 前端选型

对前端开发涉猎较少，**微信小程序**之前做过，更熟悉一些，同时，微信小程序上手难度较小一点，而且我们设计是前后端分离的，理论上有时间Web端、Android等都可以在原有的基础上建设即可。搭配**ColorUI**等微信小程序前端框架等。

#### 3. 需要用到的框架

* **SpringBoot。**
* **JOOQ**。选用JOOQ作为ORM持久层框架，使用时间久，较熟悉。
* **gradle**。构建工具，gradle较maven更灵活。
* **Screw**。数据库表结构生成框架，用于生成可视库表页面。
* **HikariCP**。数据库连接池，HikariCP是市面上效率最高的数据库连接池了。全面比Druid，但是Druid可以有数据库监控等，侧重点不一样。
* **Swagger、knife4j。**接口文档框架。
* **logback。**日志框架推荐SLF4J和Logback。
* **Elasticsearch**。es客户端jar。
* **Fastjson**。主流json解析工具。
* **Resilience4j**。熔断。
* ...

#### 4. 工具

* **Redis**。用作缓冲、消息队列、分布式锁等。
* **ElasticSearch**。搜索引擎，用作缓存，对于查询较多的数据，减轻DB压力。
* **Kibana**。配合es可以实现可视化查询搜索。
* **FileBeat**。日志收集工具。



