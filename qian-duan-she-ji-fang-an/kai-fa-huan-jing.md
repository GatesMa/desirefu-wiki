# 开发环境

微信小程序也有开发环境，一般本地开发的时候使用的后端接口是本地的，一般是localhost，但是在线上的接口一般都是别的ip，所以怎么区分。解决办法是微信端可以用envVersion来区分环境，我们根据这个环境选择合适的后端接口地址即可，看代码：

{% code title="app.js" %}
```javascript
// 获取后端接口地址
getBaseUrl: function () {
  // 获取当前帐号信息
  var accountInfo = wx.getAccountInfoSync();
  // env类型
  var env = accountInfo.miniProgram.envVersion;
  this.globalData.env = env;
  if (!env) {
    console.error("获取运行环境失败!");
  }
  console.log('当前环境：' + env)
  var baseApi = {
    // 开发版
    develop: "https://xxxxx:81", // http://192.168.124.3:8088
    // 体验版
    trial: "https://xxxx:81",
    // 正式版
    release: "https://xxxx:81" // 正式版本
  };

  // request请求baseURL
  return baseApi[env];
},
```
{% endcode %}

然后我们把getBaseUrl获取到的地址保存到全局变量，调用的时候，我们只需要补充接口的地址后部分就行：

```javascript
var baseUrl = this.getBaseUrl()
this.globalData.baseUrl = baseUrl;
```

