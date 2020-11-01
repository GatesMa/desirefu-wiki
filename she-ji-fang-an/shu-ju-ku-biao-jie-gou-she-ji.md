---
description: DFU的数据库表结构设计页
---

# 数据库表结构设计

### 数据库

```sql
CREATE DATABASE DFU;
```

### 1. 账号表

{% hint style="info" %}
Account\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Account_`(
   `accountId` BIGINT AUTO_INCREMENT NOT NULL COMMENT '帐号ID',
   `accountType` INT NOT NULL COMMENT '账号类型',
   `nickName` VARCHAR(256) NOT NULL COMMENT '账号昵称',
   `accountStatus` INT NOT NULL COMMENT '账号状态',
   `approvalStatus` INT NOT NULL COMMENT '审核状态',
   `memo` VARCHAR(1024) NOT NULL COMMENT '备注',
   `auditUserId` BIGINT NOT NULL COMMENT '审核人userId',
   `auditMsg` VARCHAR(2048) NOT NULL COMMENT '审核结果信息',
   `auditedTime` DATETIME NOT NULL COMMENT '审核时间',
   `createdUserId` BIGINT NOT NULL COMMENT '创建人userId',
   `createdTime` DATETIME NOT NULL COMMENT '创建时间',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   `lastModifiedUserId` BIGINT NOT NULL COMMENT '最后修改人userId',
   `lastModifiedTime` TIMESTAMP NOT NULL COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
   PRIMARY KEY ( `accountId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE `Account_` AUTO_INCREMENT =100000;
```

### 2. 用户表

{% hint style="info" %}
User\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `User_`(
   `userId` BIGINT AUTO_INCREMENT NOT NULL COMMENT '用户ID',
   `userName` INT NOT NULL COMMENT '用户名',
   `cellphone` VARCHAR(128) NOT NULL COMMENT '手机号',
   `email` VARCHAR(255) NOT NULL COMMENT '邮箱地址',
   `memo` VARCHAR(1024) NOT NULL COMMENT '备注',
   `createdTime` DATETIME NOT NULL COMMENT '创建时间',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   `lastModifiedUserId` BIGINT NOT NULL COMMENT '最后修改人userId',
   `lastModifiedTime` TIMESTAMP NOT NULL COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
   PRIMARY KEY ( `userId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE `User_` AUTO_INCREMENT =10000;
```





