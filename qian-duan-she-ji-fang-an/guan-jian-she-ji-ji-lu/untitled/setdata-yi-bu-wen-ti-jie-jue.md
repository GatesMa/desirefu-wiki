# setData异步问题解决

前端开发过程中的另一个坑，因为有时候我们的代码是需要在setData之后再执行的，但是微信的setData设置为异步的，导致结果和表现出来就有很大的问题，解决办法，利用回调：

{% hint style="info" %}
setData 函数用于将数据从逻辑层发送到视图层（异步），同时改变对应的 this.data 的值（同步）。
{% endhint %}

```text
this.setData({
  userInfo:{
    userName:"小山",
    userAge:11
  }
},()=>{
  //这里面的执行是异步的吗？
  console.log(222)
})
console.log(1111)


这里成功设置值后才会打印222
```

