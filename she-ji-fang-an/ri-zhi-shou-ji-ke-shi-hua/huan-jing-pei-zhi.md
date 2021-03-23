# 环境配置

### 安装elastic

elatsic不能用root启动，文件夹必须切换到新建的用户组下，否则会出现权限问题

```bash
修改文件权限：chmod 754 start.sh

添加用户：useradd elastic 创建用户不指定组会默认自己名称一个组
修改用户所在组：usermod -g root elastic
添加用户并设置组：useradd -g root elastic
修改用户密码：passwd elastic   (123456)
切换用户：su elastic

修改文件所属组：chgrp -R elastic elasticsearch/
修改文件所属用户：chown -R elastic elasticsearch/
查看用户信息（所在组等）：id elastic

查看内核版本：arch
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

启动中的问题：

```bash
[1]: max number of threads [3517] for user [elastic] is too low, increase to at least [4096]
[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[3]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured

设置/etc/security/limits.conf
* soft nproc 5000
* hard nproc 5000
root soft nproc 5000
root hard nproc 5000

设置/etc/sysctl.conf
添加如下配置
vm.max_map_count=655360
并执行
sysctl -p


[1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured

#添加配置
discovery.seed_hosts: ["127.0.0.1"]
 
cluster.initial_master_nodes: ["node-1"]


[1]: max file descriptors [65535] for elasticsearch process is too low, increase to at least [65536]
vim /etc/security/limits.conf
* soft nofile 65536
* hard nofile 65536

vim /etc/profile
ulimit -n
ulimit -n 65536
/etc/init.d/sshd restart

修改后需要重启终端

```

es配置文件：

```yaml
network.host: 0.0.0.0
http.port: 9200
discovery.seed_hosts: ["127.0.0.1"]
cluster.initial_master_nodes: ["node-1"]
```

kibana配置文件：

```yaml
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
kibana.index: ".kibana"
```

filebeat启停：

```yaml
nohup ./filebeat -e -c filebeat.yml &
```

filebeat 安装问题：

```yaml
2020-04-01T15:13:16.939+0800    INFO    instance/beat.go:412    filebeat stopped.
2020-04-01T15:13:16.939+0800    ERROR    instance/beat.go:933    Exiting: 1 error: setting 'filebeat.prospectors' has been removed
Exiting: 1 error: setting 'filebeat.prospectors' has been removed

查看官网有此问题答案：https://discuss.elastic.co/t/filebeat-prospectors-has-been-removed/205563
What is the version of Filebeat your are using? Prospectors has been renamed to inputs in 6.3. So if you simply change prospectors to inputs, it should work.

在6.3版本以后，在配置文件中需要把filebeat.prospectors 修改为filebeat.inputs
```





