# /desire\_fu/v1/normal\_account/get

**/desire\_fu/v1/login/can\_login\_account（查询一个用户可以登录的账号）**

| **线程数量** | **\#Samples** | 平均 | **Error%** |
| :--- | :--- | :--- | :--- |
| 10 | 4000 | 450 | 0.00% |
| 20 | 4000 | 690 | 0.00% |
| 40 | 5000 | 1401 | 0.05% |

可以看到在40的时候就已经出现了异常请求，已经达到了服务可以处理的极限了，前面已经说过，只有三个2核4G的机器，并且nginx的机器是很低端的机器，后面会考虑优化。

接口QPS：**30左右**

![&#x8BF7;&#x6C42;&#x7684;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2867%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x6210;&#x529F;&#x7684;&#x8BF7;&#x6C42;&#x7684;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x767E;&#x5206;&#x6BD4;&#x5206;&#x5E03;&#x56FE;](../../.gitbook/assets/image%20%2877%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x541E;&#x5410;&#x7387;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2893%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x53D1;&#x9001;&#x4E00;&#x4E2A;&#x5B8C;&#x6574;&#x7684;&#x8BF7;&#x6C42;&#x6240;&#x9700;&#x65F6;&#x95F4;&#x7684;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2876%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x4E8B;&#x52A1;&#xFF08;&#x8BF7;&#x6C42;&#xFF09;&#x5EFA;&#x7ACB;&#x8FDE;&#x63A5;&#x6240;&#x82B1;&#x8D39;&#x7684;&#x5E73;&#x5747;&#x65F6;&#x95F4;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2875%29.png)

![&#x811A;&#x672C;&#x8FD0;&#x884C;&#x671F;&#x95F4;&#xFF0C;&#x54CD;&#x5E94;&#x72B6;&#x6001;&#x7801;&#x7684;&#x6570;&#x91CF;&#x53D8;&#x5316;&#x8D8B;&#x52BF;&#x56FE;](../../.gitbook/assets/image%20%2873%29.png)

![&#x6BCF;&#x79D2;&#x4E8B;&#x52A1;&#x6570;&#xFF0C;&#x5373; TPS](../../.gitbook/assets/image%20%2892%29.png)

![&#x5E73;&#x5747;&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x548C;&#x7EBF;&#x7A0B;&#x6570;&#x7684;&#x5BF9;&#x5E94;&#x53D8;&#x5316;&#x66F2;&#x7EBF;](../../.gitbook/assets/image%20%2894%29.png)

![&#x54CD;&#x5E94;&#x65F6;&#x95F4;&#x5206;&#x5E03;&#x56FE;](../../.gitbook/assets/image%20%2870%29.png)

