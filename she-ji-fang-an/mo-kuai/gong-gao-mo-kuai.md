---
description: Notification
---

# 公告模块

公告模块只涉及一张数据库表Notification\_，但是也是比较重要的一个部分。例如，学生系统的homepage页轮播图跳转到某个比赛的详情页、学生系统的广场页的公告栏、学生系统广场页的第二个轮播图等，都是通过读写这张表完成的。所以具体某个记录属于哪种类型的公告需要有个字段标识（type），具体的枚举如下：

```java
FRONT_PAGE_SWIPER(1, "学生系统首页swiper"),
PARK_SWIPER(2, "广场通知"),
PARK_NOTICE(3, "广场swiper");
```

表结构：

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

#### （1）首页轮播图

首页轮播图的类型是1，实现的效果是在页面上方有轮播图展示几张图片，用户如果点击，就会跳转到比赛的详情页面，所以这个type的公告是一个列表，可以允许同时有几个type为1的列表。

![&#x6548;&#x679C;](../../.gitbook/assets/image%20%2834%29.png)

公告查询请求`/desire_fu/v1/notification/select`接口的一个返回值如下，包含两条记录，也就是上面轮播图里的图片，front\_img字段是图片链接，可以直接展示，content字段里的内容就是对应要跳转的比赛的id，微信小程序前端拿到id后，请求比赛查询接口`/desire_fu/v1/competition/select_scroll`，拿到比赛数据，渲染到比赛展示页即可。

```sql
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "noticeId": 3,
        "front_img": "https://dfu-1257282228.cos.ap-chengdu.myqcloud.com/img/4f2ddd10-35b2-4327-9224-3eed7c8f68fe.png",
        "type": 1,
        "content": "100",
        "status": 1
      },
      {
        "noticeId": 4,
        "front_img": "https://dfu-1257282228.cos.ap-chengdu.myqcloud.com/img/3044f81b-a63a-4e6a-8de7-3b6541223397.jpeg",
        "type": 1,
        "content": "101",
        "status": 1
      }
    ]
  }
}
```

#### （2）广场公告

广场公告效果展示，其实是一个纯文字的公告，比较简单，但是跟轮播图不一样的是这个文字公告只有一条，我们有两个解决方案，可以在添加一条记录后把之前type为2的记录标记为删除，也可以直接添加后，把记录按插入时间倒序排，取第一条即可，但是数据量大的时候查询效率和接口请求时长会变慢：

![](../../.gitbook/assets/image%20%2839%29.png)

请求结果示例：

```sql
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "noticeId": 1,
        "front_img": null,
        "type": 2,
        "content": "小程序及小游戏代码审核将在...",
        "status": 1
      }
    ]
  }
}
```



