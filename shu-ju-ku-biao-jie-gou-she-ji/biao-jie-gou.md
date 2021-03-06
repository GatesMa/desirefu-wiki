---
description: 项目用到的所有mysql建表语句
---

# 表结构

### 数据库

```sql
CREATE DATABASE DFU_;
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

### 3. 用户登陆表

{% hint style="info" %}
UserLogin\_
{% endhint %}

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

### 4. OpenIdWXIdx\_

```sql
CREATE TABLE IF NOT EXISTS `OpenIdWXIdx_`(
   `openId` VARCHAR(128) NOT NULL COMMENT 'wx对应生成的openId',
   `wxId` VARCHAR(128) NOT NULL COMMENT '微信ID',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   PRIMARY KEY ( `openId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '用于存储openId对应wx关联关系';
```

### 5. OpenIdQQIdx\_

```sql
CREATE TABLE IF NOT EXISTS `OpenIdQQIdx_`(
   `openId` VARCHAR(128) NOT NULL COMMENT 'qq对应生成的openId',
   `qqNumber` VARCHAR(128) NOT NULL COMMENT 'qq号',
   `deleteStatus` INT NOT NULL COMMENT '删除状态，0-正常，1-删除',
   PRIMARY KEY ( `openId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '用于存储openId对应qq关联关系';
```

###  6. 用户账号角色信息表

{% hint style="info" %}
AccountUserRole\_
{% endhint %}

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

### 7. 高校信息

{% hint style="info" %}
College\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `College_` (
  `collegeId` int(11) NOT NULL AUTO_INCREMENT COMMENT '学校Id',
  `name` varchar(1024) NOT NULL DEFAULT '' COMMENT '学校名称',
  `ministry` varchar(1024) NOT NULL DEFAULT '' COMMENT '主管部门',
  `identification` varchar(255) DEFAULT '' COMMENT '学校标识码',
  `location` varchar(255) DEFAULT '' COMMENT '所在地',
  `level` varchar(255) DEFAULT '' COMMENT '办学层次',
  `memo` varchar(255) DEFAULT '' COMMENT '备注',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`collegeId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='高校信息';
```

### 8. 学院信息

{% hint style="info" %}
Department\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Department_`(
   `departmentId` int(11) NOT NULL AUTO_INCREMENT COMMENT '学院Id',
   `collegeId` int(11) default 0 COMMENT '高校id',
   `name` VARCHAR(1024) default '' COMMENT '学院名称',
   `createdTime` datetime NOT NULL COMMENT '创建时间',
   `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
   `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
   PRIMARY KEY ( `departmentId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '学院信息';
```

### 9. 普通学生账号

{% hint style="info" %}
NormalAccount\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `NormalAccount_` (
  `accountId` bigint(20) NOT NULL COMMENT '帐号ID',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `collegeId` int(11) NOT NULL COMMENT '学校Id',
  `departmentId` int(11) NOT NULL COMMENT '学院Id',
  `major` varchar(255) NOT NULL COMMENT '专业',
  `stuId` varchar(255) DEFAULT NULL COMMENT '学号',
  `realName` varchar(255) DEFAULT NULL COMMENT '真实姓名',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`accountId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '普通学生账号';
```

### 10. 比赛创建者账号

{% hint style="info" %}
CompetitionCreatorAccount\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `CompetitionCreatorAccount_` (
  `accountId` bigint(20) NOT NULL COMMENT '帐号ID',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `author` varchar(4096) default '' COMMENT '可以创建比赛的权限',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`accountId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '比赛创建者账号';
```

### 11. OSS运营人员账号

{% hint style="info" %}
OSSAccount\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `OSSAccount_` (
  `accountId` bigint(20) NOT NULL COMMENT '帐号ID',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `type` int(11) default NULL COMMENT '什么类型的运营账号，操作者观察者',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`accountId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT 'OSS运营人员账号';
```

### 12. ROOT账号

{% hint style="info" %}
RootAccount\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `RootAccount_` (
  `accountId` bigint(20) NOT NULL COMMENT '帐号ID',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`accountId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT 'ROOT账号';
```

### 13. 比赛

{% hint style="info" %}
Competition\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Competition_` (
  `competitionId` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '帐号ID',
  `accountId` bigint(20) NOT NULL COMMENT '比赛归属帐号ID',
  `accountType` int(11) NOT NULL COMMENT '比赛归属账号类型',
  `status` int(11) NOT NULL COMMENT '0-草稿，1-可见',
  `type` int(11) NOT NULL COMMENT '比赛类型 - 省级、校级等',
  `title` varchar(255) default '' COMMENT '比赛名称',
  `founder` varchar(255) default null COMMENT '创办方',
  `content` text default null COMMENT '具体通知内容，富文本',
  `pv` int(11) default 0 COMMENT '浏览人数',
  `overviewText` varchar(255) default null COMMENT '概览文字',
  `overviewImg` varchar(1024) default null COMMENT '概览图片url',
  `beginTime` datetime default null COMMENT '比赛开始时间',
  `endTime` datetime default null COMMENT '比赛结束时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`competitionId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '比赛';
```

### 14. 高校可见比赛表

{% hint style="info" %}
CompetitionVisible\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `CompetitionVisible_` (
  `collegeId` int(11) NOT NULL COMMENT '高校ID',
  `competitionId` bigint(20) NOT NULL COMMENT '帐号ID',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `lastModifiedTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`competitionId`, `collegeId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '高校可见比赛表';
```

### 15. 文件上传表

{% hint style="info" %}
UploadFile\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `UploadFile_` (
  `fileId` int(11) NOT NULL AUTO_INCREMENT COMMENT '文件ID',
  `fileType` varchar(32) default null COMMENT '文件类型',
  `accountId` bigint(20) default null COMMENT '比赛归属帐号ID',
  `accountType` int(11) default null COMMENT '比赛归属账号类型',
  `fileName` varchar(255) default null COMMENT '文件名称',
  `fileURL` varchar(1024) default null COMMENT '文件Url',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `lastModifiedTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`fileId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '文件上传表';
```

### 16. 公告

{% hint style="info" %}
Notification\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Notification_` (
  `noticeId` int(11) NOT NULL AUTO_INCREMENT COMMENT '公告ID',
  `type` int(11) default null COMMENT '类型',
  `frontImg` varchar(512) default null COMMENT '展示图片',
  `status` int(11) default null COMMENT '状态，是否可见等',
  `content` varchar(1024) default null COMMENT '内容',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `lastModifiedTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`noticeId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '公告';
```

### 17. 组织

{% hint style="info" %}
Organize\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Organize_` (
  `organizeId` bigint(20) NOT NULL COMMENT '圈子Id, 等于account表里的accountId',
  `competitionId` bigint(20) NOT NULL COMMENT '比赛ID',
  `srcAccountId` bigint(20) NOT NULL COMMENT '队长账号Id',
  `createdUserId` bigint(20) DEFAULT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) DEFAULT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`organizeId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '组织';
```

### 18. OrganizeAccount关系表

{% hint style="info" %}
OrganizeAccountRelation\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `OrganizeAccountRelation_` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '圈子Id',
  `organizeId` bigint(20) NOT NULL COMMENT '关系中的organizeId',
  `accountId` bigint(20) NOT NULL COMMENT '关系中的AccountID',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `isOwnerAccount` int(11) NOT NULL COMMENT 'accountId是否是开户账号',
  `createdUserId` bigint(20) DEFAULT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) DEFAULT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT 'OrganizeAccount关系表';
```

### 19. 圈子申请信息表

{% hint style="info" %}
OrganizeAccountApplication\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `OrganizeAccountApplication_` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'Id',
  `organizeId` bigint(20) NOT NULL COMMENT '关系中的organizeId',
  `accountId` bigint(20) NOT NULL COMMENT '关系中的AccountID',
  `accountType` int(11) DEFAULT NULL COMMENT '账号类型',
  `status` int(11) DEFAULT NULL COMMENT '申请状态 0:申请中;1:申请成功;2:客户拒绝;3:用户取消;4:申请过期;',
  `createdUserId` bigint(20) DEFAULT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) DEFAULT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '圈子申请信息表';
```

### 20. 消息表

{% hint style="info" %}
Message\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Message_` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'Id',
  `accountId` bigint(20) NOT NULL COMMENT 'AccountID',
  `type` int(11) default null COMMENT '类型',
  `content` varchar(1024) default null COMMENT '内容',
  `status` int(11) DEFAULT NULL COMMENT '状态 0:未读;1:已读;',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '消息表';
```

### 21. 收藏表

{% hint style="info" %}
Collect\_
{% endhint %}

```sql
CREATE TABLE IF NOT EXISTS `Collect_` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'Id',
  `accountId` bigint(20) NOT NULL COMMENT 'AccountID',
  `competitionId` bigint(20) default null COMMENT '收藏的比赛ID',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '收藏表';
```

