# 文件上传工具

写了一个工具类，包含一个文件上传方法，以免每次需要上传文件的时候都要写一次新的代码：

{% code title="common.js" %}
```javascript
function uploadFile(path, type) {

  return new Promise((resolve, reject) => {
    console.log('uploadFile')
    wx.uploadFile({
      url: app.globalData.baseUrl + '/desire_fu/v1/upload/upload_file',
      filePath: path,
      name: 'file',
      header: {
        'Accept': 'application/json'
      },
      formData: {
        'account_id': 1,
        'account_type': 1,
        'file_type': type
      },
      success (res){
        // console.log(JSON.parse(res.data).data.file_url)
        //do something
        resolve(JSON.parse(res.data).data.file_url);
      },
      fail: err => {
        reject(err)
      }
    })
  })
```
{% endcode %}

方法调用，引入common.js，调用方法，还可以捕获异常：

```javascript
var common = require("../../../common.js")
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
```

