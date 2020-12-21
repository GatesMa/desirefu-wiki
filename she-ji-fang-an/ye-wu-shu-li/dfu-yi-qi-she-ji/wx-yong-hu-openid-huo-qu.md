---
description: 获取wx用户的唯一标识appId
---

# WX用户openId获取

微信没有办法直接获取用户的微信号，最多只能授权获取手机号，但是手机号其实也不能作为微信号的唯一凭证，所以需要调用微信的接口获取微信的唯一凭证openId



### 1. 第一步，获取code

调用wx.login\(OBJECT\) 获取**登录凭证（code）**进而换取用户登录态信息，包括用户的**唯一标识（openid）** 及本次登录的 **会话密钥（session\_key）**等。**用户数据的加解密通讯**需要依赖会话密钥完成。

[接口地址](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)

![&#x63A5;&#x53E3;&#x56DE;&#x8C03;](../../../.gitbook/assets/image%20%2814%29.png)

![&#x6210;&#x529F;&#x56DE;&#x8C03;](../../../.gitbook/assets/image%20%2811%29.png)

**注：调用 login 会引起登录态的刷新，之前的 sessionKey 可能会失效**

示例代码：

```javascript
//app.js
App({
  onLaunch: function() {
    wx.login({
      success: function(res) {
        if (res.code) {
          //发起网络请求
          wx.request({
            url: 'https://test.com/onLogin',
            data: {
              code: res.code
            }
          })
        } else {
          console.log('获取用户登录态失败！' + res.errMsg)
        }
      }
    });
  }
})
```

### 2. 根据code获取openid和session\_key

这是一个 HTTPS 接口，开发者服务器使用**登录凭证 code** 获取 session\_key 和 openid。

[接口地址](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/login/auth.code2Session.html)

![&#x8BF7;&#x6C42;&#x53C2;&#x6570;](../../../.gitbook/assets/image%20%2810%29.png)

![&#x8FD4;&#x56DE;&#x503C;](../../../.gitbook/assets/image%20%2815%29.png)

![&#x9519;&#x8BEF;&#x7801;](../../../.gitbook/assets/image%20%2812%29.png)

示例请求代码：

```javascript
//根据code获取openid等信息
wx.login({
        //获取code
        success: function (res) {
          var code = res.code; //返回code
          console.log(code);
          var appId = '...';
          var secret = '...';
          wx.request({
            url: 'https://api.weixin.qq.com/sns/jscode2session?appid=' + appId + '&secret=' + secret + '&js_code=' + code + '&grant_type=authorization_code',
            data: {},
            header: {
              'content-type': 'json'
            },
            success: function (res) {
              var openid = res.data.openid //返回openid
              console.log('openid为' + openid);
            }
          })
        }
      })

//正常返回的JSON数据包
{
      "openid": "OPENID",
      "session_key": "SESSIONKEY",
      "unionid": "UNIONID"
}
//错误时返回JSON数据包(示例为Code无效)
{
    "errcode": 40029,
    "errmsg": "invalid code"
}
```

### 3. 将请求接口放在服务后端

#### **1. 问题1**

**注意：** 按照上面的方式，如果你未勾选项目中的“开发环境不效验请求域名、TLS版本以及HTTPS证书”则会报错。开发环境到设置-&gt;项目设置-&gt;勾选：开发环境不校验请求域名、TLS版本以及HTTPS证书。

但是正式环境，会发现报错：不在一下合法域名列表，请参考文档：  
[https://map.weixin.qq.com/debug/wxadoc/dev/api/network-request.html](https://map.weixin.qq.com/debug/wxadoc/dev/api/network-request.html)  
意思是说：[https://api.weixin.qq.com/](https://api.weixin.qq.com/sns/jscode2session?appid=APPID&secret=SECRET&js_code=JSCODE&grant_type=authorization_code) 不是一个合法域名，在测试的时候是没有问题的，但是要上线的时候就出现了问题

#### 2. 问题2

如果在配置服务器域名中填写了“api.weixin.qq.com”会出现上述错误提示。出于安全考虑，为避免开发者将AppSecret放置在小程序的前端代码内，平台禁止设置此域名。 小程序的开发者密码（AppSecret）是一个非常重要的字段，使用该密码可以调用小程序的所有后台接口。请不要将该字段放置在微信小程序的前端代码中，因为微信手机客户端容易被反编译并轻松获得Appsecret，造成重大的安全威胁。开发者应将Appsecret保存到后台服务器中，通过服务器使用Appsecert获取Accesstoken。微信公众平台小程序后台的服务器地址设置也将禁止将“api.weixin.qq.com”域名的配置，**所有对于“api.weixin.qq.com”域名下的接口请求请全部通过后台服务器发起，请勿直接通过小程序的前端代码发起。**

#### **3. 解决方法**

**把code传给后台，让后台去请求微信的官方接口获得openId和session-key。**

{% api-method method="get" host="/external/code\_2\_wx\_openid" path="" %}
{% api-method-summary %}
Code转OpenId接口
{% endapi-method-summary %}

{% api-method-description %}
通过code，调用微信接口获取微信账号的openId
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="code" type="string" %}
登录时获取的 code
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "code": "...",
    "openId": "..."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}







