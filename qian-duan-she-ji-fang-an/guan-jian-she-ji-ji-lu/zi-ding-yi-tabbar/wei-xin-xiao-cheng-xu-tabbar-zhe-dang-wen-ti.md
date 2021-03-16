# 微信小程序tabbar遮挡问题

因为自定义的tabbar，而tabbar是样式果固定在尾部的，会遮挡页面的一部分内容，例如，设置了页面有0-99共100个数，但是最后几个数字被底部固定的按钮遮挡住了，显示不全：

![](../../../.gitbook/assets/image%20%2828%29.png)

解决办法其实很简单，就是在页面底部增加一个空白的view：

```markup
<!-- 防止tabbar遮挡 -->
<view style="padding-top: {{tabbarHeight}}px"></view>
```

那么tabbarHeigt如何获得呢，因为这里我们是不能直接固定这个高度的，不同型号的手机，tabbar的高度不一样，tabbarHeight的值需要动态获取，利用了wx.getSystemInfo这个原生方法获取系统信息：

```javascript
// CustomBar就是tabbarHeight的值
wx.getSystemInfo({
  success: e => {
    this.globalData.StatusBar = e.statusBarHeight;
    let capsule = wx.getMenuButtonBoundingClientRect();
    if (capsule) {
      this.globalData.Custom = capsule;
      this.globalData.CustomBar = capsule.bottom + capsule.top - e.statusBarHeight;
    } else {
      this.globalData.CustomBar = e.statusBarHeight + 50;
    }

    // 计算高度，换算为rpx
    let clientHeight = e.windowHeight,
      clientWidth = e.windowWidth,
      rpxR = 750 / clientWidth;
    var calc = clientHeight * rpxR;
    console.log(calc)
    this.globalData.windowHeight = calc;
  }
})
```







