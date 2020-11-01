# 账号管理

### 1. 账号类型

作为一个比赛组队小程序，目前暂定应该有以下角色：

1. **普通账号**。可以参与到比赛当中，可以与其他队友形成一个队伍“圈子，不能进行创建比赛等特殊操作。
2. **比赛创建人账号**。可以为整个程序新增一个比赛类型，如果有其他想要参数到其中的人，可以进行发起组队，认领其他队友。
3. **OSS账号**。后台运营角色，可以看到整个程序的全部数据，用于统筹调度，处理一些客户的问题，他们可能因为权限不足无法操作，那么OSS就可以派上用场了。
4. **系统拥有者账号**。理论上只有一个，用于其他权限账号的任免

目前设计方案，账号类型枚举：

```java
NORMAL(1, "NORMAL"), // NORMAL类型的账号，可以参与组队
COMPETITION_CREATOR(2, "COMPETITION_CREATOR"),// 比赛创建者账号
OSS(3, "OSS"), // 运营人员账号
SUPER_USER(4, "SUPER_USER");// 最高权限账号，原则上只有一个，用于OSS账户、比赛创建者账户任免等顶级权限操作
```

### 2. 账号表结构设计

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
```

### **3. 账号相关接口实现（注册、修改）**

\*\*\*\*

