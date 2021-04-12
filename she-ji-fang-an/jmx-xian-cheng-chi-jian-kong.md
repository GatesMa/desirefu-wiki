# JMX对Tomcat线程池监控

Tomcat也是有连接池的，用于管理服务接受的请求，Tomcat连接池和平常的线程池一样，有这同样的几个重要参数，对Tomcat的线程池进行监控也是一个非常重要的工作，一个示例是服务实际处理时间比较短，但是很长时间才有返回值，有可能就是Tomcat收到了太多请求，一些请求被阻塞了，导致请求时长增加。

`JMX（Java Management Extensions）`，监控管理框架，通过使用`JMX`可以监控和管理应用程序。`JMX`最常见的场景是监控`Java`程序的基本信息和运行情况，任何Java程序都可以开启`JMX`，然后使用`JConsole`或`Visual VM`进行预览。

**线程池简单介绍**

线程池是线程的管理工具，通过使用线程池可以复用线程降低资源消耗、提高响应速度、提高线程的可管理性。如果在系统中大量使用线程池，就必须对线程池进行监控方便出错时定位问题。可以通过线程池提供的参数进行监控，线程池提供的参数如下：

| 方法 | 含义 |
| :--- | :--- |
| getActiveCount | 线程池中正在执行任务的线程数量 |
| getCompletedTaskCount | 线程池已完成的任务数量 |
| getCorePoolSize | 线程池的核心线程数量 |
| getLargestPoolSize | 线程池曾经创建过的最大线程数量 |
| getMaximumPoolSize | 线程池的最大线程数量 |
| getPoolSize | 线程池当前的线程数量 |
| getTaskCount | 线程池需要执行的任务数量 |

**定时任务监控线程池**

每5秒执行一次，打印当前服务的tomcat连接池的状态：连接池大小、活跃链接数、空闲连接数等，打成一条日志到日志文件里，在日志文件里我们只需要grep一下就可以知道当前服务的连接池状态了。

```java
@Scheduled(cron = "0/5 * * * * ?") //每5秒执行一次
public void metricsReport() {
        try {
            MBeanServer mBeanServer = ManagementFactory.getPlatformMBeanServer();
            ObjectName metrics = new ObjectName("org.springframework.boot:type=Endpoint,name=metricsEndpoint");
            Map<String, Object> dataMap = (Map<String, Object>) mBeanServer.getAttribute(metrics, "Data");
        
            int tomcatPoolSize = dataMap.get("tomcat.threads.pool_size") == null ? -1
                    : (Integer) dataMap.get("tomcat.threads.pool_size");
            int tomcatActiveCount = dataMap.get("tomcat.threads.active_count") == null ? -1
                    : (Integer) dataMap.get("tomcat.threads.active_count");
            int tomcatQueueSize =
                    dataMap.get("tomcat.threads.queue") == null ? -1 : (Integer) dataMap.get("tomcat.threads.queue");
            int largestPoolSize =
                    dataMap.get("tomcat.threads.largest_pool_size") == null ? -1 : (Integer) dataMap.get("tomcat.threads.largest_pool_size");
            int submittedCount =
                    dataMap.get("tomcat.threads.submitted_count") == null ? -1 : (Integer) dataMap.get("tomcat.threads.submitted_count");
        
            // 打印日志文件
            logger.info("[poolSize={},activeCount={},queueSize={},largestPoolSize={},submittedCount={}]",
                            tomcatPoolSize, tomcatActiveCount, tomcatQueueSize,largestPoolSize,submittedCount);
        
        } catch (Exception e) {
            logger.error("occur error!", e);
        }
}
```

日志grep命令：cat desire\_fu.log \| grep 'poolSize'：

![](../.gitbook/assets/image%20%2885%29.png)





