# 建表SQL

> 建表SQL：

```sql
create database DFU_;
use DFU_;
CREATE TABLE IF NOT EXISTS `Account_` (
  `accountId` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '帐号ID',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `nickName` varchar(256) NOT NULL COMMENT '账号昵称',
  `accountStatus` int(11) NOT NULL COMMENT '账号状态',
  `approvalStatus` int(11) NOT NULL COMMENT '审核状态',
  `memo` varchar(1024) DEFAULT '' COMMENT '备注',
  `auditUserId` bigint(20) NOT NULL COMMENT '审核人userId',
  `auditMsg` varchar(2048) DEFAULT NULL COMMENT '审核结果信息',
  `auditedTime` datetime DEFAULT NULL COMMENT '审核时间',
  `rootUserId` bigint(20) NOT NULL COMMENT '创建人userId',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`accountId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE IF NOT EXISTS `AccountUserRole_` (
  `accountRoleId` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'qq对应生成的openId',
  `accountType` int(11) NOT NULL COMMENT '账号类型',
  `userId` bigint(20) NOT NULL COMMENT '用户ID',
  `accountId` bigint(20) NOT NULL COMMENT '账号ID',
  `role` int(11) NOT NULL COMMENT '系统角色',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人ID',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
  PRIMARY KEY (`accountRoleId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用于存储账号对应用户和角色信息';

CREATE TABLE IF NOT EXISTS `OpenIdQQIdx_` (
  `openId` varchar(128) NOT NULL COMMENT 'qq对应生成的openId',
  `qqNumber` varchar(128) NOT NULL COMMENT 'qq号',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  PRIMARY KEY (`openId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用于存储openId对应qq关联关系';

CREATE TABLE IF NOT EXISTS `OpenIdWXIdx_` (
  `openId` varchar(128) NOT NULL COMMENT 'wx对应生成的openId',
  `wxId` varchar(128) NOT NULL COMMENT '微信ID',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  PRIMARY KEY (`openId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用于存储openId对应wx关联关系';

CREATE TABLE IF NOT EXISTS `User_` (
  `userId` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '用户ID',
  `userName` varchar(200) NOT NULL DEFAULT '' COMMENT 'userName',
  `cellphone` varchar(128) NOT NULL COMMENT '手机号',
  `email` varchar(255) NOT NULL COMMENT '邮箱地址',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建者userId',
  PRIMARY KEY (`userId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE IF NOT EXISTS `UserLogin_` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `userId` bigint(20) NOT NULL COMMENT '用户ID',
  `loginName` varchar(128) NOT NULL COMMENT '请求帐号时的登录名字或者Id',
  `loginNameType` int(11) NOT NULL COMMENT '用户登录账号类型',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `createdUserId` bigint(20) NOT NULL COMMENT '创建人ID',
  `lastModifiedUserId` bigint(20) NOT NULL COMMENT '最后修改人userId',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='用户登录表';

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

CREATE TABLE IF NOT EXISTS `Department_`(
   `departmentId` int(11) NOT NULL AUTO_INCREMENT COMMENT '学院Id',
   `collegeId` int(11) default 0 COMMENT '高校id',
   `name` VARCHAR(1024) default '' COMMENT '学院名称',
   `createdTime` datetime NOT NULL COMMENT '创建时间',
   `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
   `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
   PRIMARY KEY ( `departmentId` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '学院信息';

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

CREATE TABLE IF NOT EXISTS `CompetitionVisible_` (
  `collegeId` int(11) NOT NULL COMMENT '高校ID',
  `competitionId` bigint(20) NOT NULL COMMENT '帐号ID',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `lastModifiedTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`competitionId`, `collegeId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '高校可见比赛表';

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

CREATE TABLE IF NOT EXISTS `Collect_` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'Id',
  `accountId` bigint(20) NOT NULL COMMENT 'AccountID',
  `competitionId` bigint(20) default null COMMENT '收藏的比赛ID',
  `createdTime` datetime NOT NULL COMMENT '创建时间',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `lastModifiedTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '收藏表';


alter table `account_` auto_increment=10000;
alter table `competition_` auto_increment=100;
alter table `organize_` auto_increment=1000;
alter table `User_` auto_increment=100;



```

