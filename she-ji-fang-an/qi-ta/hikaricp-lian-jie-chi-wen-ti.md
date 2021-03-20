# Hikaricp连接池问题

项目中的数据库连接池采用了HiKariCP，是数据库连接池的一个后起之秀，号称性能最好，稳定性也不错，完美地PK掉其他连接池。提供一篇文章介绍[主流Java数据库连接池比较及前瞻](https://blog.csdn.net/belalds/article/details/80339479)，文中重点介绍了当前主流开源数据库连接池（比如C3P0、DBCP、Tomcat Jdbc Pool、Druid和Hikaricp）的性能分析和功能比较，有一定的参考价值。

记录项目里遇到的一个坑，采用HiKariCP部署里线上的服务后，刚开始系统可以正常链接数据库并执行sql，但是一段时间之后就会出现超时，并没有高并发的请求：

`Caused by: java.sql.SQLTransientConnectionException: HikariPool-1 - Connection is not available, request timed out after 932347ms.`

**问题分析：**

1、异常抛出时间每次都是在15分30秒左右，但该系统的并行环境并没有此异常，生产环境的差异只是数据库部署在不同的主机上；

2、访问数据库命令的网络耗时正常，数据库端收到连接请求，且该时段数据库连接数并没有耗尽；

3、数据库管理员在异常时间段查询得知，不存在慢sql语句，不存在不合理设置索引导致此异常。

继续分析，只能从Hikaricp的配置着手：以下是出异常的数据源配置 

自此附上hikarecp各个配置的中文解释：[hikariCP连接池配置](https://my.oschina.net/u/2300159/blog/1816537)

```text
<bean id="dataSourcePubdba" class="com.zaxxer.hikari.HikariDataSource">
     <property name="driverClassName" value="${datasource.driver}">
     <property name="jdbcUrl" value="${datasource.jdbcUrl}">
     <property name="username" value="${datasource.user}">
     <property name="password" value="${datasource.pass.encrypt}">
     <property name="connectionTimeout" value="30000" />
     <property name="idleTimeout" value="60000" />
     <property name="maxLifetime" value="1800000" />
     <property name="maximumPoolSize" value="65" />
</bean>
```

后四个重要参数的解释：

    1）connectionTimeout：等待连接池分配连接的最大时长（毫秒），超过这个时长还没可用的连接则发生SQLException， 缺省:30秒 

    2）idleTimeout：此属性控制允许连接在池中闲置的最长时间,此设置仅适用于minimumIdle设置为小于maximumPoolSize的情况 默认:600000\(10minutes\)

   2）maxLifetime：一个连接的生命时长（毫秒），超时而且没被使用则被释放（retired），缺省:30分钟，建议设置比数据库超时时长少30秒

3）maximumPoolSize：连接池中允许的最大连接数\(包括空闲和正在使用的连接\)。缺省值：10；

{% hint style="info" %}
**综合以上分析：**_connectionTimeout采用默认值30s，应该不会有太大问题。maximumPoolSize根据业务量的设置，不会有太大问题。maxLifetime如果过长，且idleTimeout没有时间限制时，会导致连接数很大，空闲连接一直得不到释放，严重挤占资源，容易引起连接数不够的问题。特别是maxLifetime如果大于数据库超时时长，就会抛出数据库连接异常，这也是本次生产问题所在：maxLifetime设置成30分钟，超过了数据库连接时长15分钟，connectionTimeout为30秒，所以每次异常都是在15分30秒抛出，数据库端已经收到Hikaricp连接请求，但是因网络问题等因素，到达超时时长而没有获取到连接。idleTimeout只有在minimumIdle设置为小于maximumPoolSize的情况下才生效，而我没有设置最小空闲连接数minimumIdle的值，minimumIdle默认是等于maximumPoolSize，此时idleTimeout不受限，空闲连接一直没有得到回收，出于系统优化以及并发稳定性考虑，应该增加此配置。_
{% endhint %}

**优化后的配置：**

```text
<bean id="dataSourceTxndba" class="com.zaxxer.hikari.HikariDataSource" destroy-method="close" >//对象销毁前调用了close函数，这是必须的，否则对象离开后，数据库资源还可用
       <property name="driverClassName" value="${datasource.driver}"></property>
       <property name="jdbcUrl" value="${datasource.jdbcUrl2}"></property>
       <property name="username" value="${datasource.user2}"></property>
       <property name="password" value="${datasource.pass2.encrypt}"></property>
       <property name="connectionTimeout" value="30000" />
       <property name="idleTimeout" value="60000" />
       <property name="maxLifetime" value="600000" />
       <property name="minimumIdle" value="10" />
       <property name="maximumPoolSize" value="65" />
       <property name="poolName" value="TxnDatabasePool" />
       <property name="leakDetectionThreshold" value="5000"/>
</bean>
```

**相比较之前的配置，优化点如下：**

{% hint style="info" %}
**1、增加数据连接关闭方法，当从dataSource获取的连接使用完成后，调用close方法，以避免数据源对象任然可用，造成连接泄露**

 **2、增加最小空闲连接数minimumIdle，根据业务场景，设置为10，小于maximumPoolSize值**

**3、连接空闲时间idleTimeout生效，根据业务请求重发频率为40多秒，值可设为1分钟，减少空闲连接占用，尽快释放数据库连接**

**4、连接生命周期maxLifetime值设为10分钟，低于数据库超时时长，尽快释放数据库无效连接**

**5、增加连接池的用户定义名称**

**6、开启连接监测泄露leakDetectionThreshold方法，此属性控制在记录消息之前连接可能离开池的时间量，表明可能的连接泄漏。值代表连接被占用的泄露时间最低可接受值为5秒，不过此值的设定需要根据场景多次调试，如果真实泄露时间小幅度超过5秒，会引起warning，但不一定会导出数据不能入库，因为该方法只是检查，只有到达idleTimeout ，才会强制执行关闭连接。**
{% endhint %}

运行结果：使用该配置，目前系统暂未发生数据库连接异常现象**。**

