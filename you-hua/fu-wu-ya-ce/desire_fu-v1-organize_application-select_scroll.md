# /desire\_fu/v1/organize\_application/select\_scroll

**/desire\_fu/v1/organize\_application/select\_scroll（查询申请）**

| **线程数量** | **\#Samples** | **Average** | **Median** | **90%Line** | **95%Line** | **99%Line** | **Min** | **Max** | **Error%** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 5 | 532 | 1142 | 1171 | 1275 | 1346 | 1561 | 842 | 1861 | 0.0 |

这个接口是最麻烦的一个接口，虽然只有一个查询，但是涉及非常多张表，效率很低，5的时候虽然没有出现错误请求，但是平均访问时长已经超过了1s，对用户的体验是极差的，已经达到了服务可以处理的极限了，前面已经说过，只有三个2核4G的机器，并且nginx的机器是很低端的机器，后面会考虑优化。

接口QPS：5**左右**

![&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x5206;&#x5E03;&#x56FE;](../../.gitbook/assets/image%20%28108%29.png)

![ TPS](../../.gitbook/assets/image%20%28109%29.png)







\*\*\*\*

