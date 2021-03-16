# 微信小程序异步请求问题

微信小程序为了提高用户体验，提供的api大部分都是异步操作，除了数据缓存操作里面有一些同步操作。是提高了用户体验，但是在开发的时候，给我们带来很大的问题，经常拿不到数据。

例如在app.js里请求用户数据，发起网络请求，去后台去一些数据，成功之后，再做一些操作，但是由于wx.request是异步请求，就会导致进入到其他页面了，这个数据还没处理完，网络请求还没结束，就会执行后面的代码，从而引起异常，经常会出现undefined。微信小程序是通过wx.request进行异步处理，可以使用**promise**稍加改造。

举例：

```javascript
// 生命周期函数onload用于监听页面加载

onload: function () {
    wx.request({
        url: https://api,
        // 某个api接口地址
        success: res => {
            console.log(res.data)
            this.setData({
                myData: res.data
            })
        console.log(this.data.myData)
        }
    })
    // 调用之前的函数
    this.loadMyData()
}
```

wx.request是异步请求，JS不会等待wx.request执行完毕再往下执行，所以JS按顺序会先执行this.loadMyData\(\)，等服务器返回数据以后，loadMyData\(\)早就执行完了，也就会出现没有拿到值啦。

#### 初步解决：通过回调接收结果

最简单的解决方案，就是把需要使用异步数据的函数写在回调里：

```javascript
onload: function () {
    wx.request({
        url: https://api,
        // 某个api接口地址
        success: res => {
            console.log(res.data)
            this.setData({
            myData: res.data
        })
        console.log(this.data.myData)
            // 把使用数据的函数写在回调函数success中
            this.loadMyData()
        }
    })
}
```

#### 使用Promise包装wx.request

Promise可以将异步的执行逻辑和结果处理分离，摒弃了一层又一层的回调嵌套，使得处理逻辑更加清晰。例如登陆：

```javascript
//登录
login:function(){
  var that = this;
  // Promise
  return new Promise(function(resolve, reject){
    wx.login({
      success: res => {
        // 发送 res.code 到后台换取 openId, sessionKey, unionId
        console.log(res.code)
        //发送请求
        wx.request({
          url: that.globalData.severUrl, //接口地址
          data: {code:res.code},
          method:'GET',
          header: {
            'content-type': 'application/json' //默认值
          },
          success: function (res) {
            resolve('success')
            //后面的操作
          },
          fail:function (res) {
            reject('error')
          }
        })
      }
    })
  })
}

// 调用的地方
app.login().then(function(resolve, reject){
//resolve, reject可以用来判断，然后执行接下来的操作
})
```

结果和使用回调函数一致。当有多个异步请求时，直接不断地**.then**去处理即可，逻辑更加清晰。

