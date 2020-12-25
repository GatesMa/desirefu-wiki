# openId本地缓存

微信的openId获取接口很慢，而且会限制1分钟只能请求100次，所以需要对openId做本地缓存没，这样，只有第一次访问小程序或者更换了手机的用户才会去请求微信的接口获取openId。

```text
GET https://api.weixin.qq.com/sns/jscode2session?appid=APPID&secret=SECRET&js_code=JSCODE&grant_type=authorization_code
```

> 微信的接口的返回时长**3-7s**不等

设置缓存，如果正确请求openId，把openId存储到本地：

```javascript
// 本地存储，以免每次进入都需要很长的加载时间，微信的接口很慢
wx.setStorage({
  key: 'openId',
  data: openId
})
```

下一次登陆或访问小程序先从本地去拿，看是否能命中缓存

```javascript
// 先尝试本地缓存中获取
wx.getStorage({
  key: 'openId',
  success: function(res) {
    that.globalData.openId = res.data;
    console.log('cache find openId:' + that.globalData.openId)
    return
  }  
})
```

