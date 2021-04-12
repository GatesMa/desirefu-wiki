# Jmeter

### 1 Jmeter有哪些特征？ <a id="hid-HcGK3R"></a>

* 功能齐全的测试编辑界面，允许快速记录测试计划（来自浏览器或本机应用程序），构建和调试 **【HTTP代理服务器，类似抓包】**
* 命令行模式，可以在任何装了Java环境的系统（win、linux、mac）上进行测试**【移植性好】**
* 提供完整且随时可查看的HTML报告
* 可以在大多数流行的响应格式**（HTML、JSON、XML或任何文本格式）**中提取数据，实现关联**【常说的数据关联】**
* 多线程框架允许通过多个线程进行并发采样，并通过单独的线程组同时对不同的方法进行采样。
* 可以对测试结果进行缓存和离线分析、离线重放

### 2 Jmeter实际使用场景 <a id="hid-wAj4pQ"></a>

* 接口测试
* 压力测试
* 分布式压力测试
* 测试 Restful 风格的API

### 3 Jemter常用图表介绍

一共有三大模块

* Over Time
* Throughput
* Response Times

#### Response times Over Time 

* 脚本运行期间，不同事务（请求）的响应时间变化趋势图
* 包括**事务控制器**样本结果
* **重点：**可以根据响应时间和变化和TPS以及模拟的并发数变化，判断性能拐点的范围
* 一条线代表一个事务（请求）

![](../../.gitbook/assets/image%20%28104%29.png)

#### Response times Percentiles Over Time

* 脚本运行期间，**成功的请求**的响应时间百分比分布图
* 可理解为聚合报告对应的指标（图二）

![](../../.gitbook/assets/image%20%28113%29.png)

#### Bytes throughput Over Time

* 脚本运行期间，吞吐率变化趋势图
* 在容量规划、可用性测试和大文件上传下载场景中，吞吐量是很重要的一个监控和分析指标
* 会**忽略**事务控制器样本结果

![](../../.gitbook/assets/image%20%2895%29.png)

#### Latencies Over Time

* 脚本运行期间，发送一个完整的请求所需时间的变化趋势图
* **可理解理解成：**从发送请求到收到第一个响应所花费的时间
* **包括**事务控制器样本结果

![](../../.gitbook/assets/image%20%2874%29.png)

#### Connect Time Over Time

* 脚本运行期间，事务（请求）**建立连接**所花费的平均时间变化趋势图
* 包括 SSL 三次握手的时间
* 当出现链 Connection Time Out 的错误时，Connect Time 就会等于链接超时时间

![](../../.gitbook/assets/image%20%2893%29.png)

#### Transactions Per Second（最重要）

* 每秒事务数，即 TPS
* 衡量系统处理能力的重要指标
* **包括**事务控制器样本结果

![](../../.gitbook/assets/image%20%2882%29.png)

#### Time Vs Threads

* 平均响应时间和线程数的对应变化曲线
* 可以通过这个对应的变化曲线来作为确定性能拐点的一个参考值
* 可以选中或取消选中下面的 Sampler

![](../../.gitbook/assets/image%20%2877%29.png)

#### Response Time Distribution（重要）

* 响应时间分布图
* 不同响应时间区间内，成功响应数是多少

![](../../.gitbook/assets/image%20%2868%29.png)



