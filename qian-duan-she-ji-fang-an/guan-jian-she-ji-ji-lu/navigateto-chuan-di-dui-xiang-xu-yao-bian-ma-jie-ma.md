# navigateTo传递对象，需要编码解码

navigateTo可以在页面间传递参数，但是如果传递的对象里有特殊字符，就会被截断（参数里面含有,逗号一类的符号会把参数切断），导致出错。**原因：**参数被url截断了 、需要编码传送，解码接收

```javascript
//传参
wx.navigateTo({//wx.redirectTo、wx.reLaunch
    url: '../details/details?id=' + encodeURIComponent(id)
});
```

```javascript
接收
onLoad(options) {
    var id = decodeURIComponent(options.id);
}
```

