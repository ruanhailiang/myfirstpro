# 服务端能力

### 后端 API <a id="&#x540E;&#x7AEF;-API"></a>

小程序还提供了一系列在后端服务器使用 HTTPS 请求调用的 API，帮助开发者在后台完成各类数据分析、管理和查询等操作。如 `getAccessToken`，`code2Session` 等。详细介绍请参考 [API 文档](https://developers.weixin.qq.com/miniprogram/dev/api/index.html)。

####  access\_token <a id="access-token"></a>

`access_token` 是小程序全局唯一后台接口调用凭据，调用绝大多数后台接口时都需使用。开发者可以通过 `getAccessToken` 接口获取并进行妥善保存。

为了 `access_token` 的安全性，**后端 API 不能直接在小程序内通过** [**wx.request**](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html) **调用**，即 `api.weixin.qq.com` 不能被配置为服务器域名。开发者应在后端服务器使用`getAccessToken`获取 `access_token`，并调用相关 API；

####  请求参数说明 <a id="&#x8BF7;&#x6C42;&#x53C2;&#x6570;&#x8BF4;&#x660E;"></a>

* 对于 GET 请求，请求参数应以 QueryString 的形式写在 URL 中。
* 对于 POST 请求，部分参数需以 QueryString 的形式写在 URL 中（一般只有 `access_token`，如有额外参数会在文档里的 URL 中体现），其他参数如无特殊说明均以 JSON 字符串格式写在 POST 请求的 body 中。

####  返回参数说明 <a id="&#x8FD4;&#x56DE;&#x53C2;&#x6570;&#x8BF4;&#x660E;"></a>

**注意：当API调用成功时，部分接口不会返回 errcode 和 errmsg，只有调用失败时才会返回。**

\*\*\*\*

### 消息推送 <a id="&#x6D88;&#x606F;&#x63A8;&#x9001;"></a>

接入微信小程序消息推送服务，可以两种方式选择其一：

1. [开发者服务器接收消息推送](https://developers.weixin.qq.com/miniprogram/dev/framework/server-ability/message-push.html#option-url)
2. [云函数接收消息推送](https://developers.weixin.qq.com/miniprogram/dev/framework/server-ability/message-push.html#option-cloud)

####  开发者服务器接收消息推送 <a id="&#x5F00;&#x53D1;&#x8005;&#x670D;&#x52A1;&#x5668;&#x63A5;&#x6536;&#x6D88;&#x606F;&#x63A8;&#x9001;"></a>

开发者需要按照如下步骤完成：

1. 填写服务器配置
2. 验证服务器地址的有效性
3. 据接口文档实现业务逻辑，接收消息和事件

 **第一步：填写服务器配置**

登录[小程序后台](https://mp.weixin.qq.com/)后，在「开发」-「开发设置」-「消息推送」中，管理员扫码启用消息服务，填写服务器地址（URL）、令牌（Token） 和 消息加密密钥（EncodingAESKey）等信息。

* URL: 开发者用来接收微信消息和事件的接口 URL。开发者所填写的URL 必须以 http:// 或 https:// 开头，分别支持 80 端口和 443 端口。
* Token: 可由开发者可以任意填写，用作生成签名（该 Token 会和接口 URL 中包含的 Token 进行比对，从而验证安全性）。
* EncodingAESKey: 由开发者手动填写或随机生成，将用作消息体加解密密钥。

同时，开发者可选择消息加解密方式：明文模式（默认）、兼容模式和安全模式。可以选择消息数据格式：XML 格式（默认）或 JSON 格式。

![&#x586B;&#x5199;&#x670D;&#x52A1;&#x5668;&#x914D;&#x7F6E;](https://res.wx.qq.com/wxdoc/dist/assets/img/callback_help.ae01307b.png)

模式的选择与服务器配置在提交后都会立即生效，请开发者谨慎填写及选择。切换加密方式和数据格式需要提前配置好相关代码，详情请参考 [消息加解密说明](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419318479&token=&lang=zh_CN)。

 **第二步：验证消息的确来自微信服务器**

开发者提交信息后，微信服务器将发送GET请求到填写的服务器地址URL上，GET请求携带参数如下表所示：

| 参数 | 描述 |
| :--- | :--- |
| signature | 微信加密签名，signature结合了开发者填写的token参数和请求中的timestamp参数、nonce参数。 |
| timestamp | 时间戳 |
| nonce | 随机数 |
| echostr | 随机字符串 |

开发者通过检验 signature 对请求进行校验（下面有校验方式）。若确认此次 GET 请求来自微信服务器，请原样返回 echostr 参数内容，则接入生效，成为开发者成功，否则接入失败。加密/校验流程如下：

1. 将token、timestamp、nonce三个参数进行字典序排序
2. 将三个参数字符串拼接成一个字符串进行sha1加密
3. 开发者获得加密后的字符串可与signature对比，标识该请求来源于微信

验证URL有效性成功后即接入生效，成为开发者。

检验signature的PHP示例代码：

```text
private function checkSignature()
{
    $signature = $_GET["signature"];
    $timestamp = $_GET["timestamp"];
    $nonce = $_GET["nonce"];

    $token = TOKEN;
    $tmpArr = array($token, $timestamp, $nonce);
    sort($tmpArr, SORT_STRING);
    $tmpStr = implode( $tmpArr );
    $tmpStr = sha1( $tmpStr );

    if ($tmpStr == $signature ) {
        return true;
    } else {
        return false;
    }
}

```

PHP示例代码下载：[下载](https://wximg.gtimg.com/shake_tv/mpwiki/cryptoDemo.zip)

 **第三步：接收消息和事件**

当某些特定的用户操作引发事件推送时（如用户向小程序客服发送消息、或者进入会话等情况），微信服务器会将消息（或事件）的数据包以 POST 请求发送到开发者配置的 URL，开发者可以依据自身业务逻辑进行响应。

微信服务器在将用户的消息发给开发者服务器地址后，微信服务器在五秒内收不到响应会断掉连接，并且重新发起请求，总共重试三次。如果在调试中，发现用户无法收到响应的消息，可以检查是否消息处理超时。关于重试的消息排重，有 msgid 的消息推荐使用 msgid 排重。事件类型消息推荐使用 FromUserName + CreateTime 排重。

服务器收到请求必须做出下述回复，这样微信服务器才不会对此作任何处理，并且不会发起重试，否则，将出现严重的错误提示。详见下面说明：

1. 直接回复success（推荐方式）
2. 直接回复空串（指字节长度为0的空字符串，而不是结构体中content字段的内容为空）
3. 若接口文档有指定返回内容，应按文档说明返回

对于客服消息，一旦遇到以下情况，微信会在小程序会话中向用户下发系统提示“该小程序客服暂时无法提供服务，请稍后再试”：

1. 开发者在5秒内未回复任何内容
2. 开发者回复了异常数据

如果开发者希望增强安全性，可以在开发者中心处开启消息加密，这样，用户发给小程序的消息以及小程序被动回复用户消息都会继续加密，详见[消息加解密说明](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419318479&token=&lang=zh_CN)。

####  云函数接收消息推送 <a id="&#x4E91;&#x51FD;&#x6570;&#x63A5;&#x6536;&#x6D88;&#x606F;&#x63A8;&#x9001;"></a>

> 需开发者工具版本至少 `1.02.1906252`

开通了[云开发](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html)的小程序可以使用云函数接收消息推送，目前仅支持客服消息推送。

接入步骤如下：

1. 开发者工具中填写配置并上传
2. 云函数中处理消息

 **第一步：开发者工具云开发控制台中增加配置**

打开云开发控制台，到设置 tab 中选择全局设置 - 添加消息推送配置。消息类型对应收包的 `MsgType`，事件类型对应收包的 `Event`，同一个 `<消息类型, 事件类型>` 二元组只能推到一个环境的一个云函数。例如客服消息文本消息对应的就是消息类型为 `text`，事件类型为空。具体值请查看各个消息的消息格式。

 **第二步：云函数中处理消息**

云函数被触发时，其 `event` 参数即是接口所定义的 JSON 结构的对象（统一 `JSON` 格式，不支持 `XML` 格式）。

以客服消息为例，接收到客服消息推送时，`event` 结构如下：

```text
{
  "FromUserName": "ohl4L0Rnhq7vmmbT_DaNQa4ePaz0",
  "ToUserName": "wx3d289323f5900f8e",
  "Content": "测试",
  "CreateTime": 1555684067,
  "MsgId": "49d72d67b16d115e7935ac386f2f0fa41535298877_1555684067",
  "MsgType": "text"
}
```

此时可调用客服消息[发送](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.send.html)接口回复消息，一个简单的接收到消息后统一回复 “收到” 的示例如下：

```text
// 云函数入口文件
const cloud = require('wx-server-sdk')

cloud.init()

// 云函数入口函数
exports.main = async (event, context) => {
  const wxContext = cloud.getWXContext()
  
  await cloud.openapi.customerServiceMessage.send({
    touser: wxContext.OPENID,
    msgtype: 'text',
    text: {
      content: '收到',
    },
  })

  return 'success'
}
```

