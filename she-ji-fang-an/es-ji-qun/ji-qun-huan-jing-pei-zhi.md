# 集群环境配置

### es集群配置

es版本选择: 5.4.1，不同版本的es配置起来差距较大

配置了两台机器，一主一从。106.13.202.72    119.3.184.74    

### 配置文件   

{% code title="node1" %}
```yaml
#需要设置成一样的才能加入到集群
cluster.name: my-application
#每个节点的名称不能相同
node.name: node-1
# master节点
node.master: true
node.data: true
node.max_local_storage_nodes: 1
# 绑定本机ip, 否则不能远程访问
network.bind_host: 0.0.0.0
network.publish_host: 106.13.202.72
http.port: 9200
transport.tcp.port: 9300
# 节点列表
discovery.zen.ping.unicast.hosts: ["106.13.202.72:9300","119.3.184.74:9300"]

bootstrap.memory_lock: false
bootstrap.system_call_filter: false

http.cors.enabled: true
http.cors.allow-origin: "*"

discovery.zen.ping_timeout: 10s
discovery.zen.fd.ping_timeout: 300s
discovery.zen.fd.ping_interval: 3s
discovery.zen.fd.ping_retries: 10
thread_pool.search.queue_size: 3000
thread_pool.bulk.queue_size: 2000
thread_pool.index.queue_size: 1000
discovery.zen.minimum_master_nodes: 1
```
{% endcode %}

{% code title="node2" %}
```yaml
cluster.name: my-application
node.name: node-2
node.master: false
node.data: true
node.max_local_storage_nodes: 1
network.bind_host: 0.0.0.0
network.publish_host: 119.3.184.74
http.port: 9200
transport.tcp.port: 9300
discovery.zen.ping.unicast.hosts: ["106.13.202.72:9300","119.3.184.74:9300"]

bootstrap.memory_lock: false
bootstrap.system_call_filter: false

http.cors.enabled: true
http.cors.allow-origin: "*"

discovery.zen.ping_timeout: 10s
discovery.zen.fd.ping_timeout: 300s
discovery.zen.fd.ping_interval: 3s
discovery.zen.fd.ping_retries: 10
thread_pool.search.queue_size: 3000
thread_pool.bulk.queue_size: 2000
thread_pool.index.queue_size: 1000
discovery.zen.minimum_master_nodes: 1
```
{% endcode %}

### 测试集群

因为ElasticSearch原生就是支持集群的，启动单个es节点也是集群，每个集群都有一个名称，默认的集群名称为 elasticsearch ，同样每个 elasticsearch 节点也都有名称，如果不指定，ElasticSearch会从自己的配置文件中随机选出一个作为自己的名称。

所以在ElasticSearch中启动集群是很简单的事情，启动其他的节点，可以在其上 重复第一步和第二步的操作 ，如果不希望修改集群名称和节点名称，那么ElasticSearch通过使用局域网广播自动发现机制寻找默认集群名称的所有节点，最终拥有相同集群名称的节点就自动的构成了一个ElasticSearch集群，不用做其他繁琐的配置，这样一个集群环境就搭建好了。这时可以在任意一台服务器使用 curl 'localhost:9200/\_cat/health?v' 查看集群健康状态了，再比如：

```bash
   curl 'localhost:9200/_cat/nodes?v'   查看集群中所有的节点信息
   curl 'localhost:9200/_cat/indices?v' 查看所有索引
   curl -XPUT 'localhost:9200/customer?pretty' 创建索引名称为customer的索引
   curl -XPUT 'localhost:9200/customer/external/1?pretty' -d '{"name": "John Doe"}' 索引一篇文档，类型为external，文档ID：1
   curl -XGET 'localhost:9200/customer/external/1?pretty' 查询一篇文档
   curl -XDELETE 'localhost:9200/customer?pretty' 删除索引名称为customer的索引
```

### 常见问题

```yaml
org.elasticsearch.discovery.MasterNotDiscoveredException: null
```

在每个配置文件指定初始节点：cluster.initial\_master\_nodes: node-1

```yaml
it is forbidden to set both [discovery.seed_hosts] and [discovery.zen.ping.unicast.hosts]
master not discovered or elected yet, an election requires
```

ES 7.0版本集群配置报错，没发现master的异常。 \[hadoop-1\] master not discovered or elected yet, an election requires a node with id \[CCzmifRuSm6RxolhJ7XfDw\], have discovered \[\] which is not a quorum; discovery will continue using \[\]

首先，很多问题都出现在第一次配置失败。假如你Es项目路径下有建立了datas的目录，那就要在每次改配置的时候去清掉里面的东西，像是缓存垃圾，导致后面每次修改都不生效。7.0后的集群 配置是discovery.seed\_hosts: \[“IP\_1:9300”, “IP\_2:9300”, “IP\_3:9300”\]，之前的版本使用discovery.zen.ping.unicast.hosts，不能同时配置。

```yaml
[1]: memory locking requested for elasticsearch process but memory is not locked
```

修改elasticsearch.yml中的配置： bootstrap.memory\_lock: false

