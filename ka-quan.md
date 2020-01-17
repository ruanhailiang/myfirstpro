# 卡券

###  卡券 <a id="&#x5361;&#x5238;"></a>

####  说明 <a id="&#x8BF4;&#x660E;"></a>

小程序卡券接口支持在小程序中领取/查看/使用公众号 AppId 创建的会员卡、票、券（含通用卡）。更多使用方法可参考 [小程序&卡券打通](https://mp.weixin.qq.com/cgi-bin/announce?action=getannouncement&key=1490190158&version=1&lang=zh_CN&platform=2)

####  使用条件 <a id="&#x4F7F;&#x7528;&#x6761;&#x4EF6;"></a>

目前只有认证小程序才能使用卡券接口，可参考 [指引](https://developers.weixin.qq.com/miniprogram/product/renzheng.html) 进行认证。

####  接口 <a id="&#x63A5;&#x53E3;"></a>

小程序内可以通过 [wx.addCard](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/card/wx.addCard.html) 接口给用户添加卡券。通过 [wx.openCard](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/card/wx.openCard.html) 让用户选择已有卡券。



### 会员卡组件 <a id="&#x4F1A;&#x5458;&#x5361;&#x7EC4;&#x4EF6;"></a>

开发者可以在小程序内调用该接口拉起会员开卡组件，方便用户快速填写会员注册信息并领卡。该接口拉起开卡组件无须提前将开卡组件和发起小程序绑定至同一个公众号，开发者直接调用即可。

调用前开发者须完成以下步骤：

1. 创建一张微信会员卡并设置为一键激活模式；
2. 设置开卡字段；
3. 获取开卡组件参数；

详情查看会员开卡组件介绍：[会员开卡组件](https://mp.weixin.qq.com/cgi-bin/announce?action=getannouncement&key=1479824356&version=1&lang=zh_CN&platform=2)

**参数说明**

| 参数名 | 类型 | 是否必填 | 参数说明 |
| :--- | :--- | :--- | :--- |
| appId | String | 是 | 填写 wxeb490c6f9b154ef9，固定为此appid |
| extraData | Object | 是 | 开卡组件参数，由第3步获取，包含以下三个参数 |
| encrypt\_card\_id | String | 是 | 加密 card\_id，传入前须 urldecode |
| outer\_str | String | 是 | 会员卡领取渠道值，会在卡券领取事件回传给商户 |
| biz | String | 是 | 商户公众号标识参数，传入前须 urldecode |
| success | Function | 否 | 接口调用成功的回调函数 |
| fail | Function | 否 | 接口调用失败的回调函数 |
| complete | Function | 否 | 接口调用结束的回调函数（调用成功、失败都会执行） |

**返回参数**

| 参数名 | 类型 | 参数说明 |
| :--- | :--- | :--- |
| errMsg | String | 调用结果 |

**示例代码**

```text
wx.navigateToMiniProgram({
  appId: 'wxeb490c6f9b154ef9', //固定为此 appid，不可改动
  extraData: data, // 包括 encrypt_card_id, outer_str, biz三个字段，须从 step3 中获得的链接中获取参数
  success: function() {
  },
  fail: function() {
  },
  complete: function() {
  }
})
```

`navigateToMiniProgram`接口即将废弃，新版本中请使用[navigator](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)组件来使用此功能

```text
<navigator target="miniProgram" app-id="wxeb490c6f9b154ef9" extra-data="{{data}}">会员卡开卡</navigator>
```

**返回说明**

在 App.onShow 里判断从会员开卡小程序返回的数据data

1. 判断 data.referrerInfo.appId 是否为开卡小程序 appId `wxeb490c6f9b154ef9`，如果不是则中止判断
2. 判断是否有 data.referrerInfo.extraData 是否有数据，如果没有，表示用户未激活直接左滑/点返回键返回，或者用户已经激活
3. 若用户激活成功，可以从 data.referrerInfo.extraData 中获取 activate\_ticket，card\_id，code 参数用于下一步操作

 **Tip**

1. 在开发者工具上调用此 API 并不会真实的跳转到另外的小程序，但是开发者工具会校验本次调用跳转是否成功详情
2. 开发者工具上支持被跳转的小程序处理接收参数的调试详情
3. 开卡组件是使用wx.navigateToMiniProgram开发的官方组件，跳转时无须绑定同一个公众号，直接调用即可

####  <a id="&#x8BF4;&#x660E;"></a>

