# /desire\_fu/v1/login/can\_login\_account

 **/desire\_fu/v1/login/can\_login\_account（查询一个用户可以登录的账号）**

| **线程数量** | **\#Samples** | **Average** | **Median** | **90%Line** | **95%Line** | **99%Line** | **Min** | **Max** | **Error%** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 10 | 3000 | 247 | 377 | 542 | 738 | 1022 | 334 | 1289 | 0.00 |
| 20 | 4000 | 352 | 260 | 556 | 640 | 1283 | 198 | 16246 | 0.00 |
| 40 | 5000 | 653 | 919 | 3053 | 4682 | 11655 | 339 | 27665 | 0.00 |
| 80 | 8000 | 1462 | 968 | 3988 | 4565 | 10435 | 265 | 38234 | 0.98 |

可以看到在80的时候出现了异常请求，其实正常压测到60的时候已经少量的异常请求了。这应该已经达到了服务可以处理的极限了，因为我们只有三个2核4G的机器，并且nginx的机器是很低端的机器，后面会考虑优化。

接口QPS：45左右

![&#x8BF7;&#x6C42;&#x7684;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%28123%29.png)

![&#x6210;&#x529F;&#x7684;&#x8BF7;&#x6C42;&#x7684;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x767E;&#x5206;&#x6BD4;&#x5206;&#x5E03;&#x56FE;](../../.gitbook/assets/image%20%2890%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x541E;&#x5410;&#x7387;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2897%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x53D1;&#x9001;&#x4E00;&#x4E2A;&#x5B8C;&#x6574;&#x7684;&#x8BF7;&#x6C42;&#x6240;&#x9700;&#x65F6;&#x95F4;&#x7684;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%28114%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x4E8B;&#x52A1;&#xFF08;&#x8BF7;&#x6C42;&#xFF09;&#x5EFA;&#x7ACB;&#x8FDE;&#x63A5;&#x6240;&#x82B1;&#x8D39;&#x7684;&#x5E73;&#x5747;&#x65F6;&#x95F4;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%28124%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x54CD;&#x5E94;&#x72B6;&#x6001;&#x7801;&#x7684;&#x6570;&#x91CF;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2876%29.png)

![&#x6BCF;&#x79D2;&#x4E8B;&#x52A1;&#x6570;&#xFF0C;&#x5373; TPS](../../.gitbook/assets/image%20%2871%29.png)

![&#x5E73;&#x5747;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x548C;&#x7EBF;&#x7A0B;&#x6570;&#x7684;&#x5BF9;&#x5E94;&#x53D8;&#x5316;&#x66F2;&#x7EBF;](../../.gitbook/assets/image%20%2887%29.png)

![&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x5206;&#x5E03;&#x56FE;](../../.gitbook/assets/image%20%2899%29.png)





