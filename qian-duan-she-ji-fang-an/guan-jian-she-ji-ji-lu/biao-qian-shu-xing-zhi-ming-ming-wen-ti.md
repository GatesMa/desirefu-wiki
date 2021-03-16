# 标签属性值命名问题

记录微信小程序开发的一个坑，标签中属性的属性值问题。在微信开发中我们经常会用到标签中属性的属性值，[数据绑定](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/data.html)，有时候我们通过 data-\* 和 e.target.dateset 来获取属性值会出现一点小bug，即是调用出来的数据是undefined。

```javascript
<--HTML写法：-->
<button binTap="buy" data-textId="101"></button>

//JS调用:
buy:function(e){
    console.log(e.target.dataset.textId);
    //输出结果：undefined
}
```

原因是data后面的属性名写得不规范，在data后面的属性名是不能按照驼峰式的写法，只要把定义的**属性名全部换成小写**就没有问题了，如下：

```javascript
<--HTML写法：-->
<button binTap="buy" data-productid="101"></button>
//JS调用:
buy:function(e){
    console.log(e.target.dataset.productid);
    //输出结果：101
}
```

