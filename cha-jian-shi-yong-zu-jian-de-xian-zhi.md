# 插件使用组件的限制

## 插件使用组件的限制 <a id="&#x63D2;&#x4EF6;&#x4F7F;&#x7528;&#x7EC4;&#x4EF6;&#x7684;&#x9650;&#x5236;"></a>

在插件开发中，以下组件不能在插件页面中使用：

* 开放能力（open-type）为以下之一的 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)：
  * contact（打开客服会话）
  * getPhoneNumber（获取用户手机号）
  * getUserInfo（获取用户信息）
* [open-data](https://developers.weixin.qq.com/miniprogram/dev/component/open-data.html)
* [web-view](https://developers.weixin.qq.com/miniprogram/dev/component/web-view.html)

以下组件的使用对基础库版本有要求：

* [navigator](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html) 需要基础库版本 [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)
* [live-player](https://developers.weixin.qq.com/miniprogram/dev/component/live-player.html) 和 [live-pusher](https://developers.weixin.qq.com/miniprogram/dev/component/live-pusher.html) 需要基础库版本 [2.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)

