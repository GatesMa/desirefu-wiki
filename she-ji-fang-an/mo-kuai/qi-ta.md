# 其他

### （1）比赛

在小程序中，比赛其实是一种以文章通知的形式存在的，而且不是纯文本，支持图片等复杂的格式，采用了富文本编辑器，使用起来更加友好，对于文章也有一张数据表：Competition\_，其中的content字段存储文章内容（富文本，类似于html的标签文本）：

![&#x5BCC;&#x6587;&#x672C;&#x793A;&#x4F8B;](../../.gitbook/assets/image%20%2820%29.png)

### （2）消息

消息模块也是比较复杂的模块，系统发送的消息涉及消息的类型、消息是否已读等，所以我设计的表结构如下，type字段表示类型，status标识是否已读，accountid是发送给哪一个账号。为满足前端展示需求，消息模块的接口有：删除消息（/desire\_fu/v1/message/delete）、全读消息（/desire\_fu/v1/message/read\_all\_mag）、获取消息（/desire\_fu/v1/message/select）、更新状态（/desire\_fu/v1/message/update\_status）。消息类型字段对应着一个枚举，不同类型的消息对应一个消息标题。

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

// 枚举
COMMON_MSG(1, "常规消息"),
JOIN_ORGANIZE(2, "入队请求"),
LEAVE_ORGANIZE(3, "离队消息"),
JOIN_ORGANIZE_SUCCESS(4, "入队请求审批通过"),
JOIN_ORGANIZE_FAIL(5, "入队请求被拒绝"),
CREATE_ORGANIZE(6, "创建队伍成功")
```

