# 开发插件

## 开发插件 <a id="&#x5F00;&#x53D1;&#x63D2;&#x4EF6;"></a>

开发插件前，请阅读了解[《小程序插件接入指南》](https://developers.weixin.qq.com/miniprogram/introduction/plugin.html)了解开通流程及开放范围，并开通插件功能。如果未开通插件功能，将无法上传插件。

###  创建插件项目 <a id="&#x521B;&#x5EFA;&#x63D2;&#x4EF6;&#x9879;&#x76EE;"></a>

插件类型的项目可以在开发者工具中直接创建。[详情](https://developers.weixin.qq.com/miniprogram/dev/devtools/plugin.html)

![&#x521B;&#x5EFA;&#x63D2;&#x4EF6;](https://res.wx.qq.com/wxdoc/dist/assets/img/createplugin.fc7a49e7.png)

新建插件类型的项目后，如果创建示例项目，则项目中将包含三个目录：

* `plugin` 目录：插件代码目录。
* `miniprogram` 目录：放置一个小程序，用于调试插件。
* `doc` 目录：用于放置插件开发文档。

`miniprogram` 目录内容可以当成普通小程序来编写，用于插件调试、预览和审核。下面的内容主要介绍 `plugin` 中的插件代码及 `doc` 中的插件开发文档。

我们提供了[一个可以直接在微信开发者工具中查看的完整插件示例](https://developers.weixin.qq.com/s/NrPCBmmT7B1B)，开发者可以和本文互相对照以便理解。请注意：

1. 由于插件需要 appid 才能工作，请填入一个 appid；
2. 由于当前代码片段的限制，打开该示例后请 **手动将 appid 填写到 `miniprogram/app.json` 中（如下图）使示例正常运行。**

![&#x624B;&#x52A8;&#x586B;&#x5199; appid](https://res.wx.qq.com/wxdoc/dist/assets/img/plugin_minicode_guide.09990255.png)

###  插件目录结构 <a id="&#x63D2;&#x4EF6;&#x76EE;&#x5F55;&#x7ED3;&#x6784;"></a>

一个插件可以包含若干个自定义组件、页面，和一组 js 接口。插件的目录内容如下：

```text
plugin
├── components
│   ├── hello-component.js   // 插件提供的自定义组件（可以有多个）
│   ├── hello-component.json
│   ├── hello-component.wxml
│   └── hello-component.wxss
├── pages
│   ├── hello-page.js        // 插件提供的页面（可以有多个，自小程序基础库版本 2.1.0 开始支持）
│   ├── hello-page.json
│   ├── hello-page.wxml
│   └── hello-page.wxss
├── index.js                 // 插件的 js 接口
└── plugin.json              // 插件配置文件
```

###  插件配置文件 <a id="&#x63D2;&#x4EF6;&#x914D;&#x7F6E;&#x6587;&#x4EF6;"></a>

向第三方小程序开放的所有自定义组件、页面和 js 接口都必须在插件配置文件 `plugin.json` 列出，格式如下：

**代码示例：**

```text
{
  "publicComponents": {
    "hello-component": "components/hello-component"
  },
  "pages": {
    "hello-page": "pages/hello-page"
  },
  "main": "index.js"
}
```

这个配置文件将向第三方小程序开放一个自定义组件 `hello-component`，一个页面 `hello-page` 和 `index.js` 下导出的所有 js 接口。

###  进行插件开发 <a id="&#x8FDB;&#x884C;&#x63D2;&#x4EF6;&#x5F00;&#x53D1;"></a>

请注意：在插件开发中，只有[部分接口](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/api-limit.html)可以直接调用；另外还有部分能力（如 获取用户信息 和 发起支付 等）可以通过[插件功能页](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages.html)的方式使用。

####  自定义组件 <a id="&#x81EA;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;"></a>

插件可以定义若干个自定义组件，这些自定义组件都可以在插件内相互引用。但提供给第三方小程序使用的自定义组件必须在配置文件的 `publicComponents` 段中列出（参考上文）。

除去接口限制以外，自定义组件的编写和组织方式与一般的自定义组件相同，每个自定义组件由 `wxml`, `wxss`, `js` 和 `json` 四个文件组成。具体可以参考[自定义组件的文档](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)。

####  页面 <a id="&#x9875;&#x9762;"></a>

插件从小程序基础库版本 [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始支持页面。插件可以定义若干个插件页面，可以从本插件的自定义组件、其他页面中跳转，或从第三方小程序中跳转。所有页面必须在配置文件的 `pages` 段中列出（参考上文）。

除去接口限制以外，插件的页面编写和组织方式与一般的页面相同，每个页面由 `wxml`, `wxss`, `js` 和 `json` 四个文件组成。具体可以参考其他关于页面的文档。

插件执行页面跳转的时候，可以使用 `navigator` 组件。当插件跳转到自身页面时， `url` 应设置为这样的形式：`plugin-private://PLUGIN_APPID/PATH/TO/PAGE` 。需要跳转到其他插件时，也可以这样设置 `url` 。

**代码示例：**

```text
<navigator url="plugin-private://wxidxxxxxxxxxxxxxx/pages/hello-page">
  Go to pages/hello-page!
</navigator>
```

自基础库版本 [2.2.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，在插件自身的页面中，插件还可以调用 [wx.navigateTo](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.navigateTo.html) 来进行页面跳转， `url` 格式与使用 `navigator` 组件时相仿。

####  接口 <a id="&#x63A5;&#x53E3;"></a>

插件可以在接口文件（在配置文件中指定，详情见上文）中 export 一些 js 接口，供插件的使用者调用，如：

**代码示例：**

```text
module.exports = {
  hello: function() {
    console.log('Hello plugin!')
  }
}
```

###  预览、上传和发布 <a id="&#x9884;&#x89C8;&#x3001;&#x4E0A;&#x4F20;&#x548C;&#x53D1;&#x5E03;"></a>

插件可以像小程序一样预览和上传，但插件没有体验版。

插件会同时有多个线上版本，由使用插件的小程序决定具体使用的版本号。

手机预览和提审插件时，会使用一个特殊的小程序来套用项目中 `miniprogram` 文件夹下的小程序，从而预览插件。

* （建议的方式）如果当前开发者有[测试号](https://developers.weixin.qq.com/miniprogram/dev/devtools/sandbox.html)，则会使用这个测试号；在测试号的设置页中可以看到测试号的 `appid` 、 `appsecret` 并设置域名列表。
* 否则，将使用“插件开发助手”，它具有一个特定的 `appid` 。

###  在开发版小程序中测试 <a id="&#x5728;&#x5F00;&#x53D1;&#x7248;&#x5C0F;&#x7A0B;&#x5E8F;&#x4E2D;&#x6D4B;&#x8BD5;"></a>

通常情况下，可以将 `miniprogram` 下的代码当做使用插件的小程序代码，来进行插件的调试和测试。

但有时，需要将插件的代码放在实际运行的小程序中进行调试、测试。此时，可以使用开发版的小程序直接引用开发版插件。方法如下：

1. 在开发者工具的插件项目中上传插件，此时，在上传成功的通知信息中将包含这次上传获得的插件开发版 ID （一个英文、数字组成的随机字符串）；
2. 点击开发者工具右下角的通知按钮，可以打开通知栏，看到新生成的 ID ；
3. 在使用这个插件的任意小程序项目中，可以将插件 version 设置为 `"version": "dev-[开发版ID]"` 的形式，如 `"version": "dev-abcdef0123456789abcdef0123456789"` ；
4. 这样就会引用到这次上传的开发版插件；
5. 注意，再次上传插件时， ID 可能会改变。

如果开发版小程序引用了开发版插件，此时这个小程序就不能上传发布了。必须要将插件版本设为正式版本之后，小程序才可以正常上传、发布。

###  插件开发文档 <a id="&#x63D2;&#x4EF6;&#x5F00;&#x53D1;&#x6587;&#x6863;"></a>

在第三方小程序使用插件时，插件代码并不可见。因此，除了插件代码，我们还支持插件开发者上传一份插件开发文档。这份开发文档将展示在插件详情页，供其他开发者在浏览插件和使用插件时进行阅读和参考。插件开发者应在插件开发文档中对插件提供的自定义组件、页面、接口等进行必要的描述和解释，方便第三方小程序正确使用插件。

插件开发文档必须放置在插件项目根目录中的 `doc` 目录下，目录结构如下：

```text
doc
├── README.md   // 插件文档，应为 markdown 格式
└── picture.jpg // 其他资源文件，仅支持图片
```

其中，`README.md` 的编写有一定的 **限制条件**，具体来说：

1. 引用到的图片资源不能是网络图片，且必须放在这个目录下；
2. 文档中的链接只能链接到：
   * 微信开发者社区（developers.weixin.qq.com）
   * 微信公众平台（mp.weixin.qq.com）
   * GitHub（github.com）

编辑 `README.md` 之后，可以使用开发者工具打开 `README.md`，并在编辑器的右下角预览插件文档和单独上传插件文档。发布上传文档后，文档不会立刻发布。此时可以使用帐号和密码登录 [管理后台](https://mp.weixin.qq.com) ，在 小程序插件 &gt; 基本设置 中预览、发布插件文档。

###  其他注意事项 <a id="&#x5176;&#x4ED6;&#x6CE8;&#x610F;&#x4E8B;&#x9879;"></a>

####  插件间互相调用 <a id="&#x63D2;&#x4EF6;&#x95F4;&#x4E92;&#x76F8;&#x8C03;&#x7528;"></a>

插件不能直接引用其他插件。但如果小程序引用了多个插件，插件之间是可以互相调用的。

一个插件调用另一个插件的方法，与插件调用自身的方法类似。可以使用 `plugin-private://APPID` 访问插件的自定义组件、页面（暂不能使用 `plugin://` ）。

对于 js 接口，可使用 `requirePlugin` ，但目前尚不能在文件一开头就使用 requirePlugin ，因为被依赖的插件可能还没有初始化，请考虑在更晚的时机调用 `requirePlugin` ，如接口被实际调用时、组件 attached 时。（未来会修复这个问题。）

####  插件请求签名 <a id="&#x63D2;&#x4EF6;&#x8BF7;&#x6C42;&#x7B7E;&#x540D;"></a>

插件在使用 [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html) 等 API 发送网络请求时，将会额外携带一个签名 `HostSign` ，用于验证请求来源于小程序插件。这个签名位于请求头中，形如：

```text
X-WECHAT-HOSTSIGN: {"noncestr":"NONCESTR", "timestamp":"TIMESTAMP", "signature":"SIGNATURE"}
```

其中， `NONCESTR` 是一个随机字符串， `TIMESTAMP` 是生成这个随机字符串和 `SIGNATURE` 的 UNIX 时间戳。它们是用于计算签名 `SIGNATRUE` 的参数，签名算法为：

```text
SIGNATURE = sha1([APPID, NONCESTR, TIMESTAMP, TOKEN].sort().join(''))
```

其中，`APPID` 是 **所在小程序** 的 AppId （可以从请求头的 `referrer` 中获得）；`TOKEN` 是插件 Token，可以在小程序插件基本设置中找到。

网络请求的 referer 格式固定为 https://servicewechat.com/{appid}/{version}/page-frame.html，其中 {appid} 为小程序的 appid，{version} 为小程序的版本号，版本号为 0 表示为开发版、体验版以及审核版本，版本号为 devtools 表示为开发者工具，其余为正式版本。

插件开发者可以在服务器上按以下步骤校验签名：

1. `sort` 对 `APPID` `NONCESTR` `TIMESTAMP` `TOKEN` 四个值表示成字符串形式，按照字典序排序（同 JavaScript 数组的 sort 方法）；
2. `join` 将排好序的四个字符串直接连接在一起；
3. 对连接结果使用 `sha1` 算法，其结果即 `SIGNATURE` 。

自基础库版本 [2.0.7](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，在小程序运行期间，若网络状况正常， `NONCESTR` 和 `TIMESTAMP` 会每 10 分钟变更一次。如有必要，可以通过判断 `TIMESTAMP` 来确定当前签名是否依旧有效。

