---
description: DesireFu怎么实现不需要账号密码登陆
---

# 微信登陆

DesireFU没有维护一套账号密码服务，不需要用户去注册账号、记密码，而是用微信账号来唯一标识一个用户，用户即使更换手机，只要是同一个微信号，就可以实现无密码登陆。

### 1. 登陆简介

我们的程序实际上不仅限于微信小程序，如果有迁移到web或者其他平台的需求，那么登陆的类型是必不可少的。比如一个学生又一个账号，他使用微信登陆，那么以后迁移到web端，我们同样要支持web登陆，在此基础上我们还要支持QQ登陆，学号登陆等，那么登陆类型就必不可少。



### 2. OpenID

openid是微信用户的唯一用户标识（用户不同，则获取到的openid就不同），可用于永久标记一个用户

{% page-ref page="wx-yong-hu-openid-huo-qu.md" %}

### 3. 用户数据模型设计

![&#x7528;&#x6237;&#x6A21;&#x578B;](../../.gitbook/assets/image%20%2839%29.png)

一个用户对应多个登陆信息，例如一个账号可以用微信登陆，可以用QQ登陆（目前只有微信登陆的例子），这个信息保存在`UserLogin_` 表中，有两个字段loginName和loginNameType，loginNameType的枚举如下，loginName对应登录名，如果是微信登陆，这个字段是微信账号的OpenID，从第一步获得，如果是QQ登陆，这个字段就是QQ号，以此类推。

```java
LOGIN_NAME_TYPE_QQ(1, "QQ"),
LOGIN_NAME_TYPE_WX(2, "WX");
```

通过AccountUserRole表，将User用户与账号Account进行多对多的绑定就可以实现用户多账号，账号多应用。

### 4. 登陆流程

微信小程序前端登陆流程（请求接口和保存数据的顺序）：

#### （1）调用微信端接口getUserInfo

获取微信用户的userInfo，包含微信昵称、头像等信息。获取后保存在globalData中全局使用。

#### （2）改进的wxLogin方法

先调用微信提供的wx.login方法获取code，发送 res.code 到后台换取 openId, sessionKey, unionId，这里调用的后端接口是`/desire_fu/v1/external/code_2_wx_openid`，获取当前登陆的微信的openId后，保存到本地缓存中，下次登陆可以直接使用微信的小程序缓存，因为微信提供的获取openId的接口是很慢的，微信端不保证效率，一般情况下要5s左右才有返回，很慢，所以缓存openId下次就不用请求了。

```javascript
// 缓存openId
wx.getStorage({
    key: 'openId',
    success: function (res) {
      that.globalData.openId = res.data;
      console.log('cache find openId:' + that.globalData.openId)
      return
    }
  })
```

#### （3）获取userId

获取到openId后，相当于知道了loginName和loginnameType，对于UserLogin表来说，这两个字段是唯一索引，可以用这两个字段直接查询得到userid，建了索引的话，这条sql执行还是很快的。

```sql
select * from userLogin_ where loginName='' and loginNameType=''
```

调用一个查询接口查询到userid后，微信小程序端将这个userId保存到globalData全局变量中。

#### （4）通过userId获取首页账号数据

有了userId后，相当于已经知道了用户的唯一身份，因为userId在我们的系统里是可以唯一确定一个用户的。在AccountUserRole表中，我们就可以查询出这个userId绑定的账号以及对于的角色信息。

```sql
select * from AccountUserRole_ where userId = '';
```

至此，首页的数据已经全部加载完成，利用微信账号进行无密码登陆完成。过程中的缺陷是用户第一次登陆应用的时候会有些卡顿，原因是微信的接口限制，以及我们需要用openId进行新建用户等写操作，第一次加载完成后，各种缓存那么后面再次登陆应用的时候就会快很多。

### 5. 登陆相关表结构设计

登陆涉及的几张数据库表已经放在下面：

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



