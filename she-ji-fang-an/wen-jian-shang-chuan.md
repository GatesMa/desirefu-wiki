# 文件上传

文件上传是比较复杂的一个部分，因为小程序里需要大量用到图片上传的地方，刚开始的想法是自己大家一个静态服务器，重新写一个服务，提供接口将上传的文件保存到静态文件目录，提供url，但是我的域名绑定的服务器是初级学生服务器，不能够支持大量的文件上传，并且网络带宽页不够，也不能支持同时大量的网络请求，所以最后权衡之下，选择里购买腾讯云的对象存储服务（COS），而且按量付费，费用也不是很贵（目前阶段来说，费用不超过一个月一块钱），非常适合我的小程序。

### （1）腾讯云COS对象存储

对象存储（Cloud Object Storage，COS）是腾讯云提供的一种存储海量文件的分布式存储服务，具有高扩展性、低成本、可靠安全等优点。通过控制台、API、SDK 和工具等多样化方式，用户可简单、快速地接入 COS，进行多格式文件的上传、下载和管理，实现海量数据存储和管理。

使用外包的对象存储，可以很方便的上传下载文件，不需要自己取搭建服务，省去了很多时间，并且可以保证访问速度，是一个不错的选择。

在Spring容器中我们只需要注册一个cosClient就可以实现文件的上传，上传后的文件具有固定的url：

```java
CosProperties cosProperties = cosProperties();
// 1 初始化用户身份信息（secretId, secretKey）。
String secretId = cosProperties.getSecretId();
String secretKey = cosProperties.getSecretKey();
COSCredentials cred = new BasicCOSCredentials(secretId, secretKey);
// 2 设置 bucket 的区域, COS 地域的简称请参照 https://cloud.tencent.com/document/product/436/6224
// clientConfig 中包含了设置 region, https(默认 http), 超时, 代理等 set 方法, 使用可参见源码或者常见问题 Java SDK 部分。
Region region = new Region(cosProperties.getRegion());
ClientConfig clientConfig = new ClientConfig(region);
// 3 生成 cos 客户端。
return new COSClient(cred, clientConfig);
```

### （2）文件上传数据表

每一个上传的文件，我希望知道是谁上传的，之后也可以做数据统计，所以为文件上传也做了一个数据库表，`UploadFile`：

```sql
CREATE TABLE IF NOT EXISTS `UploadFile_` (
  `fileId` int(11) NOT NULL AUTO_INCREMENT COMMENT '文件ID',
  `fileType` varchar(32) default null COMMENT '文件类型',
  `accountId` bigint(20) default null COMMENT '比赛归属帐号ID',
  `accountType` int(11) default null COMMENT '比赛归属账号类型',
  `fileName` varchar(255) default null COMMENT '文件名称',
  `fileURL` varchar(1024) default null COMMENT '文件Url',
  `deleteStatus` int(11) NOT NULL COMMENT '删除状态，0-正常，1-删除',
  `createdTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `lastModifiedTime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'lastModifiedTime',
  PRIMARY KEY (`fileId`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT '文件上传表';
```

提供的接口中，如果有文件上传，就传入一条记录，标识文件来源。为防止文件名重复，我们使用了UUID  
：UUID全称通用唯一识别码（universally unique identifier），JDK通过`java.util.UUID`提供了 Leach-Salz 变体的封装。在Hutool中，生成一个UUID字符串方法如下：（这里我们用到了一个其他人的包Hutool，一个非常好用的工具依赖）

```java
//生成的UUID是带-的字符串，类似于：a5c8a5e8-df2b-4706-bea4-08d0939410e3
String uuid = IdUtil.randomUUID();

//生成的是不带-的字符串，类似于：b17f24ff026d40949c85a24f4f375d42
String simpleUUID = IdUtil.simpleUUID();
```

{% hint style="info" %}
说明 Hutool重写`java.util.UUID`的逻辑，对应类为`cn.hutool.core.lang.UUID`，使生成不带-的UUID字符串不再需要做字符替换，性能提升一倍左右。
{% endhint %}

### （3）文件类型验证

因为要插入数据库表，我们不能需要验证文件的类型，否则可能会有问题，比如我们需要用户上传一个图片文件，但是用户上传了其他类型的文件，那么在显示头像的适合就会出现问题，所以后端对上传的文件需要做一个验证，并给出错误提示，小程序目前允许上传的文件类型有：`PDF、 IMG 、PPT;`

这里我们主要是对IMG图片类型的验证有点难度，因为图片有img后缀、png、jeg等等，我们无法直接通过后缀来识别文件，所以想到了其他的方法：

IMG：对于图片来说，利用了ImageReader这个工具类，如果可以生成ImageReader，表示是一个图片。

```java
ImageInputStream imgInputStream = ImageIO.createImageInputStream(new ByteArrayInputStream(content));
Iterator<ImageReader> imgReaders = ImageIO.getImageReaders(imgInputStream);
return imgReaders != null && imgReaders.hasNext();
```

PDF和PPT格式的验证：利用了这种文件格式，前面的固定编码，有缺陷，但是足够用了。

```java
// PDF
return content != null && content.length >= 4 && content[0] == 0x25 && content[1] == 0x50 && content[2] == 0x44 && content[3] == 0x46;
// PPT
boolean isPptFormat = content.length >= 8 && content[0] == 0XD0 && content[1] == 0XCF && content[2] == 0X11 && content[3] == 0XE0
    && content[4] == 0XA1 && content[5] == 0XB1 && content[6] == 0X1A && content[7] == 0XE1;
boolean isPptxFormat = content.length >= 4 && content[0] == 0x50 && content[1] == 0x4B && content[2] == 0x03 && content[3] == 0x04;
return isPptFormat || isPptxFormat;
```

###  （4）文件上传接口

接口地址：`/desire_fu/v1/upload/upload_file`

![&#x63A5;&#x53E3;&#x8BE6;&#x7EC6;](../.gitbook/assets/image%20%2842%29.png)

### （5）微信小程序公共方法

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

}
```

