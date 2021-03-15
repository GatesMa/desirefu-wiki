# 角色规则

DesireFU是一个多账号多系统应用，一个微信用户可以登入多个系统，多个微信用户可以登陆同一个账号，那么多个微信登入同一个系统时肯定是有区别的，区别就在登陆账号的角色（Role）上面：例如有些账号是开户账号，有些账号是管理，有些账号只是观察者，登陆账号后只能观察数据，不能进行操作：

![&#x9996;&#x9875;&#x5C55;&#x793A;&#x8D26;&#x53F7;&#x89D2;&#x8272;](../../.gitbook/assets/image%20%2823%29.png)

角色枚举：

```java
ROLE_ROOT(1, "开户账号"),// ROOT用户
ROLE_ADMIN(2, "管理员"),// 管理员
ROLE_OBSERVER(3, "观察者"),// 观察者
```

角色字段存储在账号用户角色关系表中（`AccountUserRole_`），有一个role字段，展示user对于account的角色。

{% page-ref page="../../shu-ju-ku-biao-jie-gou-she-ji/biao-jie-gou.md" %}



