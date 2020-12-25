# 首页账号列表

{% hint style="info" %}
首页展示来当前微信号可以登陆的账号类型以及该类型下可以登陆的账号，这些都是从后端接口获得，包括账号状态等信息
{% endhint %}

### 1. 页面概览

展示了目前登陆微信号可以登陆的账号列表，包含账号名称、账号ID、微信号对于该账号的角色信息、账号状态（未审核、审核不通过、审核通过、冻结）。对于第一次登陆的微信号，同时增加新增普通账号的接口，后续新增的账号会交由学校负责人审核，审核完成的账号即可正常登陆。

![&#x5C0F;&#x7A0B;&#x5E8F;&#x9996;&#x9875;](../../.gitbook/assets/image%20%2818%29.png)

### 2. 获得微信账号唯一凭证openId

微信没有办法直接获取用户的微信号，最多只能授权获取手机号，但是手机号其实也不能作为微信号的唯一凭证，所以需要调用微信的接口获取微信的唯一凭证openId，具体的获取方法见：（调用微信提供的接口获取）

{% page-ref page="../../she-ji-fang-an/dfu-yi-qi-she-ji/wx-yong-hu-openid-huo-qu.md" %}

### 3. 获取可登陆账号列表流程

* 1. 获取userInfo、openId、userId

获取userInfo，包含微信账号的昵称、头像链接等信息。后面注册学生账号的时候，nickName就是获取的微信的昵称，没有让用户自己输入。（这里采用了Promise解决异步处理问题）

```javascript
// getUser，通过电话，名称等信息，获取user
  getUserInfo() {
    return new Promise((resolve, reject) => {
      wx.getSetting({
        success: res => {
          if (res.authSetting['scope.userInfo']) {
            // 1. 已经授权，可以直接调用 getUserInfo 获取头像昵称，不会弹框
            wx.getUserInfo({
              success: res => {
                // 可以将 res 发送给后台解码出 unionId
                this.globalData.userInfo = res.userInfo
                // 由于 getUserInfo 是网络请求，可能会在 Page.onLoad 之后才返回
                // 所以此处加入 callback 以防止这种情况
                if (this.userInfoReadyCallback) {
                  this.userInfoReadyCallback(res)
                }
                resolve(res);
              }
            })
          }
        },
        fail: (err) => {
          reject(err);
        }
      })
    })
  }
```

* 2. 获取openId,等待app.js回调完成

这里获取openId，先调用自己服务器的后端接口，再调用微信提供的接口，后端接口地址：

> /desire\_fu/v1/external/code\_2\_wx\_openid

```javascript
// 登录，获取openId，换成userId
wxLogin() {
  return new Promise((resolve, reject) => {
    wx.login({
      success: res => {
        // 发送 res.code 到后台换取 openId, sessionKey, unionId
        var code = res.code; //返回code
        wx.request({
          url: this.globalData.baseUrl + '/desire_fu/v1/external/code_2_wx_openid',
          data: {
            code: code
          },
          method: "POST",
          header: {
            'content-type': 'application/json',
            'Accept': 'application/json'
          },
          success: (res) => {
            var openId = res.data.data.open_id //返回openid
            this.globalData.openId = openId;
            resolve(res);
          }
        })
      },
      fail: (err) => {
        reject(err);
      }
    })
  })
}
```

* 3. 获取userId

以为只拿到openId的话，也无法确认该微信号已经注册过，解决办法是调用后端的创建user接口，这个接口目前是幂等的，也就是同一个微信号可以多次创建，返回一个userId。创建user接口地址：

> /desire\_fu/v1/user/add

```javascript
// getUser，通过openId，获取userId
getUserId(openId) {
  return new Promise((resolve, reject) => {
    // 用openId和userInfo信息创建一个user，这个接口是幂等的，如果存在则不会创建
    // 目前手机号、邮箱这些还没有办法获取到
    wx.request({
      url: this.globalData.baseUrl + '/desire_fu/v1/user/add',
      data: {
        cellphone: '',
        created_user_id: 1,
        email: '',
        login_name: openId,
        login_name_type: 2,
        user_name: this.globalData.userInfo.nickName
      },
      method: "POST",
      header: {
        'content-type': 'application/json',
        'Accept': 'application/json'
      },
      success: (res) => {
        var userId = res.data.data.user_id //返回userId
        this.globalData.userId = userId;
        resolve(res);
      },
      fail: (err) => {
        reject(err);
      }
    })
  })
}
```

* 4. 通过userId获取可以登陆的账号

最后一步就是通过userId获取该user绑定的账号信息，并返回成为列表，供前端展示，也是调用的后端接口，后端接口地址：

> /desire\_fu/v1/login/can\_login\_account

```javascript
// 4. 通过userId获取可以登陆的账号
wx.request({
  url: app.globalData.baseUrl + '/desire_fu/v1/login/can_login_account',
  data: {
    user_id: app.globalData.userId
  },
  method: "POST",
  header: {
    'content-type': 'application/json',
    'Accept': 'application/json'
  },
  success: (res) => {
    console.log('5:'  + new Date())
    var canLoginAccountData = res.data.data
    this.setData({
      canLoginAccountData: canLoginAccountData
    })
    // console.log('canLoginAccountData:' + canLoginAccountData)
  },
  fail: (err) => {
    wx.showToast({
      title: '获取数据异常',
      icon: 'error',
      duration: 2000
    })
  },
  complete: () => {
    this.setData({
      loadModal: false
    })
  }
})
```

{% hint style="info" %}
账号列表页后端接口返回格式如
{% endhint %}

```javascript
{
  "code": 0,
  "message": "OK",
  "data": [
    {
      "account_list": [
        {
          "account_id": 11,
          "account_type": 1,
          "account_name": "爆龙战士",
          "role": 1,
          "role_name": "开户账号",
          "account_status": 1,
          "account_status_str": "有效"
        }
      ],
      "platform_type": 1,
      "platform": "组队账号"
    }
  ]
}
```

### 4. 学生账号补充信息界面

![&#x8D26;&#x53F7;&#x7533;&#x8BF7;&#x9875;&#x9762;](../../.gitbook/assets/image%20%2816%29.png)

![&#x8865;&#x5145;&#x5B66;&#x751F;&#x8EAB;&#x4EFD;&#x4FE1;&#x606F;&#x9875;&#x9762;](../../.gitbook/assets/image%20%2817%29.png)





