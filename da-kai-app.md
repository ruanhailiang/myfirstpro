# 打开 App

### 打开 App <a id="&#x6253;&#x5F00;-App"></a>

此功能需要用户主动触发才能打开 APP，所以不由 API 来调用，需要用 `open-type` 的值设置为 `launchApp` 的 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件的点击来触发。

当小程序从 APP 分享消息卡片的场景打开（场景值 1036，APP 分享小程序文档 [iOS](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419317332) / [Android](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419317340)） 或从 APP 打开的场景打开时（场景值 1069），小程序会获得打开 APP 的能力，此时用户点击按钮可以打开分享该小程序卡片/拉起该小程序的 APP。即小程序不能打开任意 APP，只能 `跳回` APP。

在一个小程序的生命周期内，只有在特定条件下，才具有打开 APP 的能力。

在基础库 &lt; 2.5.1 的版本，这个能力的规则如下：

当小程序从 1069 场景打开时，可以打开 APP。

当小程序从非 1069 的打开时，会在小程序框架内部会管理的一个状态，为 true 则可以打开 APP，为 false 则不可以打开 APP。这个状态的维护遵循以下规则：

* 当小程序从 App 分享消息卡片（场景值1036）打开时，该状态置为 true。
* 当小程序从以下场景打开时，保持上一次打开小程序时打开 App 能力的状态：
  * 从其他小程序返回小程序（场景值1038）时（基础库 2.2.4 及以上版本支持）
  * 小程序从聊天顶部场景（场景值1089）中的「最近使用」内打开时
  * 长按小程序右上角菜单唤出最近使用历史（场景值1090）打开时
* 当小程序从非以上场景打开时，不具有打开 APP 的能力，该状态置为 false。

![](https://res.wx.qq.com/wxdoc/dist/assets/img/launch-app.5aaae552.png)

在基础库 &gt;= 2.5.1 时，这个能力的规则如下：

当小程序从任意场景打开时，会在小程序框架内部会管理的一个状态，为 true 则可以打开 APP，为 false 则不可以打开 APP。这个状态的维护遵循以下规则：

* 当小程序从 App 分享消息卡片（场景值1036）或从 APP 打开的场景打开时（场景值 1069）打开时，该状态置为 true。
* 当小程序从以下场景打开时，保持上一次打开小程序时打开 App 能力的状态：
  * 从其他小程序返回小程序（场景值1038）时（基础库 2.2.4 及以上版本支持）
  * 小程序从聊天顶部场景（场景值1089）中的「最近使用」内打开时
  * 长按小程序右上角菜单唤出最近使用历史（场景值1090）打开时
* 当小程序从非以上场景打开时，不具有打开 APP 的能力，该状态置为 false。

![](https://res.wx.qq.com/wxdoc/dist/assets/img/launch-app-new.991a7d95.png)

####  使用方法 <a id="&#x4F7F;&#x7528;&#x65B9;&#x6CD5;"></a>

 **小程序端**

需要将 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件 `open-type` 的值设置为 `launchApp`。如果需要在打开 APP 时向 APP 传递参数，可以设置 `app-parameter` 为要传递的参数。通过 `binderror` 可以监听打开 APP 的错误事件。

 **app 端**

APP 需要接入 OpenSDK。 文档请参考 [iOS](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=1417694084) / [Android](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=1417751808)

Android 第三方 app 需要处理 `ShowMessageFromWX.req` 的微信回调，iOS 则需要将 appId 添加到第三方 app 工程所属的 plist 文件 URL types 字段。 `app-parameter` 的获取方法，请参考 [Android SDKSample](https://open.weixin.qq.com/zh_CN/htmledition/res/dev/download/sdk/WeChatSDK_sample_Android.zip) 中 WXEntryActivity 中的 onResp 方法以及 [iOS SDKSample](https://open.weixin.qq.com/zh_CN/htmledition/res/dev/download/sdk/WeChatSDK_sample_iOS_1.4.2.1.zip) 中 WXApiDelegate 中的 onResp 方法。

####  代码示例 <a id="&#x4EE3;&#x7801;&#x793A;&#x4F8B;"></a>

```text
<button open-type="launchApp" app-parameter="wechat" binderror="launchAppError">打开APP</button>
```

```text
Page({
  launchAppError (e) {
    console.log(e.detail.errMsg)
  }
})
```

####  error 事件参数说明 <a id="error-&#x4E8B;&#x4EF6;&#x53C2;&#x6570;&#x8BF4;&#x660E;"></a>

| 值 | 说明 |
| :--- | :--- |
| invalid scene | 调用场景不正确，即此时的小程序不具备打开 APP 的能力。 |

