# 环境配置

安装elastic，elatsic不能用root启动

```text
修改文件权限：chmod 754 start.sh

添加用户：useradd elastic 创建用户不指定组会默认自己名称一个组
修改用户所在组：usermod -g root elastic
添加用户并设置组：useradd -g root elastic
修改用户密码：passwd elastic   (123456)
切换用户：su elastic

修改文件所属组：chgrp -R elastic elasticsearch/
修改文件所属用户：chown -R elastic elasticsearch/
查看用户信息（所在组等）：id elastic


```

启停脚本：

{% code title="start.sh" %}
```bash
#!/bin/sh

cd /usr/local/services/elasticsearch/elasticsearch-5.4.1
./bin/elasticsearch -d
```
{% endcode %}

{% code title="stop.sh" %}
```bash
#!/bin/sh

ps aux|grep elasticsearch|grep -v grep |awk  '{print $2}'| xargs kill -9
```
{% endcode %}



