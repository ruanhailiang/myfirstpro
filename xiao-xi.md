# 消息

### 小程序订阅消息 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x8BA2;&#x9605;&#x6D88;&#x606F;"></a>

####  功能介绍 <a id="&#x529F;&#x80FD;&#x4ECB;&#x7ECD;"></a>

消息能力是小程序能力中的重要组成，我们为开发者提供了订阅消息能力，以便实现服务的闭环和更优的体验。

* 订阅消息推送位置：服务通知
* 订阅消息下发条件：用户自主订阅
* 订阅消息卡片跳转能力：点击查看详情可跳转至该小程序的页面

![intro](https://res.wx.qq.com/wxdoc/dist/assets/img/request-subscribe-message.3851318e.jpg)

####  使用说明 <a id="&#x4F7F;&#x7528;&#x8BF4;&#x660E;"></a>

####  步骤一：获取模板 ID <a id="&#x6B65;&#x9AA4;&#x4E00;&#xFF1A;&#x83B7;&#x53D6;&#x6A21;&#x677F;-ID"></a>

在微信公众平台手动配置获取模板 ID：  
 登录 [https://mp.weixin.qq.com](https://mp.weixin.qq.com) 获取模板，如果没有合适的模板，可以申请添加新模板，审核通过后可使用。

![intro](https://res.wx.qq.com/wxdoc/dist/assets/img/subscribe-message.b562750a.jpg)

####  步骤二：获取下发权限 <a id="&#x6B65;&#x9AA4;&#x4E8C;&#xFF1A;&#x83B7;&#x53D6;&#x4E0B;&#x53D1;&#x6743;&#x9650;"></a>

详见小程序端消息订阅接口 [wx.requestSubscribeMessage](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/subscribe-message/wx.requestSubscribeMessage.html)

####  步骤三：调用接口下发订阅消息 <a id="&#x6B65;&#x9AA4;&#x4E09;&#xFF1A;&#x8C03;&#x7528;&#x63A5;&#x53E3;&#x4E0B;&#x53D1;&#x8BA2;&#x9605;&#x6D88;&#x606F;"></a>

详见服务端消息发送接口 [subscribeMessage.send](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html)

### 模板消息 <a id="&#x6A21;&#x677F;&#x6D88;&#x606F;"></a>

> 微信6.5.2及以上版本支持

请注意，小程序模板消息接口将于2020年1月10日下线，开发者可使用[订阅消息功能](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/subscribe-message.html)  
 基于微信的通知渠道，我们为开发者提供了可以高效触达用户的模板消息能力，以便实现服务的闭环并提供更佳的体验。

* 模板推送位置：服务通知
* 模板下发条件：用户本人在微信体系内与页面有交互行为后触发，详见 [下发条件说明](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/template-message.html#%E4%B8%8B%E5%8F%91%E6%9D%A1%E4%BB%B6%E8%AF%B4%E6%98%8E)
* 模板跳转能力：点击查看详情仅能跳转下发模板的该帐号的各个页面

####  使用说明 <a id="&#x4F7F;&#x7528;&#x8BF4;&#x660E;"></a>

 **步骤一：获取模板 ID**

有两个方法可以获取模板 ID：

1. 通过模板消息管理接口获取模板 ID（详见 [模板消息管理](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/template-message.html#%E6%A8%A1%E6%9D%BF%E6%B6%88%E6%81%AF%E7%AE%A1%E7%90%86)）
2. 在微信公众平台手动配置获取模板 ID

登录 https://mp.weixin.qq.com 获取模板，如果没有合适的模板，可以申请添加新模板，审核通过后可使用，详见 [模板审核说明](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/template-message.html#%E5%AE%A1%E6%A0%B8%E8%AF%B4%E6%98%8E)

![](https://res.wx.qq.com/wxdoc/dist/assets/img/mp-notice.aea3c107.jpg)

 **步骤二：页面的 form 组件，属性 report-submit 为 true 时，可以声明为需要发送模板消息，此时点击按钮提交表单可以获取 formId，用于发送模板消息。或者当用户完成 支付行为，可以获取 prepay\_id 用于发送模板消息。**

 **步骤三：调用接口下发模板消息（详见 templateMessage.send ）**

**使用效果**

![](https://res.wx.qq.com/wxdoc/dist/assets/img/notice.0fade5b2.png)

####  模板消息管理 <a id="&#x6A21;&#x677F;&#x6D88;&#x606F;&#x7BA1;&#x7406;"></a>

1. [获取小程序模板库标题列表](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/template-message/templateMessage.getTemplateLibraryList.html)
2. [获取模板库某个模板标题下关键词库](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/template-message/templateMessage.getTemplateLibraryById.html)
3. [组合模板并添加至帐号下的个人模板库](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/template-message/templateMessage.addTemplate.html)
4. [获取帐号下已存在的模板列表](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/template-message/templateMessage.getTemplateList.html)
5. [删除帐号下的某个模板](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/template-message/templateMessage.deleteTemplate.html)

####  下发条件说明 <a id="&#x4E0B;&#x53D1;&#x6761;&#x4EF6;&#x8BF4;&#x660E;"></a>

 **1. 支付**

当用户在小程序内完成过支付行为，可允许开发者向用户在7天内推送有限条数的模板消息（1次支付可下发3条，多次支付下发条数独立，互相不影响）

 **2. 提交表单**

当用户在小程序内发生过提交表单行为且该表单声明为要发模板消息的，开发者需要向用户提供服务时，可允许开发者向用户在7天内推送有限条数的模板消息（1次提交表单可下发1条，多次提交下发条数独立，相互不影响）

####  审核说明 <a id="&#x5BA1;&#x6838;&#x8BF4;&#x660E;"></a>

 **1. 标题**

* 标题不能存在相同
* 标题意思不能存在过度相似
* 标题必须以“提醒”或“通知”结尾
* 标题不能带特殊符号、个性化字词等没有行业通用性的内容
* 标题必须能体现具体服务场景
* 标题不能涉及营销相关内容，包括不限于：消费优惠类、购物返利类、商品更新类、优惠券类、代金券类、红包类、会员卡类、积分类、活动类等营销倾向通知

 **2. 关键词**

* 同一标题下，关键词不能存在相同
* 同一标题下，关键词不能存在过度相似
* 关键词不能带特殊符号、个性化字词等没有行业通用性的内容
* 关键词内容示例必须与关键词对应匹配
* 关键词不能太过宽泛，需要具有限制性，例如：“内容”这个就太宽泛，不能审核通过

####  违规说明 <a id="&#x8FDD;&#x89C4;&#x8BF4;&#x660E;"></a>

除不能违反运营规范外，还不能违反以下规则，包括但不限于：

1. 不允许恶意诱导用户进行触发操作，以达到可向用户下发模板目的
2. 不允许恶意骚扰，下发对用户造成骚扰的模板
3. 不允许恶意营销，下发营销目的模板

####  处罚说明 <a id="&#x5904;&#x7F5A;&#x8BF4;&#x660E;"></a>

根据违规情况给予相应梯度的处罚，一般处罚规则如下：

* 第一次违规，删除违规模板以示警告，
* 第二次违规，封禁接口7天，
* 第三次违规，封禁接口30天，
* 第四次违规，永久封禁接口

处罚结果及原因以站内信形式告知

###  统一服务消息 <a id="&#x7EDF;&#x4E00;&#x670D;&#x52A1;&#x6D88;&#x606F;"></a>

为便于开发者对用户进行服务消息触达，简化小程序和公众号模板消息下发流程，小程序提供统一的服务消息下发接口。

####  相关接口 <a id="&#x76F8;&#x5173;&#x63A5;&#x53E3;"></a>

* [下发统一服务消息](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/uniform-message/uniformMessage.send.html)

###  客服消息 <a id="&#x5BA2;&#x670D;&#x6D88;&#x606F;"></a>

####  在页面使用客服消息 <a id="&#x5728;&#x9875;&#x9762;&#x4F7F;&#x7528;&#x5BA2;&#x670D;&#x6D88;&#x606F;"></a>

需要将 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件 `open-type` 的值设置为 `contact`，当用户点击后就会进入客服会话，如果用户在会话中点击了小程序消息，则会返回到小程序，开发者可以通过 `bindcontact` 事件回调获取到用户所点消息的页面路径 `path` 和对应的参数 `query`。

 **代码示例**

```text
<button open-type="contact" bindcontact="handleContact"></button>
```

```text
Page({
    handleContact (e) {
        console.log(e.detail.path)
        console.log(e.detail.query)
    }
})
```

 **返回参数说明**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| path | String | 小程序消息指定的路径 |
| query | Object | 小程序消息指定的查询参数 |

####  后台接入消息服务 <a id="&#x540E;&#x53F0;&#x63A5;&#x5165;&#x6D88;&#x606F;&#x670D;&#x52A1;"></a>

用户向小程序客服发送消息、或者进入会话等情况时，开发者填写的服务器配置 URL （如果使用的是云开发，则是配置的云函数）将得到微信服务器推送过来的消息和事件，开发者可以依据自身业务逻辑进行响应。接入和使用方式请参考[消息推送](https://developers.weixin.qq.com/miniprogram/dev/framework/server-ability/message-push.html)。



### 接收消息和事件 <a id="&#x63A5;&#x6536;&#x6D88;&#x606F;&#x548C;&#x4E8B;&#x4EF6;"></a>

在页面中使用 [`<button open-type="contact" />`](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 可以显示进入客服会话按钮。

当用户在客服会话发送消息、或由某些特定的用户操作引发事件推送时，微信服务器会将消息或事件的数据包发送到开发者填写的 URL，如果使用的是云开发，则可以推送到指定的云函数（详情请参考[消息推送](https://developers.weixin.qq.com/miniprogram/dev/framework/server-ability/message-push.html)）。开发者收到请求后可以使用 [发送客服消息](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.send.html) 接口进行异步回复。

各消息类型的推送JSON、XML数据包结构如下。

####  文本消息 <a id="&#x6587;&#x672C;&#x6D88;&#x606F;"></a>

用户在客服会话中发送文本消息时将产生如下数据包：

 **XML 格式**

```text
<xml>
   <ToUserName><![CDATA[toUser]]></ToUserName>
   <FromUserName><![CDATA[fromUser]]></FromUserName>
   <CreateTime>1482048670</CreateTime>
   <MsgType><![CDATA[text]]></MsgType>
   <Content><![CDATA[this is a test]]></Content>
   <MsgId>1234567890123456</MsgId>
</xml>
```

 **JSON 格式**

```text
{
  "ToUserName": "toUser",
  "FromUserName": "fromUser",
  "CreateTime": 1482048670,
  "MsgType": "text",
  "Content": "this is a test",
  "MsgId": 1234567890123456
}

```

 **参数说明**

| 参数 | 说明 |
| :--- | :--- |
| ToUserName | 小程序的原始ID |
| FromUserName | 发送者的openid |
| CreateTime | 消息创建时间\(整型） |
| MsgType | text |
| Content | 文本消息内容 |
| MsgId | 消息id，64位整型 |

####  图片消息 <a id="&#x56FE;&#x7247;&#x6D88;&#x606F;"></a>

用户在客服会话中发送图片消息时将产生如下数据包：

 **XML 格式**

```text
<xml>
      <ToUserName><![CDATA[toUser]]></ToUserName>
      <FromUserName><![CDATA[fromUser]]></FromUserName>
      <CreateTime>1482048670</CreateTime>
      <MsgType><![CDATA[image]]></MsgType>
      <PicUrl><![CDATA[this is a url]]></PicUrl>
      <MediaId><![CDATA[media_id]]></MediaId>
      <MsgId>1234567890123456</MsgId>
</xml>
```

 **JSON 格式**

```text
{
  "ToUserName": "toUser",
  "FromUserName": "fromUser",
  "CreateTime": 1482048670,
  "MsgType": "image",
  "PicUrl": "this is a url",
  "MediaId": "media_id",
  "MsgId": 1234567890123456
}
```

 **参数说明**

| 参数 | 说明 |
| :--- | :--- |
| ToUserName | 小程序的原始ID |
| FromUserName | 发送者的openid |
| CreateTime | 消息创建时间\(整型） |
| MsgType | image |
| PicUrl | 图片链接（由系统生成） |
| MediaId | 图片消息媒体id，可以调用\[获取临时素材\]\(\(getTempMedia\)接口拉取数据。 |
| MsgId | 消息id，64位整型 |

####  小程序卡片消息 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x5361;&#x7247;&#x6D88;&#x606F;"></a>

用户在客服会话中发送小程序卡片消息时将产生如下数据包：

 **XML 格式**

```text
<xml>
  <ToUserName><![CDATA[toUser]]></ToUserName>
  <FromUserName><![CDATA[fromUser]]></FromUserName>
  <CreateTime>1482048670</CreateTime>
  <MsgType><![CDATA[miniprogrampage]]></MsgType>
  <MsgId>1234567890123456</MsgId>
  <Title><![CDATA[Title]]></Title>
  <AppId><![CDATA[AppId]]></AppId>
  <PagePath><![CDATA[PagePath]]></PagePath>
  <ThumbUrl><![CDATA[ThumbUrl]]></ThumbUrl>
  <ThumbMediaId><![CDATA[ThumbMediaId]]></ThumbMediaId>
</xml>
```

 **JSON 格式**

```text
{
  "ToUserName": "toUser",
  "FromUserName": "fromUser",
  "CreateTime": 1482048670,
  "MsgType": "miniprogrampage",
  "MsgId": 1234567890123456,
  "Title":"title",
  "AppId":"appid",
  "PagePath":"path",
  "ThumbUrl":"",
  "ThumbMediaId":""
}
```

 **参数说明**

| 参数 | 说明 |
| :--- | :--- |
| ToUserName | 小程序的原始ID |
| FromUserName | 发送者的openid |
| CreateTime | 消息创建时间\(整型） |
| MsgType | miniprogrampage |
| MsgId | 消息id，64位整型 |
| Title | 标题 |
| AppId | 小程序appid |
| PagePath | 小程序页面路径 |
| ThumbUrl | 封面图片的临时cdn链接 |
| ThumbMediaId | 封面图片的临时素材id |

####  进入会话事件 <a id="&#x8FDB;&#x5165;&#x4F1A;&#x8BDD;&#x4E8B;&#x4EF6;"></a>

用户在小程序“客服会话按钮”进入客服会话时将产生如下数据包：

 **XML 格式**

```text
<xml>
    <ToUserName><![CDATA[toUser]]></ToUserName>
    <FromUserName><![CDATA[fromUser]]></FromUserName>
    <CreateTime>1482048670</CreateTime>
    <MsgType><![CDATA[event]]></MsgType>
    <Event><![CDATA[user_enter_tempsession]]></Event>
    <SessionFrom><![CDATA[sessionFrom]]></SessionFrom>
</xml>
```

 **JSON 格式**

```text
{
  "ToUserName": "toUser",
  "FromUserName": "fromUser",
  "CreateTime": 1482048670,
  "MsgType": "event",
  "Event": "user_enter_tempsession",
  "SessionFrom": "sessionFrom"
}
```

 **参数说明**

| 参数 | 说明 |
| :--- | :--- |
| ToUserName | 小程序的原始ID |
| FromUserName | 发送者的openid |
| CreateTime | 事件创建时间\(整型） |
| MsgType | event |
| Event | 事件类型，user\_enter\_tempsession |
| SessionFrom | 开发者在[客服会话按钮](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)设置的 session-from 属性 |

### 发送客服消息 <a id="&#x53D1;&#x9001;&#x5BA2;&#x670D;&#x6D88;&#x606F;"></a>

当用户和小程序客服产生特定动作的交互时（具体动作列表请见下方说明），微信将会把消息数据推送给开发者，开发者可以在一段时间内（目前为 48 小时）调用客服接口，通过调用 [发送客服消息接口](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.send.html) 来发送消息给普通用户。此接口主要用于客服等有人工消息处理环节的功能，方便开发者为用户提供更加优质的服务。

目前允许的动作列表如下，不同动作触发后，允许的客服接口下发消息条数和下发时限不同。

| 用户动作 | 允许下发条数限制 | 下发时限 |
| :--- | :--- | :--- |
| 用户发送消息 | 5 条 | 48 小时 |

###  转发客服消息 <a id="&#x8F6C;&#x53D1;&#x5BA2;&#x670D;&#x6D88;&#x606F;"></a>

如果小程序设置了消息推送，普通微信用户向小程序客服发消息时，微信服务器会先将消息 POST 到开发者填写的 URL 上，如果希望将消息转发到网页版客服工具，则需要开发者在响应包中返回 MsgType 为 transfer\_customer\_service 的消息，微信服务器收到响应后会把当次发送的消息转发至客服系统。

用户被客服接入以后，客服关闭会话以前，处于会话过程中时，用户发送的消息均会被直接转发至客服系统。当会话超过 30 分钟客服没有关闭时，微信服务器会自动停止转发至客服，而将消息恢复发送至开发者填写的 URL 上。

用户在等待队列中时，用户发送的消息仍然会被推送至开发者填写的 URL 上。

这里特别要注意，只针对微信用户发来的消息才进行转发，而对于其他事件（比如用户从小程序唤起客服会话）都不应该转发，否则客服在客服系统上就会看到一些无意义的消息了。

####  消息转发到网页版客服工具 <a id="&#x6D88;&#x606F;&#x8F6C;&#x53D1;&#x5230;&#x7F51;&#x9875;&#x7248;&#x5BA2;&#x670D;&#x5DE5;&#x5177;"></a>

开发者只要在响应包中返回 MsgType 为 transfer\_customer\_service 的消息，微信服务器收到响应后就会把当次发送的消息转发至客服系统。

如果是使用自有服务器接收的消息推送，则需返回如下格式的 XML 数据：

```text
<xml>
    <ToUserName><![CDATA[touser]]></ToUserName>
    <FromUserName><![CDATA[fromuser]]></FromUserName>
    <CreateTime>1399197672</CreateTime>
    <MsgType><![CDATA[transfer_customer_service]]></MsgType>
</xml>
```

参数说明

| 参数 | 是否必须 | 描述 |
| :--- | :--- | :--- |
| ToUserName | 是 | 接收方帐号（收到的OpenID） |
| FromUserName | 是 | 开发者微信号 |
| CreateTime | 是 | 消息创建时间 （整型） |
| MsgType | 是 | transfer\_customer\_service |

如果是使用[云函数接收的消息推送](https://developers.weixin.qq.com/miniprogram/dev/framework/server-ability/message-push.html#option-cloud)，则需在云函数被客服消息触发后返回同样格式的 `JSON` 数据：

```text
// ...
exports.main = async (event, context) => {
  // 判断处理客服消息 ...
  // 最后返回 JSON
  return {
    MsgType: 'transfer_customer_service',
    ToUserName: 'touser',
    FromUserName: 'fromuser',
    CreateTime: parseInt(+new Date / 1000),
  }
}
```

### 客服输入状态 <a id="&#x5BA2;&#x670D;&#x8F93;&#x5165;&#x72B6;&#x6001;"></a>

开发者可通过调用 [客服输入状态接口](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.setTyping.html)，返回客服当前输入状态给用户。

1. 此接口需要客服消息接口权限。
2. 如果不满足发送客服消息的触发条件，则无法下发输入状态。
3. 下发输入状态，需要客服之前 30 秒内跟用户有过消息交互。
4. 在输入状态中（持续 15 秒），不可重复下发输入态。
5. 在输入状态中，如果向用户下发消息，会同时取消输入状态。

### 在客服消息中使用临时素材 <a id="&#x5728;&#x5BA2;&#x670D;&#x6D88;&#x606F;&#x4E2D;&#x4F7F;&#x7528;&#x4E34;&#x65F6;&#x7D20;&#x6750;"></a>

开发者可在接收和发送客服消息的过程中获取或上传临时素材。

####  获取临时素材 <a id="&#x83B7;&#x53D6;&#x4E34;&#x65F6;&#x7D20;&#x6750;"></a>

接收到用户消息之后，可通过 [获取临时素材接口](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.getTempMedia.html) 获取消息中的临时素材

####  上传临时素材 <a id="&#x4E0A;&#x4F20;&#x4E34;&#x65F6;&#x7D20;&#x6750;"></a>

通过 [上传临时素材接口](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.uploadTempMedia.html) 可以上传临时素材，并在 [发送消息接口](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.send.html) 中使用。

### 位置消息 <a id="&#x4F4D;&#x7F6E;&#x6D88;&#x606F;"></a>

> 微信客户端 7.0.9 及以上版本支持，iOS 暂不支持

为了让用户更便 捷地使用小程序的打车服务，我们在位置消息详情页的菜单中，新增了打车小程序入口。

1. 打开聊天中的位置消息，点击详情页右下角绿色按钮，菜单中会展示符合条件的打车小程序，用户可以直接发起目的地为该位置的打车服务。
2. 小程序的注册类目为“打车（网约车）”，且有用户最近使用的记录，才可以出现在该菜单中。
3. 在此处点击打开小程序后，需要直接进入到发起打车页面。

####  1. 位置消息入口声明 <a id="_1-&#x4F4D;&#x7F6E;&#x6D88;&#x606F;&#x5165;&#x53E3;&#x58F0;&#x660E;"></a>

开发者需要在全局配置`app.json`声明支持从位置消息入口进入小程序。

配置示例：

```text
"entranceDeclare": {
    "locationMessage": {
        "path": "pages/index/index",
        "query": "foo=bar"
    }
}
```

**配置项**

| 属性 | 类型 | 必填 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- | :--- |
| entranceDeclare | Object | 否 | 入口声明信息 | 7.0.9 |

entranceDeclare参数列表

| 属性 | 类型 | 必填 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- | :--- |
| locationMessage | Object | 否 | 声明“位置消息”场景进入小程序的启动页面 | 7.0.9 |

locationMessage参数列表

| 属性 | 类型 | 必填 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- | :--- |
| path | string | 否 | 启动页路径，必须是在pages中已经定义 | 7.0.9 |
| query | string | 否 | 启动页参数 | 7.0.9 |

####  2. 从启动参数获取位置信息 <a id="_2-&#x4ECE;&#x542F;&#x52A8;&#x53C2;&#x6570;&#x83B7;&#x53D6;&#x4F4D;&#x7F6E;&#x4FE1;&#x606F;"></a>

示例代码：

```text
//app.js
App({
  onLaunch: function (options){
    console.log(options)
    var scene = options.scene 
    if (scene == 1146) { // 位置消息场景值
      var location = options.locationInfo
      var x = location.latitude
      var y = location.longitude
      var name = location.name
    }
  },
})
```

**Object** 启动参数

| 属性 | 类型 | 描述 |
| :--- | :--- | :--- |
| scene | number | 启动小程序的场景值，“位置消息”的启动场景值为1146 |
| locationInfo | Object | 特殊场景的启动信息 |

**locationInfo** 的结构

| 属性 | 类型 | 描述 |
| :--- | :--- | :--- |
| latitude | number | 纬度，范围为 -90~90，负数表示南纬 |
| longtitude | number | 经度，范围为 -180~180，负数表示西经 |
| name | string | POI名称 |

####  3. 工具调试 <a id="_3-&#x5DE5;&#x5177;&#x8C03;&#x8BD5;"></a>

Nightly v1.02.1912062 版本已支持条件编译增加位置消息入口。选择场景值 1146: 位置消息中用小程序打车，传入POI点名称和经纬度信息后可用真机预览调试。



