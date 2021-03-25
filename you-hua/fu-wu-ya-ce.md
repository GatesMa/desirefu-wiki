# 服务压测

服务压测主要是desirefu后端接口的压测，看看后端接口的平均响应时间等，这里将挑选后端服务40多个接口里最常用的几个接口进行压测，对于响应时间较长的接口进行优化处理。

准备压测的接口列表：

| 接口地址 | 解释 |
| :--- | :--- |
| /desire\_fu/v1/login/can\_login\_account | 查询一个用户可以登录的账号 |
| /desire\_fu/v1/normal\_account/get | 搜索学生账号 |
| /desire\_fu/v1/notification/select | 搜索公告 |
| /desire\_fu/v1/competition/collect | 搜藏比赛 |
| /desire\_fu/v1/competition/select\_scroll | 查找比赛 |
| /desire\_fu/v1/message/select | 获取账号消息 |
| /desire\_fu/v1/organize/list | 搜索组织 |
| /desire\_fu/v1/organize\_application/add | 发起加入组织请求 |
| /desire\_fu/v1/organize\_application/select\_scroll | 获取请求信息 |
| /desire\_fu/v1/user/add | 创建用户 |



