# 登陆模块

登陆模块涉及的最多的一张表是AccountUserRole，用于存储账号对应用户和角色信息，用户登入和绑定账号都是对这张表的增删。

#### （1）查询一个用户可以登录的账号

接口地址：`/desire_fu/v1/login/can_login_account`，是整个系统比较重要的接口之一，也是比较复杂的接口之一，首页微信用户登入小程序后的页面即是请求了这个接口。

接口流程：1. 根据传入的登陆信息（loginName和loginNameType）或者userId，查询AccountUserRole表，获取与User绑定的账号accountId。2. 生成一个账号类型列表，不同类型的账号放到一个map里，最后就形成了不同系统的账号列表，一般来说一个用户的学生账号只有一个，但是可以同时是几个比赛创建账号的用户，也可以是几个运营账号的用户。3. 通过accountId查询Account表获取账号名称等信息，提供给前端展示。

![&#x8BF7;&#x6C42;&#x793A;&#x4F8B;](../../.gitbook/assets/image%20%2821%29.png)

#### （2）用户绑定解绑新的账号

用户在首页账号实际上操作的都是AccountUserRole这张表，所以想要给一个微信用户绑定一个运营账号、比赛创建者账号，我们只需要CRUD这张表即可。接口：添加一个用户可以登录的账号（`/desire_fu/v1/login/addRoleRelation`）、删除RoleRelation（`/desire_fu/v1/login/deleteRoleRelation`）、获取RoleRelation（`/desire_fu/v1/login/selectRoleRelation`）

