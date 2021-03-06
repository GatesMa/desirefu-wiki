# 数据库访问日志监控

有时候一个请求执行的快慢，很大程度在与业务复杂度和访问数据库的快慢上，一条SQL有时候会执行的很慢导致请求超时等，所以对一个请求中执行的SQL耗时问题进行监控是很有必要的，我们采用的是Spring的AOP在执行SQL前和后加入时间，执行完成之后打印执行的具体SQL以及耗时，结果有误差一般偏大，但是已经够用了。**这个数据库访问耗时其实是可以把数据上传到es里通过kibana进行可视化的，但是工作量还是有点大，后期优化考虑**。

**效果：**

![](../.gitbook/assets/image%20%28112%29.png)

> elapse='90,'db='DFU\_',layer='1',method='queryNotification',parameter='\[null, 2, null\]'

90单位是ms，是执行耗时，DFU是数据库名称，同时还有执行方法名，参数等信息。

**实现方法：**

先声明一个注解，用于标记在执行sql的方法上：

```text
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Inherited
public @interface DIAccessMo {
  String table() default "";
  String db() default "";
  String crud() default "";
}

/**
 * 通过id查询账号
 */
@DIAccessMo(table = "Account_", db = "DFU_")
public Account_Record getAccountById(Long accountId, DeleteStatus deleteStatus) {
    ...
}
```

 利用Spring的切面，在方法执行前记录当前时间，保存到ThreadLocal变量里，threadlocal而是一个线程内部的存储类，可以在指定线程内存储数据，数据存储以后，只有指定线程可以得到存储数据，这样可以保证每一个请求的线程保存的数据都是唯一的。在方法执行完成之后，用当前时间减去刚才记录的时间就是耗时，同时将方法名称等信息打印到日志里即可。

{% hint style="info" %}
ThreadLocal：多线程访问同一个共享变量的时候容易出现并发问题，特别是多个线程对一个变量进行写入的时候，为了保证线程安全，一般使用者在访问共享变量的时候需要进行额外的同步措施才能保证线程安全性。ThreadLocal是除了加锁这种同步方式之外的一种保证一种规避多线程访问出现线程不安全的方法，当我们在创建一个变量后，如果每个线程对其进行访问的时候访问的都是线程自己的变量这样就不会存在线程不安全问题。
{% endhint %}

```java
private void report() {
  try {
    LinkedList<Elapse> elapseChan = context.get();
    if(CollectionUtils.isNotEmpty(elapseChan)) {
      int layer = elapseChan.size();
      Elapse elapse = elapseChan.removeLast();
      if(elapse!=null) {
        DIAccessMo mo=elapse.getMo();
        String method= elapse.getMethod();
        String params= elapse.getParams();
        String db=mo==null?"unknown":mo.db();
        long duration=System.currentTimeMillis() - elapse.getTime();
        logger.info("elapse='{},'db='{}',layer='{}',method='{}',parameter='{}'",duration,db,layer,method,params);
      }
    }
  } catch (Throwable e) {
    context.remove();
    logger.error("error!", e);
  }
}
```



