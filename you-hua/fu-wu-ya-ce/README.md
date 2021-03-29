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

### 一  测试内容

本次测试是针对desirefu后台服务进行的压力测试，查看系统最大承受能力。篇幅影响，只展示4个接口的测试过程图表等。

### 二  测试方法

本次采用apache的开源测试工具jmeter。

### 三  测试目标

获取在多台机器部署情况下最大承受能力，并对较慢接口进行优化

### 四  测试环境

| **环境** | 数量 | **机器型号** | **操作系统** | **硬件cpu** | **硬件mem** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| nginx | 1 | x86\_64 | linux | 1核 | 2G |
| 服务端 | 3 | x86\_64 | linux | 2核 | 4G |

### 五  接口性能测试结果

这里直接展示接口的大致QPS，具体每个接口的压测报告我展示了4个，可以在子页面查看。只有三个2核4G的机器，并且nginx的机器是很低端的机器，接口QPS没有特别高，但是在当前机器状况下还不错了，部分效率太低的接口后面会有优化。

| 接口地址 | 解释 | 大致QPS |
| :--- | :--- | :--- |
| /desire\_fu/v1/login/can\_login\_account | 查询一个用户可以登录的账号 | 60 |
| /desire\_fu/v1/normal\_account/get | 搜索学生账号 | 40 |
| /desire\_fu/v1/notification/select | 搜索公告 | 30 |
| /desire\_fu/v1/competition/collect | 搜藏比赛 | 30 |
| /desire\_fu/v1/competition/select\_scroll | 查找比赛 | 25 |
| /desire\_fu/v1/message/select | 获取账号消息 | 35 |
| /desire\_fu/v1/organize/list | 搜索组织 | 15 |
| /desire\_fu/v1/organize\_application/add | 发起加入组织请求 | 30 |
| /desire\_fu/v1/organize\_application/select\_scroll | 获取请求信息 | 5 |
| /desire\_fu/v1/user/add | 创建用户 | 30 |







