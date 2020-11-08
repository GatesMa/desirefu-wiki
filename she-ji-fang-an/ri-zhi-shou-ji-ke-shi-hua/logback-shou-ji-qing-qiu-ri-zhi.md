---
description: 使用logback将每一条访问日志，打印到日志文件中
---

# Logback收集请求日志

{% hint style="info" %}
[Logback官网](http://logback.qos.ch/)、[市面上常用日志框架介绍](https://blog.csdn.net/qq_40456064/article/details/109368375)、[es](https://www.elastic.co/cn/elasticsearch/)
{% endhint %}

### 1. Logback配置

配置文件需要配置：日志文件名、文件存储位置、文件日志打印规则`pattern`、滚动策略（配置了按天保存一个文件，保存30天的）等

{% code title="logback.xml" %}
```markup
<!-- 访问监控日志记录（配置了appender，打印日志文件到log/access.log） -->
<appender name="ACCESS" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>./log/access.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>./log/access.%d{yyyy-MM-dd}.log</fileNamePattern>
        <maxHistory>30</maxHistory>
    </rollingPolicy>
    <encoder>
        <pattern>%date{yyyy-MM-dd HH:mm:ss.SSS} %level %logger %thread %message%n</pattern>
    </encoder>
</appender>

<!--trace log日志-->
<logger name="access" level="info" additivity="false">
    <appender-ref ref="ACCESS"/>
</logger>
```
{% endcode %}

上面logback配置完成后，我们只需要在代码里使用`access`这个`logger`即可，使用上面的代码获取一个SLF4j logger，使用下面的代码，产生一条日志记录到文件中：

```java
// for trace log
private static final Logger LOG = LoggerFactory.getLogger("access");

// 打印者一条消息到日志文件里
LOG.info(ACCESS, "message");
```

### 2. SpringBoot使用Filter收集请求信息

使用了Spring的filter机制，对每一个请求进行拦截，具体代码可以查看clone项目

```java
@Component
public class AccessLoggingFilter extends OncePerRequestFilter {
    ...
}
```

新建一个message实体类，对应每一个请求记录，用空格分割每一个字段，方便之后解析

```java
public class Message {

    // 用空格分割每一个字段，
    private final static char TAB = ' ';

    private String version; // 版本号
    private TraceContext context;// trace_id、span_id
    private String appName;// 访问的服务名称
    private String client;// 请求端标识
    private String callee;// 服务端服务名称
    private String modName;// 请求是哪一个模块（类）处理的
    private String actName;// 请求的action，handle方法（方法名称）
    private String clientIp;// 客户端ip
    private String serverIp;// 服务端ip
    private Long duration;// 请求耗时（这个字段很关键）
    private Long errorCode;// 错误码
    private String errorMessage;// 返回信息
    private String reqMethod;// 请求方法GET/POST
    private Long respCode;// 返回代码
    private String reqUrl;// 请求地址
    private String reqBody;// 请求的body
    private String respBody;// 返回值
    
}
```

配合第一条中的logback打印规则，一条正确的请求日志如下：

```java
2020-11-07 17:03:03.167 INFO access http-nio-8088-exec-10 1 
0ad4b92f20d811ebb80d2366033fd577 0 desire_fu UNKNOWN desire_fu 
cn.gatesma.desirefu.controller.api.generate.TestApiController 
hello 127.0.0.1 9.135.14.8 2000 0 OK POST 200 /desire_fu/v1/hello 
{} {"code":0,"message":"OK","data":"Hello! It's DesireFu!"}
```

### 3. 日志收集

首先安装好elasticsearch（一个搜索引擎工具，[官网](https://www.elastic.co/cn/elasticsearch/)），非常强大的数据存储和搜索工具。安装kibana，结合es使用的可视化工具，使用语法可以上网查询，比较容易上手。

之后就是我们的日志收集工具：filebeat，可以将每一条日志上报到es，然后使用kibana展示，配置文件：

```yaml
###################### Filebeat Configuration Example #########################

# This file is an example configuration file highlighting only the most common
# options. The filebeat.full.yml file from the same directory contains all the
# supported options with more comments. You can use it as a reference.
#
# You can find the full configuration reference here:
# https://www.elastic.co/guide/en/beats/filebeat/index.html

#=========================== Filebeat prospectors =============================

filebeat.prospectors:
- input_type: log
  paths:
    - /usr/local/services/desire_fu_api_jar-1.0/log/access.log # 需要收集的日志文件
  fields:
    index: 'dfu_service' # es索引名称

setup.template.name: "dfu-log"
setup.template.pattern: "dfu-log-*"

output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["localhost:9200"]  # es地址
  indices:
    - index: "dfu-service-%{+yyyyMMdd}" # es索引名称
      when.contains:
        fields:
          index: "dfu_service"
  pipeline: "dfu-log"  # 日志预处理
```

{% hint style="info" %}
Linux后台启动服务指令（窗口关闭，服务不关闭）：`nohup <启动指令/脚本> &`
{% endhint %}

* [一般指令后2 &gt;& 1的作用](https://blog.csdn.net/qq_40456064/article/details/108565409)





