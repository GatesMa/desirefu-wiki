---
description: DesireFU校园组队招募小程序前后端开发概览wiki
---

# DesireFU开发建设文档

> Author：GatesMa

## 项目概览

近年来高校学生参加各种各样的竞赛越来越多、越来越频繁，互联网+、大创...，校级、省级、国家级...层出不穷。对于比赛举办方来说，每一场比赛都需要维护一个学生组队的系统，维护学生数据等，十分繁琐，辅导员等想查看学院报名情况也很麻烦，如果要举办一个新类型的比赛，前期准备时间可能非常长。

本项目是一个主要定位在“服务于比赛组队“的小程序，以解决高校举行大型比赛比如大创、互联网+等时，在各大群发布招募需求时，消息容易被埋没，信息不具备时效性，而且往往具有不完整等弊端，导致选手掌握信息不全，影响参赛、招募者难以在人海中找到本项目需要的神级队友等问题，同时利用所学知识将该程序变为企业级高性能、高健壮性、高可用的小程序。

主要工作可以分为两部分：**1. 完整的功能**。在不考虑效率的情况下可以保证正常使用应用完成比赛相关的操作。完成程序的功能完整性，保证可以正常使用该小程序完成比赛管理，学生、比赛管理、运营OSS、ROOT、队伍要能够正常投入使用，完成代码编写、服务部署、机器购买等。**2. 完成服务的高性能高可用建设**。更多的关注服务的性能、效率、承受能力等。包括但不仅限于机器运维、日志系统、监控系统、可视化图表、错误追踪、数据备份、服务容灾、承载能力等。



项目仓库地址

{% hint style="info" %}
后端：[https://github.com/GatesMa/desirefu](https://github.com/GatesMa/desirefu)

微信小程序端：[https://github.com/GatesMa/desirefu-mini-program](https://github.com/GatesMa/desirefu-mini-program)
{% endhint %}

Wiki文档地址：[https://wiki.desirefu.gatesma.cn/](https://wiki.desirefu.gatesma.cn/)

API-DOC地址：[http://doc.desirefu.gatesma.cn/desire\_fu/v1](http://doc.desirefu.gatesma.cn/desire_fu/v1)

Kibana监控地址：[跳转](http://182.61.51.79:5601/app/kibana#/dashboard/77923bd0-9850-11eb-9c1a-b5c51315c15f?_g=%28refreshInterval:%28pause:!t,value:0%29,time:%28from:now%2Fy,mode:quick,to:now%2Fy%29%29&_a=%28description:'',filters:!%28%29,fullScreenMode:!f,options:%28darkTheme:!f,hidePanelTitles:!f,useMargins:!t%29,panels:!%28%28embeddableConfig:%28%29,gridData:%28h:10,i:'1',w:15,x:0,y:0%29,id:'592a7ac0-907d-11eb-9c1a-b5c51315c15f',panelIndex:'1',type:visualization,version:'6.4.3'%29,%28embeddableConfig:%28%29,gridData:%28h:10,i:'2',w:9,x:15,y:0%29,id:'7546fb70-907d-11eb-9c1a-b5c51315c15f',panelIndex:'2',type:visualization,version:'6.4.3'%29,%28embeddableConfig:%28%29,gridData:%28h:15,i:'3',w:24,x:0,y:10%29,id:b185cda0-907d-11eb-9c1a-b5c51315c15f,panelIndex:'3',type:visualization,version:'6.4.3'%29,%28embeddableConfig:%28%29,gridData:%28h:15,i:'4',w:24,x:24,y:15%29,id:'2ae2d580-907e-11eb-9c1a-b5c51315c15f',panelIndex:'4',type:visualization,version:'6.4.3'%29,%28embeddableConfig:%28%29,gridData:%28h:15,i:'5',w:24,x:0,y:25%29,id:b1345cd0-907e-11eb-9c1a-b5c51315c15f,panelIndex:'5',type:visualization,version:'6.4.3'%29,%28embeddableConfig:%28%29,gridData:%28h:15,i:'6',w:24,x:24,y:0%29,id:e1a9b780-907d-11eb-9c1a-b5c51315c15f,panelIndex:'6',type:visualization,version:'6.4.3'%29%29,query:%28language:lucene,query:''%29,timeRestore:!f,title:Dashboard,viewMode:view%29)

演示视频Demo：[https://dfu-1257282228.cos.ap-chengdu.myqcloud.com/dfu/video/demo.mp4](https://dfu-1257282228.cos.ap-chengdu.myqcloud.com/dfu/video/demo.mp4)

## 当前进展：

第一阶段进度：`70%`

第二阶段进度：`20%`

当前代码量：5w6（前后端合计），涉及数据库表21张，接口42个。购买服务器5台、数据库实例1个、对象存储服务，购买设备金额500+

## 简要技术选型

{% hint style="info" %}
开发还未完成，使用到的技术框架等都待完善
{% endhint %}

* Java、SpringBoot
* 微信小程序、ColorUI
* gradle、jooq、druid、screw



