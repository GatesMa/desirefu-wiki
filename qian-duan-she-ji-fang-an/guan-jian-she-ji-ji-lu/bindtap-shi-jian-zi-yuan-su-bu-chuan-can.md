# bindtap事件子元素不传参

微信小程序开发中的一个坑：bindTap事件冒泡问题。事件中可以通过data- 来绑定一些值然后点击的时候再 e 里面获取，e为事件对象，可以携带额外信息，如 id, dataset, touches。e.target和e.currentTarget的区别。如果只绑定在图片或者按钮上获取得到的值是一模一样的。但是如果绑定在view中里面还存在子元素就会出现问题。

一个例子：

```javascript
<view class="guige" data-index="{{index}}" bindtap="guigeClick">
	<text class="color">选择规格</text>
	<text>{{item.listGuige.color[item.listGuige.cindex]}},{{item.listGuige.zhongliang[item.listGuige.zindex]}}，{{item.listGuige.size[item.listGuige.sindex]}}</text>
	<image src="/static/arrow.png" mode="heightFix" style="height:24rpx;"></image>
</view>


// 编辑规格
 guigeClick:function(e){
   let that = this;
   console.log("data-index值：",e.currentTarget.dataset.index)
   console.log("data-index值：",e.target.dataset.index)
   console.log("listGuige：",that.data.list)
   let list = that.data.list[e.target.dataset.index].listGuige;
	that.choiceGuige = that.selectComponent("#choiceGuige");
	that.choiceGuige.guige(list); //向组件中传值
	that.choiceGuige.showDialog();
},

```

当点击整个view中的空白处不会有问题，但是点击到任何子元素身上就会获取不到e.target.dataset.index，里面是空的。e.currentTarget.dataset.index则不会有问题。

**e.target指向的是添加监听事件的对象**，明显当点击子元素没有绑定该事件，故报错。**e.currentTarget 是指向触发事件监听的对象**，此时点击子元还是父元素，都是当前事件，应用e.currentTarget。 “添加（注册）监听事件”的对象与“触发事件监听”的对象是不一样的。前者指绑定了事件，如:bindtap、catchtap，后者是能够触发该事件但没有绑定事件。 把取值方式 由e.target.dataset.current修改为e.currentTarget.dataset.current即可

