# 用户信息功能页

## 用户信息功能页 <a id="&#x7528;&#x6237;&#x4FE1;&#x606F;&#x529F;&#x80FD;&#x9875;"></a>

用户信息功能页用于帮助插件获取用户信息，包括 `openid` 和昵称等，相当于 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 和 [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html) 的功能。

此外，自基础库版本 [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，用户在这个功能页中授权之后，插件就可以直接调用 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 和 [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserInfo.html) 。无需再次进入功能页获取用户信息。自基础库版本 [2.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，可以使用 [wx.getSetting](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/setting/wx.getSetting.html) 来查询用户是否授权过。

###  调用参数 <a id="&#x8C03;&#x7528;&#x53C2;&#x6570;"></a>

用户信息功能页使用 [functional-page-navigator](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html) 进行跳转时，对应的参数 name 应为固定值 `loginAndGetUserInfo`，其余参数与 [wx.getUserInfo](https://developers.weixin.qq.com/miniprogram/dev/api/open.html#wxgetuserinfoobject) 相同，具体来说：

**args参数说明：**

| 参数名 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| withCredentials | Boolean | 否 | 是否带上登录态信息 |
| lang | String | 否 | 指定返回用户信息的语言，zh\_CN 简体中文，zh\_TW 繁体中文，en 英文。默认为en。 |
| timeout | Number | 否 | 超时时间，单位 ms |

**注：当 withCredentials 为 true 时，返回的数据会包含 encryptedData, iv 等敏感信息。**

**bindsuccess返回参数说明：**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| code | String | 同 [wx.login](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html) 获得的用户登录凭证（有效期五分钟）。开发者需要在开发者服务器后台调用 api，使用 code 换取 openid 和 session\_key 等信息 |
| errMsg | String | 调用结果 |
| userInfo | OBJECT | 用户信息对象，不包含 openid 等敏感信息 |
| rawData | String | 不包括敏感信息的原始数据字符串，用于计算签名。 |
| signature | String | 使用 sha1\( rawData + sessionkey \) 得到字符串，用于校验用户信息，参考文档 [signature](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html)。 |
| encryptedData | String | 包括敏感数据在内的完整用户信息的加密数据，详细见[加密数据解密算法](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html) |
| iv | String | 加密算法的初始向量，详细见[加密数据解密算法](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html) |

**userInfo参数说明：**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| nickName | String | 用户昵称 |
| avatarUrl | String | 用户头像，最后一个数值代表正方形头像大小（有0、46、64、96、132数值可选，0代表132\*132正方形头像），用户没有头像时该项为空。若用户更换头像，原有头像URL将失效。 |
| gender | String | 用户的性别，值为1时是男性，值为2时是女性，值为0时是未知 |
| city | String | 用户所在城市 |
| province | String | 用户所在省份 |
| country | String | 用户所在国家 |
| language | String | 用户的语言，简体中文为zh\_CN |

**代码示例：**

```text
<!--plugin/components/hello-component.wxml-->
  <functional-page-navigator
    name="loginAndGetUserInfo"
    args="{{ args }}"
    version="develop"
    bind:success="loginSuccess"
    bind:fail="loginFail"
  >
    <button class="login">登录到插件</button>
  </functional-page-navigator>
```

```text
// plugin/components/hello-component.js
Component({
  properties: {},
  data: {
    args: {
      withCredentials: true,
      lang: 'zh_CN'
    }
  },
  methods: {
    loginSuccess: function (res) {
      console.log(res.detail);
    },
    loginFail: function (res) {
      console.log(res);
    }
  }
});
```

用户点击该 `navigator` 后，将跳转到如下的用户信息功能页：

![&#x7528;&#x6237;&#x4FE1;&#x606F;&#x529F;&#x80FD;&#x9875;](https://res.wx.qq.com/wxdoc/dist/assets/img/user-info-functional-page.db699ed9.png)

[在微信开发者工具中查看示例](https://developers.weixin.qq.com/s/Uof4Iomt731Z)：

1. 由于插件需要 appid 才能工作，请填入一个 appid；
2. 由于当前代码片段的限制，打开该示例后请 **手动将 appid 填写到 `miniprogram/app.json` 中（如下图）使示例正常运行。**

![&#x624B;&#x52A8;&#x586B;&#x5199; appid](https://res.wx.qq.com/wxdoc/dist/assets/img/plugin_minicode_guide.09990255.png)

