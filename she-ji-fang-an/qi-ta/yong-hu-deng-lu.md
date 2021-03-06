# 用户登陆

### 1. 登陆简介

我们的程序实际上不仅限于微信小程序，如果有迁移到web或者其他平台的需求，那么登陆的类型是必不可少的。比如一个学生又一个账号，他使用微信登陆，那么以后迁移到web端，我们同样要支持web登陆，在此基础上我们还要支持QQ登陆，学号登陆等，那么登陆类型就必不可少。

### 2. 登陆表结构设计

> User\_表（记录一个User信息）：

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

> UserLogin\_（用户的登陆表，记录可以登陆User的登陆信息）

```sql
CREATE TABLE IF NOT EXISTS `UserLogin_`(
   `id` BIGINT AUTO_INCREMENT NOT NULL COMMENT 'ID',
   `userId` BIGINT NOT NULL COMMENT '用户ID',
   `loginName` VARCHAR(128) NOT NULL COMMENT '请求帐号时的登录名字或者Id',
   `loginNameType` INT NOT NULL COMMENT '用户登录账号类型',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   `createdTime` DATETIME NOT NULL COMMENT '创建时间',
   `createdUserId` BIGINT NOT NULL COMMENT '创建人ID',
   `lastModifiedUserId` BIGINT NOT NULL COMMENT '最后修改人userId',
   `lastModifiedTime` TIMESTAMP NOT NULL COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
   PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '用户登录表';

ALTER TABLE `UserLogin_` AUTO_INCREMENT = 1000;
```

> OpenIdWXIdx\_

```sql
CREATE TABLE IF NOT EXISTS `OpenIdWXIdx_`(
   `openId` VARCHAR(128) NOT NULL COMMENT 'wx对应生成的openId',
   `wxId` VARCHAR(128) NOT NULL COMMENT '微信ID',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   PRIMARY KEY ( `openId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '用于存储openId对应wx关联关系';
```

> OpenIdQQIdx\_

```sql
CREATE TABLE IF NOT EXISTS `OpenIdQQIdx_`(
   `openId` VARCHAR(128) NOT NULL COMMENT 'qq对应生成的openId',
   `qqNumber` VARCHAR(128) NOT NULL COMMENT 'qq号',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   PRIMARY KEY ( `openId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '用于存储openId对应qq关联关系'; 
```

> AccountUserRole\_（账号用户关联关系表）

```sql
CREATE TABLE IF NOT EXISTS `AccountUserRole_`(
   `accountRoleId` BIGINT NOT NULL AUTO_INCREMENT COMMENT 'qq对应生成的openId',
   `userId` BIGINT NOT NULL COMMENT '用户ID',
   `accountId` BIGINT NOT NULL COMMENT '账号ID',
   `role` INT NOT NULL COMMENT '系统角色',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   `createdTime` DATETIME NOT NULL COMMENT '创建时间',
   `createdUserId` BIGINT NOT NULL COMMENT '创建人ID',
   `lastModifiedUserId` BIGINT NOT NULL COMMENT '最后修改人userId',
   `lastModifiedTime` TIMESTAMP NOT NULL COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
   PRIMARY KEY ( `accountRoleId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '用于存储账号对应用户和角色信息';

ALTER TABLE `AccountUserRole_` AUTO_INCREMENT = 1000;
```

### 3. 登陆相关接口实现

