---
description: 日志解析流程
---

# 日志解析

### 1. 不进行解析的结果

一条日志上报到es后，如果不进行解析，那么es会认为这只是一个字段，存储，那么这样的日志可读性较差，需要人为按照规则使用grok正则解析：

![&#x5982;&#x679C;&#x4E0D;&#x8FDB;&#x884C;&#x89E3;&#x6790;&#x7684;&#x7ED3;&#x679C;&#xFF0C;&#x53EF;&#x8BFB;&#x6027;&#x8F83;&#x5DEE;](../../.gitbook/assets/image%20%285%29.png)

### 2. 正则解析grok

解析使用到了es的预处理pipeline，利用了grok的正则匹配对一条日志进行字段解析：

{% hint style="info" %}
[grok正则匹配规则](https://github.com/elastic/logstash/blob/v1.4.2/patterns/grok-patterns)、[grok在线校验地址](https://grokdebug.herokuapp.com/)
{% endhint %}

根据dfu-service的log规则，编写的pipeline如下，pattern一般需要在在线网站上校验配置好，这个比较容易出错：

```javascript
PUT _ingest/pipeline/dfu-log
{
  "description": "dfu-log",
  "processors": [
    {
      "grok": {
        "field": "message",
        "patterns": [
          "%{TIMESTAMP_ISO8601:datetime} %{NOTSPACE:level} %{NOTSPACE:logger} %{NOTSPACE:thread} %{NOTSPACE:version} %{NOTSPACE:trace_id} %{NOTSPACE:span_id} %{NOTSPACE:app_name} %{NOTSPACE:client} %{NOTSPACE:callee} %{NOTSPACE:mod_name} %{NOTSPACE:act_name} %{IP:client_ip} %{IP:server_ip} %{NOTSPACE:exec_time_us} %{NOTSPACE:error_code} %{NOTSPACE:error_msg} %{NOTSPACE:req_method} %{NOTSPACE:rsp_code} %{NOTSPACE:req_url} %{NOTSPACE:req_body} %{GREEDYDATA:rsp_body}"
        ]
      }
    },
    {
      "remove": {
        "field": "message"   // 删除message字段
      }
    },
    {
      "remove": {
        "field": "beat"    // 删除beat字段（无用）
      }
    }
  ]
}
```

执行完成以后我们便生成了一个预处理规则，之后在filebeat上报的时候配置上这个规则即可：

![&#x914D;&#x7F6E;pipeline](../../.gitbook/assets/image%20%286%29.png)

### 3. 解析结果

上诉步奏完成后，之后再有日志记录到`assess.log`文件中，上报到es就不再是只有一个message字段了。每个字段都可以分开，之后我们还可以利用es强大的搜索功能，快速找到我们想要的请求记录，定位问题原因：

![](../../.gitbook/assets/image%20%287%29.png)









