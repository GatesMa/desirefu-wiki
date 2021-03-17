# 机器部署

### 目前机器情况

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x5730;&#x5740;</th>
      <th style="text-align:left">IP</th>
      <th style="text-align:left">&#x914D;&#x7F6E;</th>
      <th style="text-align:left">&#x6570;&#x91CF;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>&#x767E;&#x5EA6;&#x4E91;CentOS</p>
      </td>
      <td style="text-align:left">&#x5E7F;&#x5DDE;</td>
      <td style="text-align:left">182.61.51.79&#x3001;106.13.202.72</td>
      <td style="text-align:left">
        <p></p>
        <p>2&#x6838;+4G+40G</p>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>2</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>&#x534E;&#x4E3A;&#x4E91;CentOS</p>
      </td>
      <td style="text-align:left">&#x5317;&#x4EAC;</td>
      <td style="text-align:left">119.3.184.74</td>
      <td style="text-align:left">
        <p></p>
        <p>2&#x6838;+4G+40G</p>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>&#x963F;&#x91CC;&#x4E91;CentOS</p>
      </td>
      <td style="text-align:left">&#x4E0A;&#x6D77;</td>
      <td style="text-align:left">47.102.121.0</td>
      <td style="text-align:left">
        <p></p>
        <p>1&#x6838;+2G+40G</p>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>&#x817E;&#x8BAF;&#x4E91;CentOS</p>
      </td>
      <td style="text-align:left">&#x9999;&#x6E2F;</td>
      <td style="text-align:left">43.128.45.220</td>
      <td style="text-align:left">
        <p></p>
        <p>1&#x6838;+1G+25G</p>
      </td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">&#x963F;&#x91CC;&#x4E91;Mysql 8.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">20G</td>
      <td style="text-align:left">1</td>
    </tr>
  </tbody>
</table>

直接参与服务部署的是百度和华为的三台机器，可以同时提供服务，阿里云的机器部署了nginx提供负载均衡，腾讯云的机器暂未参与服务支持。

### 后端服务

包名称：dfu\_service\_api-0.0.1.jar

安装路径：/usr/local/services/desire\_fu\_api\_jar-1.0/

服务依赖：java1.8

