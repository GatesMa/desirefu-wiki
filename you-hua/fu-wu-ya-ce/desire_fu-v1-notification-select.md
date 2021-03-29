# /desire\_fu/v1/notification/select

**/desire\_fu/v1/notification/select（搜索公告）**

| **线程数量** | **\#Samples** | 平均 | **Error%** |
| :--- | :--- | :--- | :--- |
| 10 | 4000 | 408 | 0.00% |
| 20 | 4000 | 684 | 0.00% |
| 40 | 5000 | 1420 | 0.06% |

**40**的时候就已经出现了异常请求，已经达到了服务可以处理的极限了，前面已经说过，只有三个2核4G的机器，并且nginx的机器是很低端的机器，后面会考虑优化。

接口QPS：**30左右**

![&#x8BF7;&#x6C42;&#x7684;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2882%29.png)

![&#x541E;&#x5410;&#x7387;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2896%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x53D1;&#x9001;&#x4E00;&#x4E2A;&#x5B8C;&#x6574;&#x7684;&#x8BF7;&#x6C42;&#x6240;&#x9700;&#x65F6;&#x95F4;&#x7684;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2877%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x4E8B;&#x52A1;&#xFF08;&#x8BF7;&#x6C42;&#xFF09;&#x5EFA;&#x7ACB;&#x8FDE;&#x63A5;&#x6240;&#x82B1;&#x8D39;&#x7684;&#x5E73;&#x5747;&#x65F6;&#x95F4;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2886%29.png)

![&#x6BCF;&#x79D2;&#x4E8B;&#x52A1;&#x6570;&#xFF0C;&#x5373; TPS](../../.gitbook/assets/image%20%2895%29.png)

![&#x5E73;&#x5747;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x548C;&#x7EBF;&#x7A0B;&#x6570;&#x7684;&#x5BF9;&#x5E94;&#x53D8;&#x5316;&#x66F2;&#x7EBF;](../../.gitbook/assets/image%20%2870%29.png)

![&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x5206;&#x5E03;&#x56FE;](../../.gitbook/assets/image%20%2899%29.png)






