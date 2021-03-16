# 比赛创建系统

**比赛创建系统**。可以为整个程序新增一个比赛类型，如果有其他想要参数到其中的人，可以进行发起组队，认领其他队友。

![](../../.gitbook/assets/image%20%2843%29.png)

页面头部展示账号总览信息，也提供了按钮筛选组，最主要的功能是创建比赛的编辑器，这个编辑器是富文本编辑器，支持插入图片、标题、时期、黑体等多样的样式。

![&#x7F16;&#x8F91;&#x5668;](../../.gitbook/assets/image%20%2848%29.png)

图片插入时有一个细节，就是先请求后端的接口，将图片上传之后，获得网络链接再展示到编辑器中，以免本来插入的是一个图片的临时链接，到时候编辑完成之后我们再把这些临时链接转化为网络链接的时候就会很麻烦，当然这样也有缺陷，比如有人插入了然后又删除了，那么上传的图片可能就没用了，会浪费一些资源。

```javascript
//插入图片
insertImageEvent() {
  wx.chooseImage({
    count: 1,
    success: res => {
      // 上传获取url
      common.uploadFile(res.tempFilePaths[0], 'IMG').then((url) => {
        //调用子组件方法，图片应先上传再插入，不然预览时无法查看图片。
        richText.insertImageMethod(url).then(res => {
          console.log('[insert image success callback]=>', res)
        }).catch(res => {
          console.log('[insert image fail callback]=>', res)
        });
      }).catch(() => {
        wx.showToast({
          title: '出现问题',
          icon: 'none',
          duration: 1000
        })
      })

    }
  })
},
```

