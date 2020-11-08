---
description: 接口文档，
---

# API DOC 文档建设

### 1. Swagger

接口文档采用SpringBoot Swagger框架实现，可以很方便生成一个后端服务的接口文档。

[Swagger官方文档](https://swagger.io/)、[Swagger在线编辑生成文档页面](https://editor.swagger.io/)

### 2. Swagger配置文件

一个json配置文件，指定了生成的类存放的位置等信息

{% code title="swagger\_generate\_config.json" %}
```javascript
{
  "useTags":true,
  "serializableModel":true,
  "hideGenerationTimestamp":true,
  "fullJavaUtil":true,
  "apiPackage":"cn.gatesma.desirefu.controller.api.generate",
  "modelPackage":"cn.gatesma.desirefu.domain.api.generate",
  "configPackage":"cn.gatesma.desirefu.config.api.generate"
}
```
{% endcode %}

下面这个图是配置文件存放的位置，swagger\_generate\_config.json是总配置文件，swagger.yaml是接口的详细配置文件，使用yaml格式：

![swagger&#x914D;&#x7F6E;&#x6587;&#x4EF6;&#x4F4D;&#x7F6E;](../.gitbook/assets/image%20%287%29.png)

Swagger.yaml的配置案例可以看这里：[https://editor.swagger.io/](https://editor.swagger.io/)

### 3. Task生成

写了一个gradle的task任务，可以利用swagger的工具自动生成swagger的代码：

```java
// swagger代码生成task
task swaggerCodeGen(type:Exec,dependsOn:copyRuntimeLibs) {
    String jar='build/libs/lib/swagger-codegen-cli-3.0.8.jar'
    String swagger='./src/main/resources/swagger.yaml'
    String config='./src/main/resources/swagger_generate_config.json'
    String output='./build/output/swagger/'
    commandLine 'java','-jar',jar,'generate','-i',swagger,'-l','spring','-o',output,'-c',config
}
```

执行gradle命令：`gradle swaggerCodeGen`

生成好的代码在`build/output/swagger`目录下，可以直接拷贝到自己的java源目录下即可使用：

![&#x81EA;&#x52A8;&#x751F;&#x6210;&#x7684;&#x4EE3;&#x7801;&#x4F4D;&#x7F6E;](../.gitbook/assets/image%20%289%29.png)

### 4. 查看结果

写一个springboot的启动类，启动springboot即可，访问自己设置的根路径：

![](../.gitbook/assets/image%20%283%29.png)



{% hint style="info" %}
接口文档对于开发来说还是很关键的。无论是前端调用后端，还是后端调用其他后端服务，都期望有一个好的接口文档。但是这个接口文档对于程序员来说，就跟注释一样，经常会抱怨别人写的代码没有写注释，然而自己写起代码最讨厌的可能也是写注释。所以使用接口文档框架再合适不过，快捷生成简单易懂的文档页面。
{% endhint %}

