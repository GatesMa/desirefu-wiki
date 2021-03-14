# 队伍相关

队伍相关的库表有：Organize、OrganizeAccountRelation\_、OrganizeAccountApplication\_。队伍也是一种账号类型，学生账号可以通过向队伍的队长发起入队请求（OrganizeAccountApplication\_），队长同意后，这个账号就绑定到了这个队伍里，我们通过OrganizeAccountRelation\_表来存储这种包含关系。

#### （1）发起入队请求

插入一条OrganizeAccountApplication\_表的记录，status字段标识这个入队请求的状态，枚举如下：

```java
//申请状态 0:申请中;1:申请成功;2:队长拒绝;3:用户取消;4:申请过期;7:OSS直接处理的请求(不在list接口展示);
APPLYING(0),
PASSED(1),
REJECT(2),
CANCEL(3),
EXPIRED(4),
UN_BIND_REQUEST(5),
UN_BIND_CONFIRM(6),
OSS_PROCESS(7);
```

队长的入队请求列表只需要请求status为0的请求，同意或拒绝，只需要更新OrganizeAccountApplication\_这个记录的status字段即可。如果同意的话会添加一条OrganizeAccountRelation\_的记录，标识这个人已经进入了该队伍。

![](../../.gitbook/assets/image%20%2830%29.png)

#### （2）获取组织成员

接口地址/desire\_fu/v1/organize/listMember，参数队伍id，在OrganizeAccountRelation\_查询即可，每一条记录对应着一个人，根据accountId查询账号的具体信息，返回给前端即可。

效果：

![](../../.gitbook/assets/image%20%2828%29.png)

