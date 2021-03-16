# 文件上传res.data是string

前端开发过程中遇到的坑：文件上传接口后，得到的返回数据是string类型，与原来的网络接口请求不同，wxrequest的返回值是对象类型，可以直接获取值。测试返回的数据console.log\(res.data\)：

![](../../.gitbook/assets/image%20%2849%29.png)

开始用字典取值方法一直取不出来，后来发现res.data 是一个字符串，所以需要用JSON.parse\(\)将其转变成 json 对象,成功取到

![&#x5B57;&#x7B26;&#x4E32;](../../.gitbook/assets/image%20%2831%29.png)

```javascript
var data = res.data
console.log(data)
console.log(JSON.parse(data).img_link)
```



