# 账号相关

与账号有关的几张数据库表：Account、NormalAccount、OSSAccount、CompetitionCreatorAccount、RootAccount、Organize\_。其中Account表的主键作为其他几张表的主键，可以理解为外键，但是实际上我们并没有建立外键的关系，而是用程序来保证数据一致性，原因可以查看这里：

{% page-ref page="../../shu-ju-ku-biao-jie-gou-she-ji/she-ji-xiang-guan/wai-jian-shi-yong.md" %}

![&#x6570;&#x636E;&#x6A21;&#x578B;](../../.gitbook/assets/image%20%2838%29.png)

账号模块主要涉及两个大的接口，一个是账号创建、一个是账号修改（审核），具体看下面的链接：

{% page-ref page="../qi-ta/zhang-hao-guan-li.md" %}



