# 用户信息



### 小程序登录 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x767B;&#x5F55;"></a>

小程序可以通过微信官方提供的登录能力方便地获取微信提供的用户身份标识，快速建立小程序内的用户体系。

####  登录流程时序 <a id="&#x767B;&#x5F55;&#x6D41;&#x7A0B;&#x65F6;&#x5E8F;"></a>

![](https://res.wx.qq.com/wxdoc/dist/assets/img/api-login.2fcc9f35.jpg)

 **说明：**

1. 调用 [wx.login\(\)](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 获取 **临时登录凭证code** ，并回传到开发者服务器。
2. 调用 [auth.code2Session](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/login/auth.code2Session.html) 接口，换取 **用户唯一标识 OpenID** 和 **会话密钥 session\_key**。

之后开发者服务器可以根据用户标识来生成自定义登录态，用于后续业务逻辑中前后端交互时识别用户身份。

**注意：**

1. 会话密钥 `session_key` 是对用户数据进行 [加密签名](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html) 的密钥。为了应用自身的数据安全，开发者服务器**不应该把会话密钥下发到小程序，也不应该对外提供这个密钥**。
2. 临时登录凭证 code 只能使用一次

### UnionID 机制说明 <a id="UnionID-&#x673A;&#x5236;&#x8BF4;&#x660E;"></a>

如果开发者拥有多个移动应用、网站应用、和公众帐号（包括小程序），可通过 UnionID 来区分用户的唯一性，因为只要是同一个微信开放平台帐号下的移动应用、网站应用和公众帐号（包括小程序），用户的 UnionID 是唯一的。换句话说，同一用户，对同一个微信开放平台下的不同应用，unionid是相同的。

####  UnionID获取途径 <a id="UnionID&#x83B7;&#x53D6;&#x9014;&#x5F84;"></a>

绑定了开发者帐号的小程序，可以通过以下途径获取 UnionID。

1. 调用接口 [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html)，从解密数据中获取 UnionID。注意本接口需要用户授权，请开发者妥善处理用户拒绝授权后的情况。
2. 如果开发者帐号下存在**同主体的**公众号，并且该用户已经关注了该公众号。开发者可以直接通过 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) + `code2Session` 获取到该用户 UnionID，无须用户再次授权。
3. 如果开发者帐号下存在**同主体的**公众号或移动应用，并且该用户已经授权登录过该公众号或移动应用。开发者也可以直接通过 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) + `code2Session` 获取到该用户 UnionID ，无须用户再次授权。
4. 用户在小程序（暂不支持小游戏）中支付完成后，开发者可以直接通过`getPaidUnionId`接口获取该用户的 UnionID，无需用户授权。注意：本接口仅在用户支付完成后的5分钟内有效，请开发者妥善处理。
5. 小程序端调用[云函数](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/capabilities.html#%E4%BA%91%E5%87%BD%E6%95%B0)时，如果开发者帐号下存在**同主体的**公众号，并且该用户已经关注了该公众号，可在云函数中通过 [cloud.getWXContext](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-server-api/utils/getWXContext.html) 获取 UnionID。
6. 小程序端调用[云函数](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/capabilities.html#%E4%BA%91%E5%87%BD%E6%95%B0)时，如果开发者帐号下存在**同主体的**公众号或移动应用，并且该用户已经授权登录过该公众号或移动应用，也可在云函数中通过 [cloud.getWXContext](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-server-api/utils/getWXContext.html) 获取 UnionID。

####  微信开放平台绑定小程序流程 <a id="&#x5FAE;&#x4FE1;&#x5F00;&#x653E;&#x5E73;&#x53F0;&#x7ED1;&#x5B9A;&#x5C0F;&#x7A0B;&#x5E8F;&#x6D41;&#x7A0B;"></a>

登录[微信开放平台](https://open.weixin.qq.com) — 管理中心 — 小程序 — 绑定小程序

![img](https://res.wx.qq.com/wxdoc/dist/assets/img/union_bind.e13e592b.png)

### 授权 <a id="&#x6388;&#x6743;"></a>

部分接口需要经过用户授权同意才能调用。我们把这些接口按使用范围分成多个 `scope` ，用户选择对 `scope` 来进行授权，当授权给一个 `scope` 之后，其对应的所有接口都可以直接使用。

此类接口调用时：

* 如果用户未接受或拒绝过此权限，会弹窗询问用户，用户点击同意后方可调用接口；
* 如果用户已授权，可以直接调用接口；
* 如果用户已拒绝授权，则不会出现弹窗，而是直接进入接口 fail 回调。**请开发者兼容用户拒绝授权的场景。**

####  获取用户授权设置 <a id="&#x83B7;&#x53D6;&#x7528;&#x6237;&#x6388;&#x6743;&#x8BBE;&#x7F6E;"></a>

开发者可以使用 [wx.getSetting](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/setting/wx.getSetting.html) 获取用户当前的授权状态。

####  打开设置界面 <a id="&#x6253;&#x5F00;&#x8BBE;&#x7F6E;&#x754C;&#x9762;"></a>

用户可以在小程序设置界面（「右上角」 - 「关于」 - 「右上角」 - 「设置」）中控制对该小程序的授权状态。

开发者可以调用 [wx.openSetting](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/setting/wx.openSetting.html) 打开设置界面，引导用户开启授权。

####  提前发起授权请求 <a id="&#x63D0;&#x524D;&#x53D1;&#x8D77;&#x6388;&#x6743;&#x8BF7;&#x6C42;"></a>

开发者可以使用 [wx.authorize](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/authorize/wx.authorize.html) 在调用需授权 API 之前，提前向用户发起授权请求。

####  scope 列表 <a id="scope-&#x5217;&#x8868;"></a>

| scope | 对应接口 | 描述 |
| :--- | :--- | :--- |
| scope.userInfo | [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html) | 用户信息 |
| scope.userLocation | [wx.getLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.getLocation.html), [wx.chooseLocation](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.chooseLocation.html) | 地理位置 |
| scope.userLocationBackground | [wx.startLocationUpdateBackground](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.startLocationUpdateBackground.html) | 后台定位 |
| scope.address | [wx.chooseAddress](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/address/wx.chooseAddress.html) | 通讯地址 |
| scope.invoiceTitle | [wx.chooseInvoiceTitle](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/invoice/wx.chooseInvoiceTitle.html) | 发票抬头 |
| scope.invoice | [wx.chooseInvoice](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/invoice/wx.chooseInvoice.html) | 获取发票 |
| scope.werun | [wx.getWeRunData](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/werun/wx.getWeRunData.html) | 微信运动步数 |
| scope.record | [wx.startRecord](https://developers.weixin.qq.com/miniprogram/dev/api/media/recorder/wx.startRecord.html) | 录音功能 |
| scope.writePhotosAlbum | [wx.saveImageToPhotosAlbum](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.saveImageToPhotosAlbum.html), [wx.saveVideoToPhotosAlbum](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/wx.saveVideoToPhotosAlbum.html) | 保存到相册 |
| scope.camera | [camera](https://developers.weixin.qq.com/miniprogram/dev/component/camera.html) 组件 | 摄像头 |

####  授权有效期 <a id="&#x6388;&#x6743;&#x6709;&#x6548;&#x671F;"></a>

一旦用户明确同意或拒绝过授权，其授权关系会记录在后台，直到用户主动删除小程序。

####  最佳实践 <a id="&#x6700;&#x4F73;&#x5B9E;&#x8DF5;"></a>

在真正需要使用授权接口时，才向用户发起授权申请，并在授权申请中说明清楚要使用该功能的理由。

####  注意事项 <a id="&#x6CE8;&#x610F;&#x4E8B;&#x9879;"></a>

1. `wx.authorize({scope: "scope.userInfo"})`，不会弹出授权窗口，请使用 [`<button open-type="getUserInfo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)
2. 需要授权 `scope.userLocation`、`scope.userLocationBackground` 时必须[配置地理位置用途说明](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#permission)。

####  后台定位 <a id="&#x540E;&#x53F0;&#x5B9A;&#x4F4D;"></a>

与其它类型授权不同的是，scope.userLocationBackground 不会弹窗提醒用户。需要用户在设置页中，主动将“位置信息”选项设置为“使用小程序期间和离开小程序后”。开发者可以通过调用[wx.openSetting](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/setting/wx.openSetting.html)，打开设置页。

![background-location](https://res.wx.qq.com/wxdoc/dist/assets/img/background-location.8290c764.png)



### 服务端获取开放数据 <a id="&#x670D;&#x52A1;&#x7AEF;&#x83B7;&#x53D6;&#x5F00;&#x653E;&#x6570;&#x636E;"></a>

小程序可以通过各种前端接口获取微信提供的开放数据。考虑到开发者服务端也需要获取这些开放数据，微信提供了两种获取方式：

* [方式一](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html#method-decode)：开发者后台校验与解密开放数据
* [方式二](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html#method-cloud)：[云调用](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/openapi/openapi.html)直接获取开放数据（[云开发](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html)）

####  方式一：开发者后台校验与解密开放数据 <a id="&#x65B9;&#x5F0F;&#x4E00;&#xFF1A;&#x5F00;&#x53D1;&#x8005;&#x540E;&#x53F0;&#x6821;&#x9A8C;&#x4E0E;&#x89E3;&#x5BC6;&#x5F00;&#x653E;&#x6570;&#x636E;"></a>

微信会对这些开放数据做签名和加密处理。开发者后台拿到开放数据后可以对数据进行校验签名和解密，来保证数据不被篡改。

![](https://res.wx.qq.com/wxdoc/dist/assets/img/signature.8a30a825.jpg)

签名校验以及数据加解密涉及用户的会话密钥 session\_key。 开发者应该事先通过 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 登录流程获取会话密钥 session\_key 并保存在服务器。为了数据不被篡改，开发者不应该把 session\_key 传到小程序客户端等服务器外的环境。

 **数据签名校验**

为了确保开放接口返回用户数据的安全性，微信会对明文数据进行签名。开发者可以根据业务需要对数据包进行签名校验，确保数据的完整性。

1. 通过调用接口（如 [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html)）获取数据时，接口会同时返回 rawData、signature，其中 signature = sha1\( rawData + session\_key \)
2. 开发者将 signature、rawData 发送到开发者服务器进行校验。服务器利用用户对应的 session\_key 使用相同的算法计算出签名 signature2 ，比对 signature 与 signature2 即可校验数据的完整性。

**如 wx.getUserInfo的数据校验：**

接口返回的rawData：

```text
{
  "nickName": "Band",
  "gender": 1,
  "language": "zh_CN",
  "city": "Guangzhou",
  "province": "Guangdong",
  "country": "CN",
  "avatarUrl": "http://wx.qlogo.cn/mmopen/vi_32/1vZvI39NWFQ9XM4LtQpFrQJ1xlgZxx3w7bQxKARol6503Iuswjjn6nIGBiaycAjAtpujxyzYsrztuuICqIM5ibXQ/0"
}
```

用户的 session-key：

```text
HyVFkGl5F5OQWJZZaNzBBg==
```

用于签名的字符串为：

```text
{"nickName":"Band","gender":1,"language":"zh_CN","city":"Guangzhou","province":"Guangdong","country":"CN","avatarUrl":"http://wx.qlogo.cn/mmopen/vi_32/1vZvI39NWFQ9XM4LtQpFrQJ1xlgZxx3w7bQxKARol6503Iuswjjn6nIGBiaycAjAtpujxyzYsrztuuICqIM5ibXQ/0"}HyVFkGl5F5OQWJZZaNzBBg==
```

使用sha1得到的结果为

```text
75e81ceda165f4ffa64f4068af58c64b8f54b88c
```

 **加密数据解密算法**

接口如果涉及敏感数据（如[wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html)当中的 openId 和 unionId），接口的明文内容将不包含这些敏感数据。开发者如需要获取敏感数据，需要对接口返回的**加密数据\(encryptedData\)** 进行对称解密。 解密算法如下：

1. 对称解密使用的算法为 AES-128-CBC，数据采用PKCS\#7填充。
2. 对称解密的目标密文为 Base64\_Decode\(encryptedData\)。
3. 对称解密秘钥 aeskey = Base64\_Decode\(session\_key\), aeskey 是16字节。
4. 对称解密算法初始向量 为Base64\_Decode\(iv\)，其中iv由数据接口返回。

微信官方提供了多种编程语言的示例代码（（[点击下载](https://res.wx.qq.com/wxdoc/dist/assets/media/aes-sample.eae1f364.zip)）。每种语言类型的接口名字均一致。调用方式可以参照示例。

另外，为了应用能校验数据的有效性，会在敏感数据加上数据水印\( watermark \)

**watermark参数说明：**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| appid | String | 敏感数据归属 appId，开发者可校验此参数与自身 appId 是否一致 |
| timestamp | Int | 敏感数据获取的时间戳, 开发者可以用于数据时效性校验 |

如接口 [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html) 敏感数据当中的 watermark：

```text
{
    "openId": "OPENID",
    "nickName": "NICKNAME",
    "gender": GENDER,
    "city": "CITY",
    "province": "PROVINCE",
    "country": "COUNTRY",
    "avatarUrl": "AVATARURL",
    "unionId": "UNIONID",
    "watermark":
    {
        "appid":"APPID",
        "timestamp":TIMESTAMP
    }
}
```

注：

1. 解密后得到的json数据根据需求可能会增加新的字段，旧字段不会改变和删减，开发者需要预留足够的空间

 **会话密钥 session\_key 有效性**

开发者如果遇到因为 session\_key 不正确而校验签名失败或解密失败，请关注下面几个与 session\_key 有关的注意事项。

1. [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 调用时，用户的 session\_key **可能**会被更新而致使旧 session\_key 失效（刷新机制存在最短周期，如果同一个用户短时间内多次调用 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)，并非每次调用都导致 session\_key 刷新）。开发者应该在明确需要重新登录时才调用 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)，及时通过 [auth.code2Session](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/login/auth.code2Session.html) 接口更新服务器存储的 session\_key。
2. 微信不会把 session\_key 的有效期告知开发者。我们会根据用户使用小程序的行为对 session\_key 进行续期。用户越频繁使用小程序，session\_key 有效期越长。
3. 开发者在 session\_key 失效时，可以通过重新执行登录流程获取有效的 session\_key。使用接口 [wx.checkSession](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.checkSession.html)可以校验 session\_key 是否有效，从而避免小程序反复执行登录流程。
4. 当开发者在实现自定义登录态时，可以考虑以 session\_key 有效期作为自身登录态有效期，也可以实现自定义的时效性策略。

####  方式二：云调用直接获取开放数据 <a id="&#x65B9;&#x5F0F;&#x4E8C;&#xFF1A;&#x4E91;&#x8C03;&#x7528;&#x76F4;&#x63A5;&#x83B7;&#x53D6;&#x5F00;&#x653E;&#x6570;&#x636E;"></a>

接口如果涉及敏感数据（如[wx.getWeRunData](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/werun/wx.getWeRunData.html)），接口的明文内容将不包含这些敏感数据，而是在返回的接口中包含对应敏感数据的 `cloudID` 字段，数据可以通过云函数获取。完整流程如下：

**1. 获取 cloudID**

使用 2.7.0 或以上版本的基础库，如果小程序已开通云开发，在开放数据接口的返回值中可以通过 `cloudID` 字段获取（与 `encryptedData` 同级），`cloudID` 有效期五分钟。

**2. 调用云函数**

调用云函数时，对传入的 `data` 参数，如果有顶层字段的值为通过 `wx.cloud.CloudID` 构造的 `CloudID`，则调用云函数时，这些字段的值会被替换为 `cloudID` 对应的开放数据，一次调用最多可替换 5 个 `CloudID`。

示例：

在小程序获取到 `cloudID` 之后发起调用：

```text
wx.cloud.callFunction({
  name: 'myFunction',
  data: {
    weRunData: wx.cloud.CloudID('xxx'), // 这个 CloudID 值到云函数端会被替换
    obj: {
      shareInfo: wx.cloud.CloudID('yyy'), // 非顶层字段的 CloudID 不会被替换，会原样字符串展示
    }
  }
})
```

在云函数收到的 `event` 示例：

```text
// event
{
  // weRunData 的值已被替换为开放数据
  "weRunData": {
    "cloudID": "xxx",
    "data": {
      "stepInfoList": [
        {
          "step": 5000,
          "timestamp": 1554814312,
        }
      ],
      "watermark": {
        "appid": "wx1111111111",
        "timestamp": 1554815786
      }
    }
  },
  "obj": {
    // 非顶层字段维持原样
    "shareInfo": "yyy",
  }
}
```

如果 `cloudID` 非法或过期，则在 `event` 中获取得到的将是一个有包含错误码、错误信息和原始 `cloudID` 的对象。过期 `cloudID` 换取结果示例：

```text
// event
{
  "weRunData": {
    "cloudID": "xxx",
    "errCode": -601006,
    "errMsg": "cloudID expired."
  },
  // ...
}

```



### 获取手机号 <a id="&#x83B7;&#x53D6;&#x624B;&#x673A;&#x53F7;"></a>

获取微信用户绑定的手机号，需先调用[wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)接口。

因为需要用户主动触发才能发起获取手机号接口，所以该功能不由 API 来调用，需用 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件的点击来触发。

**注意：目前该接口针对非个人开发者，且完成了认证的小程序开放（不包含海外主体）。需谨慎使用，若用户举报较多或被发现在不必要场景下使用，微信有权永久回收该小程序的该接口权限。**

####  使用方法 <a id="&#x4F7F;&#x7528;&#x65B9;&#x6CD5;"></a>

需要将 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件 `open-type` 的值设置为 `getPhoneNumber`，当用户点击并同意之后，可以通过 `bindgetphonenumber` 事件回调获取到微信服务器返回的加密数据， 然后在第三方服务端结合 `session_key` 以及 `app_id` 进行解密获取手机号。

####  注意 <a id="&#x6CE8;&#x610F;"></a>

在回调中调用 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 登录，可能会刷新登录态。此时服务器使用 code 换取的 sessionKey 不是加密时使用的 sessionKey，导致解密失败。建议开发者提前进行 `login`；或者在回调中先使用 `checkSession` 进行登录态检查，避免 `login` 刷新登录态。

####  代码示例 <a id="&#x4EE3;&#x7801;&#x793A;&#x4F8B;"></a>

```text
<button open-type="getPhoneNumber" bindgetphonenumber="getPhoneNumber"></button>
```

```text
Page({
  getPhoneNumber (e) {
    console.log(e.detail.errMsg)
    console.log(e.detail.iv)
    console.log(e.detail.encryptedData)
  }
})
```

####  返回参数说明 <a id="&#x8FD4;&#x56DE;&#x53C2;&#x6570;&#x8BF4;&#x660E;"></a>

| 参数 | 类型 | 说明 | 最低版本 |
| :--- | :--- | :--- | :--- |
| encryptedData | String | 包括敏感数据在内的完整用户信息的加密数据，详细见[加密数据解密算法](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html#%E5%8A%A0%E5%AF%86%E6%95%B0%E6%8D%AE%E8%A7%A3%E5%AF%86%E7%AE%97%E6%B3%95) |  |
| iv | String | 加密算法的初始向量，详细见[加密数据解密算法](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html#%E5%8A%A0%E5%AF%86%E6%95%B0%E6%8D%AE%E8%A7%A3%E5%AF%86%E7%AE%97%E6%B3%95) |  |
| cloudID | string | 敏感数据对应的云 ID，开通云开发的小程序才会返回，可通过云调用直接获取开放数据，详细见[云调用直接获取开放数据](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html#method-cloud) | 基础库 [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

获取得到的开放数据为以下 json 结构：

```text
{
    "phoneNumber": "13580006666",
    "purePhoneNumber": "13580006666",
    "countryCode": "86",
    "watermark":
    {
        "appid":"APPID",
        "timestamp": TIMESTAMP
    }
}
```

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| phoneNumber | String | 用户绑定的手机号（国外手机号会有区号） |
| purePhoneNumber | String | 没有区号的手机号 |
| countryCode | String | 区号 |



### 生物认证 <a id="&#x751F;&#x7269;&#x8BA4;&#x8BC1;"></a>

小程序通过 [SOTER](https://github.com/Tencent/soter) 提供以下生物认证方式。

目前暂时只支持指纹识别认证。设备支持的生物认证方式可使用 [wx.checkIsSupportSoterAuthentication](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/soter/wx.checkIsSupportSoterAuthentication.html) 查询

####  调用流程 <a id="&#x8C03;&#x7528;&#x6D41;&#x7A0B;"></a>

![](https://res.wx.qq.com/wxdoc/dist/assets/img/soter.e18d4893.png)

 **流程步骤说明**

1. 调用 [wx.startSoterAuthentication](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/soter/wx.startSoterAuthentication.html)，获取 `resultJSON` 和 `resultJSONSignature`
2. （可选）签名校验。此处 `resultJSONSignature` 使用 SHA256withRSA/PSS 作为签名算法进行验签。此公式数学定义如下: `bool 验签结果=verify(用于签名的原串，签名串，验证签名的公钥)`
3. 微信提供后台接口用于可信的密钥验签服务，微信将保证该接口返回的验签结果的正确性与可靠性，并且对于 Android root 情况下该接口具有上述特征（将返回是否保证root情况安全性）。

接口地址：

```text
POST http://api.weixin.qq.com/cgi-bin/soter/verify_signature?access_token=%access_token
```

post 数据内容（JSON 编码）:

```text
{"openid":"$openid", "json_string" : "$resultJSON", "json_signature" : "$resultJSONSignature" }
```

请求返回数据内容（JSON 编码）:

```text
// 验证成功返回
{"is_ok":true}
// 验证失败返回
{"is_ok":false}
// 接口调用失败
{"errcode":"xxx,"errmsg":"xxxxx"}
```

