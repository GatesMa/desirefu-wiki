# Tomcat线程池优化

Tomcat线程池。在tomcat服务中每-一个用户请求都是一一个线程，所以可以使用线程池来提高性能。

线程池是什么？线程池是一种多线程处理形式，处理过程中将任务添加到队列，然后创建线程后自动启动这些任务，线程池线程都是后台线程。每个线程都使用默认的堆栈大小。它由线程池管理器，工作线程，任务接口，任务队列组成。

在什么情况下使用线程池？单个任务处理的时间短。将需处理的任务的数量大。

有什么好处？1. 减少在创建和销毁线程上所花的时间以及系统资源的开销。2. 如不使用线程池，有可能造成系统创建大量线程而导致消耗完系统内存以及”过度切换”

{% hint style="info" %}
### JDK线程池策略 <a id="articleHeader4"></a>

1. 每次提交任务时，如果线程数还没达到coreSize就创建新线程并绑定该任务。所以第coreSize次提交任务后线程总数必达到coreSize，不会重用之前的空闲线程。
2. 线程数达到coreSize后，新增的任务就放到工作队列里，而线程池里的线程则努力的使用take\(\)从工作队列里拉活来干。
3. 如果队列是个有界队列，又如果线程池里的线程不能及时将任务取走，工作队列可能会满掉，插入任务就会失败，此时线程池就会紧急的再创建新的临时线程来补救。
4. 临时线程使用poll\(keepAliveTime，timeUnit\)来从工作队列拉活，如果时候到了仍然两手空空没拉到活，表明它太闲了，就会被解雇掉。
5. 如果core线程数＋临时线程数 &gt;maxSize，则不能再创建新的临时线程了，转头执行RejectExecutionHanlder。默认的AbortPolicy抛RejectedExecutionException异常，其他选择包括静默放弃当前任务\(Discard\)，放弃工作队列里最老的任务\(DisacardOldest\)，或由主线程来直接执行\(CallerRuns\).
{% endhint %}

Tomcat线程池的大小都会有影响，根据以往的经验和压测结果，我配置的DesireFU后端服务Tomcat线程池如下：

```yaml
# tomcat线程池配置
server.tomcat.min-spare-threads=40
server.tomcat.max-threads=40
server.tomcat.accept-count=1000
server.tomcat.max-connections=10000
```

 





