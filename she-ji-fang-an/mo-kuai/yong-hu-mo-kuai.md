# 用户模块

用户模块涉及的几张数据库表：User（存储用户数据，可以理解为每一个人在这里有唯一的一条数据）、UserLogin（用户登陆信息表，存储用户登陆数据，如果是微信登陆，保存微信的OpenId，QQ登陆保存QQ号...）

#### （1）创建用户接口流程

接口地址：`/desire_fu/v1/user/add`，接口参数loginName和loginNameType

接口流程：如果传的是loginName和loginNameType，先通过登陆信息查询数据库看是否能得到userId，检查login对应的user是否已存在，如果user已存在，则返回user，user不存在，则通过loginName和loginNameType新增user，分别向User表和UserLogin表插入一条记录即可。

#### （2）查询用户

接口地址：`/desire_fu/v1/user/get`，查询接口，比较简单，如果传的是loginName和loginNameType，那么需要两条sql查询，如果直接传userId，直接一条sql查询数据库即可。

```sql
select * from userLogin_ where loginName='' and loginNameType='' and deleteStatus=0;
select * from user_ where userId = '';
```





