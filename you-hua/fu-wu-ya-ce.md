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

本次测试是针对desirefu后台服务进行的压力测试，查看系统最大承受能力。

### 二  测试方法

本次采用apache的开源测试工具jmeter。

### 三  测试目标

获取在多台机器部署情况下最大承受能力，并对较慢接口进行优化

### 四  测试环境

| **环境** | 数量 | **机器型号** | **操作系统** | **硬件cpu** | **硬件mem** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| nginx | 1 | x86\_64 | linux | 1核 | 2G |
| 服务端 | 3 | x86\_64 | linux | 2核 | 4G |

### 五  性能测试结果与分析

#### 6.1 /desire\_fu/v1/login/can\_login\_account（查询一个用户可以登录的账号）

| **线程数量** | **\#Samples** | 平均 | **Error%** |
| :--- | :--- | :--- | :--- |
| 10 | 3000 | 247 | 0.00% |
| 20 | 4000 | 352 | 0.00% |
| 40 | 5000 | 653 | 0.00% |
| 80 | 8000 | 1462 | 0.98% |

可以看到在80的时候出现了异常请求，其实正常压测到60的时候已经少量的异常请求了。这应该已经达到了服务可以处理的极限了，因为我们只有三个2核4G的机器，并且nginx的机器是很低端的机器。













