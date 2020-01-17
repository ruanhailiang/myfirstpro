# 转发



### 转发 <a id="&#x8F6C;&#x53D1;"></a>

####  获取更多转发信息 <a id="&#x83B7;&#x53D6;&#x66F4;&#x591A;&#x8F6C;&#x53D1;&#x4FE1;&#x606F;"></a>

通常开发者希望转发出去的小程序被二次打开的时候能够获取到一些信息，例如群的标识。现在通过调用 [wx.showShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.showShareMenu.html) 并且设置 `withShareTicket` 为 `true` ，当用户将小程序转发到任一群聊之后，此转发卡片在群聊中被其他用户打开时，可以在 [App.onLaunch](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onlaunchobject-object) 或 [App.onShow](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onshowobject-object) 获取到一个 `shareTicket`。通过调用 [wx.getShareInfo](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.getShareInfo.html) 接口传入此 `shareTicket` 可以获取到转发信息。

####  页面内发起转发 <a id="&#x9875;&#x9762;&#x5185;&#x53D1;&#x8D77;&#x8F6C;&#x53D1;"></a>

> 基础库 1.2.0 开始支持，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。

通过给 `button` 组件设置属性 `open-type="share"`，可以在用户点击按钮后触发 [`Page.onShareAppMessage`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onshareappmessageobject-object) 事件，相关组件：[button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)。

####  使用指引 <a id="&#x4F7F;&#x7528;&#x6307;&#x5F15;"></a>

转发按钮，旨在帮助用户更流畅地与好友分享内容和服务。转发，应是用户自发的行为，且在需要时触手可及。开发者在使用时若遵从以下指引，会得到更佳的用户体验。

1. 含义清晰：明确、一目了然的图形按钮，将为用户减少理解的时间。在我们的资源下载中心，你可以找到这样的按钮素材并直接使用。或者你可以根据自己业务的设计风格，灵活设计含义清晰的按钮的样式。当然，你也可以直接使用带文案的按钮，“转发给好友”，它也足够明确。
2. 方便点击：按钮点击热区不宜过小，亦不宜过大。同时，转发按钮与其他按钮一样，热区也不宜过密，以免用户误操作。
3. 按需出现：并非所有页面都适合放置转发按钮，涉及用户隐私的非公开内容，或可能打断用户完成当前操作体验的场景，该功能并不推荐使用。同时，由于转发过程中，我们将截取用户屏幕图像作为配图，因此，需要注意帮助用户屏蔽个人信息。
4. 尊重意愿：理所当然，并非所有的用户，都喜欢与朋友分享你的小程序。因此，它不应该成为一个诱导或强制行为，如转发后才能解锁某项功能等。请注意，这类做法不仅不被推荐，还可能违反我们的[《运营规范》](https://mp.weixin.qq.com/debug/wxadoc/product/index.html)，我们强烈建议你在使用前阅读这部分内容。

以上，我们陈列了最重要的几点，如果你有时间，可以完整浏览[《设计指南》](https://mp.weixin.qq.com/debug/wxadoc/design/index.html)，相信会有更多的收获。

####  Tips <a id="Tips"></a>

1. 不自定义转发图片的情况下，默认会取当前页面，从顶部开始，高度为 80% 屏幕宽度的图像作为转发图片。
2. 转发的调试支持请查看 [普通转发的调试支持](https://developers.weixin.qq.com/miniprogram/dev/devtools/different.html#普通的转发) 和 [带 shareTicket 的转发](https://developers.weixin.qq.com/miniprogram/dev/devtools/different.html#带-shareticket-的转发)
3. 只有转发到群聊中打开才可以获取到 `shareTickets` 返回值，单聊没有 `shareTickets`
4. `shareTicket` 仅在当前小程序生命周期内有效
5. 由于策略变动，小程序群相关能力进行调整，开发者可先使用 [wx.getShareInfo](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.getShareInfo.html) 接口中的群 ID 进行功能开发。

### 动态消息 <a id="&#x52A8;&#x6001;&#x6D88;&#x606F;"></a>

从基础库 [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，支持转发动态消息。动态消息对比普通消息，有以下特点：

1. 消息发出去之后，开发者可以通过后台接口修改**部分**消息内容。
2. 消息有对应的提醒按钮，用户点击提醒按钮可以订阅提醒，开发者可以通过后台修改消息状态并推送一次提醒消息给订阅了提醒的用户

####  消息属性 <a id="&#x6D88;&#x606F;&#x5C5E;&#x6027;"></a>

动态消息有状态、文字内容、文字颜色。

 **状态**

消息有两个状态，分别有其对应的文字内容和颜色。其中状态 0 可以转移到状态 0 和 1，状态 1 无法再转移。

| 状态 | 文字内容 | 颜色 | 允许转移的状态 |
| :--- | :--- | :--- | :--- |
| 0 | "成员正在加入，当前 {member\_count}/{room\_limit} 人" | \#FA9D39 | 0, 1 |
| 1 | "已开始" | \#CCCCCC | 无 |

 **状态参数**

每个状态转移的时候可以携带参数，具体参数说明如下。

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| member\_count | string | 状态 0 时有效，文字内容模板中 `member_count` 的值 |
| room\_limit | string | 状态 0 时有效，文字内容模板中 `room_limit` 的值 |
| path | string | 状态 1 时有效，点击「进入」启动小程序时使用的路径。对于小游戏，没有页面的概念，可以用于传递查询字符串（query），如 `"?foo=bar"` |
| version\_type | string | 状态 1 时有效，点击「进入」启动小程序时使用的版本。有效参数值为：`develop`（开发版），`trial`（体验版），`release`（正式版） |

####  使用方法 <a id="&#x4F7F;&#x7528;&#x65B9;&#x6CD5;"></a>

 **一、创建 activity\_id**

每条动态消息可以理解为一个活动，活动发起前需要通过 [updatableMessage.createActivityId](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/updatable-message/updatableMessage.createActivityId.html) 接口创建 `activity_id`。后续转发动态消息以及更新动态消息都需要传入这个 `activity_id`。

活动的默认有效期是 24 小时。活动结束后，消息内容会变成统一的样式：

* 文字内容：“已结束”
* 文字颜色：`#00ff00`

 **二、在转发之前声明消息类型为动态消息**

通过调用 [wx.updateShareMenu](https://developers.weixin.qq.com/miniprogram/dev/api/share/wx.updateShareMenu.html) 接口，传入 `isUpdatableMessage: true`，以及 `templateInfo`、`activityId` 参数。其中 `activityId` 从步骤一中获得。

```text
wx.updateShareMenu({
  withShareTicket: true,
  isUpdatableMessage: true,
  activityId: '', // 活动 ID
  templateInfo: {
    parameterList: [{
      name: 'member_count',
      value: '1'
    }, {
      name: 'room_limit',
      value: '3'
    }]
  }
})
```

 **三、修改动态消息内容**

动态消息发出去之后，可以通过 [updatableMessage.setUpdatableMsg](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/updatable-message/updatableMessage.setUpdatableMsg.html) 修改消息内容。

####  低版本兼容 <a id="&#x4F4E;&#x7248;&#x672C;&#x517C;&#x5BB9;"></a>

对于不支持动态消息的客户端版本，收到动态消息后会展示成普通消息

