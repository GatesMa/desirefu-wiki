# 一致性保证

因为使用到了缓存，就必须考虑到数据一致性问题，我们需要尽量保证DB里的数据和ES里的数据可以保持一致，我使用到了下面几个方式：

1. AOP

AOP是spring里的一个特性，面向切面编程。AOP是OOP的延续，是软件开发中的一个 热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑 的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

我利用AOP的特性，在新增学生账号和新增队伍的接口上增加了切面，将新增的账号异步处理到ES里，这里的同步跟DB大概有1s的延迟，对于目前来说还是可以接收的。

```java
// 在接口上加上标记注解
@Override
@NeedSyncAddNormalAnnotation
public ResponseEntity<AddNormalAccountRet> addNormalAccount(@ApiParam(value = "创建账号" ,required=true )  @Valid @RequestBody AddNormalAccountRequest body) {
}

/**
 *  取返回值中的accountId, 同步es
 */
@AfterReturning(value = "@annotation(cn.gatesma.desirefu.annotation.NeedSyncAddNormalAnnotation)",
        returning = "response")
public void doAfterAddNormalController(JoinPoint joinPoint, ResponseEntity response) {
    if (joinPoint.getArgs().length < 1) {
        return;
    }

    Object ret = response.getBody();
    if (ret == null) {
        return;
    }

    if (isRetOK(ret) == false) {
        return;
    }

    try {
        Method m = ret.getClass().getMethod("getData");
        Object data = m.invoke(ret);
        if (data != null) {
            m = data.getClass().getMethod("getAccountId");
            Long accountId = (Long) m.invoke(data);
            LOG.info("doAfterAddNormalController accountId: {}", accountId);
            syncAccountToEsProcessor.syncSubAccountToEs(Arrays.asList(accountId));
        }
    } catch (Exception e) {
        LOG.error("doAfterAddNormalController throws an exception! e = ", e);
    }
}
```

2. 定时任务

Linux中crontab是用来定期执行程序的命令。crontab 是用来让使用者在固定时间或固定间隔执行程序之用，换句话说，也就是类似使用者的时程表。对于定时任务有一个表达式来决定任务的执行时间。在Spring里也有类似于Linux的Cron的工具，我们利用这个实现每天晚上8点钟同步最近一天被修改的数据，更新数据到ES里，类似于一种数据对账。

Cron表达式：

```java
*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 6) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
```

每天晚上8点执行任务，表达式为：

> 0 0 20 \* \* ？

定时任务的代码：

```java
@Scheduled(cron = "0 0 20 * * ？") //每天晚上8点执行
public void syncEditData() {

    // 获取最近一天被修改的账号
    Date to = new Date();
    Date from = TimeUtils.getSomeDay(to, -1);

    Timestamp start = new Timestamp(from.getTime());
    Timestamp end = new Timestamp(to.getTime());

    List<Long> accountIdList = normalAccountRepository.getRecentUpdateAccountIdList(start, end);
    
    // 调用Processor处理这些ID
    syncAccountToEsProcessor.syncSubAccountToEs(accountIdList);

}
```











