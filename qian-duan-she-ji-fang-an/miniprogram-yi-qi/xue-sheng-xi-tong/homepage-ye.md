# HomePage页

### （1）效果预览

![](../../../.gitbook/assets/image%20%2830%29.png)

### （2）设计

学生系统的首页，上方是一个轮播图，用于展示热门的比赛，这些图片也是可以点击的，点击后会跳转到具体的比赛通知页面：

```javascript
navigateToNotice: function (e) {
  var item = e.currentTarget.dataset.item
  var competitionId = item.content // 比赛的id放在content里了
  // 跳转
  wx.navigateTo({
    url: '/pages/normal/preview/preview?isPre=false&hasId=true&competitionId=' + competitionId
  })
},
```

轮播图下面是一个筛选的按钮组，可以用于对具体的比赛进行条件筛选，筛选条件有状态（已结束、进行中、未开始）、等级、名称、举办方，以及各种排序规则，满足用户的需要，同时比赛列表请求默认一页五条数据，页面下方提供两个按钮用于翻页。





