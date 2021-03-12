# 账号管理

### 1. 账号类型

作为一个比赛组队小程序，目前暂定应该有以下角色：

1. **普通账号**。可以参与到比赛当中，可以与其他队友形成一个队伍“圈子，不能进行创建比赛等特殊操作。
2. **比赛创建人账号**。可以为整个程序新增一个比赛类型，如果有其他想要参数到其中的人，可以进行发起组队，认领其他队友。
3. **OSS账号**。后台运营角色，可以看到整个程序的全部数据，用于统筹调度，处理一些客户的问题，他们可能因为权限不足无法操作，那么OSS就可以派上用场了。**`OSS(Operation Support Systems)  运营支撑系统`**
4. **系统拥有者账号**。理论上只有一个，用于其他权限账号的任免
5. **队伍系统账号。**所有一个队伍的人可以进入一个队伍账号查看信息。

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

账号几张表的模型图：

![&#x6570;&#x636E;&#x6A21;&#x578B;&#x56FE;](../../.gitbook/assets/image%20%2819%29.png)

#### （1）账号注册

以注册学生账号（NormalAccount）为例，涉及两张数据库表：Account\_、NormalAccount，

{% api-method method="post" host="/desire\_fu/v1/normal\_account/add" path="" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



\*\*\*\*

