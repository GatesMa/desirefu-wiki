# 广场页

相比于首页，广场页的开发时长要大的多，主要涉及很多的独立页面。

### （1）广场页概览

![&#x6982;&#x89C8;](../../../.gitbook/assets/image%20%2855%29.png)

### （2）设计

广场页的开头是一个图标排列的列表，5个一行，采用了flex布局，依然是用colorUI原生框架完成，点击热门、问答、我的收藏后可以跳转到相应的页面，灰色的图标为待完善。中部是一个公告栏，可以展示一段文字公告（“小程序...“），如果文字过长，采用了截断式样式，点击公告栏，可以展开公告文字的详细信息，点击右边的关闭图标实现公告栏关闭。

下面是与我有关页面卡，有两个，一个是消息页一个是入队申请页面，右上角的数字代表了消息的数量，点击后可以进入详细页面查看，下面是一个纯图片的轮播图，可以展示一些信息，当然，这些公告栏和轮播图的图片是可以动态修改的，可以在运营OSS系统中修改，非常的方便。

目前页面上的所有数据都是从后端获取的，可以动态改变。



