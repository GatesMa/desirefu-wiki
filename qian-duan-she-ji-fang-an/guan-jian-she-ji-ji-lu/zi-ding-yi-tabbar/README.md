# 自定义tabbar

我们的小程序应用里，有5个系统，如果只使用微信提供的原声tabbar的话，肯定是不够用的，而且自定义程度很低，所以我使用里微信官方推荐的自定义tabbar，使用里Component组件来构建页面。[自定义tabbar](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/custom-tabbar.html)。

* 在 `app.json` 中的 `tabBar` 项指定 `custom` 字段，同时其余 `tabBar` 相关配置也补充完整。
* 所有 tab 页的 json 里需声明 `usingComponents` 项，也可以在 `app.json` 全局开启。

利用currentTab来切换到底显示哪一个界面

```javascript
.wxml
<view hidden="{{currentTab == 0? false: true}}">
  <component_homepage />
</view>
<view hidden="{{currentTab == 1? false: true}}">
  <component_search />
</view>
<view hidden="{{currentTab == 2? false: true}}">
  <component_my />
</view>

.json
{
  "usingComponents": {
    "component_search": "/pages/normal/component/search/search",
    "component_homepage": "/pages/normal/component/homepage/homepage",
    "component_my": "/pages/normal/component/my/my"
  }
}
```

实现效果理论上可以同时多个tab页面，而且不同系统都可以进行自定义tabbar。

![&#x793A;&#x4F8B;](../../../.gitbook/assets/image%20%2827%29.png)



