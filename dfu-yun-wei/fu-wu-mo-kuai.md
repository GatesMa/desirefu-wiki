# 服务模块

### 1. 模块目录结构

![](../.gitbook/assets/image%20%2832%29.png)

主目录：/usr/local/services/desire\_fu\_api\_jar-1.0

bin目录：主要存放启动、暂停、重启等可执行脚本

lib目录：服务依赖的所有jar包

log目录：存放服务生成的日志文件

mylib目录：存放服务执行的jar包，名称为 dfu\_service\_api-0.0.1.jar

spring.log目录：spring日志目录，没有此目录服务会报错，需要保留

### 2. 模块主要配置文件，数据文件

模块主要的配置文件分为两种：

服务运行环境配置文件，启动脚本执行时，首先会判断当前运行环境，并传参给服务，服务根据传参决定加载哪一个配置文件，由bin/getenv.sh脚本获取环境变量。

![](../.gitbook/assets/image%20%2825%29.png)

![](../.gitbook/assets/image%20%2830%29.png)

{% hint style="info" %}
环境配置文件目录： /data/release/

env\_prod: 线上环境

env\_dns: 测试环境

env\_dev: 开发环境

env\_preview: 预发环境

env\_sandbox: 沙箱环境
{% endhint %}

服务依赖配置文件，打包在服务执行jar包中，无需关注，服务会根据传参自动对应加载。

### 3. 模块日志文件说明

![](../.gitbook/assets/image%20%2846%29.png)

access.\*log  请求日志，每一个请求都会有一条记录

desire\_fu.\*log 主要的日志文件，服务产生的日志都这这里

error.\*log 错误日志文件

### 4. 模块启停方式

bin/start.sh 启动服务

bin/stop.sh 停止服务

bin/restart.sh 重启服务

或者通过织云平台进行启停。







