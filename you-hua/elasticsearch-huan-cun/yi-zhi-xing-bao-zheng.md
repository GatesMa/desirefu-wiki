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













