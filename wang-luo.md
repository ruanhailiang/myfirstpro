# 网络



### 网络 <a id="&#x7F51;&#x7EDC;"></a>

在小程序/小游戏中使用网络相关的 API 时，需要注意下列问题，请开发者提前了解。

####  1. 服务器域名配置 <a id="_1-&#x670D;&#x52A1;&#x5668;&#x57DF;&#x540D;&#x914D;&#x7F6E;"></a>

每个微信小程序需要事先设置通讯域名，小程序**只可以跟指定的域名与进行网络通信**。包括普通 HTTPS 请求（[wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)）、上传文件（[wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html)）、下载文件（[wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/download/wx.downloadFile.html)\) 和 WebSocket 通信（[wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.connectSocket.html)）。

从基础库 2.4.0 开始，网络接口允许与局域网 IP 通信，但要注意 **不允许与本机 IP 通信**。

从 2.7.0 开始，提供了 UDP 通信（[wx.createUDPSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/udp/wx.createUDPSocket.html)\)。

 **配置流程**

服务器域名请在 「小程序后台-开发-开发设置-服务器域名」 中进行配置，配置时需要注意：

* 域名只支持 `https` \([wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)、[wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html)、[wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/download/wx.downloadFile.html)\) 和 `wss` \([wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.connectSocket.html)\) 协议；
* 域名不能使用 IP 地址（小程序的[局域网](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/mDNS.html) IP 除外）或 localhost；
* 可以配置端口，如 https://myserver.com:8080，但是配置后只能向 https://myserver.com:8080 发起请求。如果向 https://myserver.com、https://myserver.com:9091 等 URL 请求则会失败。
* 如果不配置端口。如 https://myserver.com，那么请求的 URL 中也不能包含端口，甚至是默认的 443 端口也不可以。如果向 https://myserver.com:443 请求则会失败。
* 域名必须经过 ICP 备案；
* **出于安全考虑，`api.weixin.qq.com` 不能被配置为服务器域名，相关API也不能在小程序内调用。** 开发者应将 AppSecret 保存到后台服务器中，通过服务器使用 `getAccessToken` 接口获取 `access_token`，并调用相关 API；
* 对于每个接口，分别可以配置最多 20 个域名。

####  2. 网络请求 <a id="_2-&#x7F51;&#x7EDC;&#x8BF7;&#x6C42;"></a>

 **超时时间**

* 默认超时时间和最大超时时间都是 **60s**；
* 超时时间可以在 `app.json` 或 `game.json` 中通过 [`networktimeout`](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html) 配置。

 **使用限制**

* 网络请求的 `referer` header 不可设置。其格式固定为 `https://servicewechat.com/{appid}/{version}/page-frame.html`，其中 `{appid}` 为小程序的 appid，`{version}` 为小程序的版本号，版本号为 `0` 表示为开发版、体验版以及审核版本，版本号为 `devtools` 表示为开发者工具，其余为正式版本；
* [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)、[wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html)、[wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/download/wx.downloadFile.html) 的最大并发限制是 **10** 个；
* [wx.connectSockt](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/%28wx.connectSockt%29) 的最大并发限制是 **5** 个。
* 小程序进入后台运行后，如果 **5s** 内网络请求没有结束，会回调错误信息 `fail interrupted`；在回到前台之前，网络请求接口调用都会无法调用。

 **返回值编码**

* 建议服务器返回值使用 **UTF-8** 编码。对于非 UTF-8 编码，小程序会尝试进行转换，但是会有转换失败的可能。
* 小程序会自动对 BOM 头进行过滤（只过滤一个BOM头）。

 **回调函数**

* **只要成功接收到服务器返回，无论 `statusCode` 是多少，都会进入 `success` 回调。请开发者根据业务逻辑对返回值进行判断。**

####  3. 常见问题 <a id="_3-&#x5E38;&#x89C1;&#x95EE;&#x9898;"></a>

 **HTTPS 证书**

**小程序必须使用 HTTPS/WSS 发起网络请求**。请求时系统会对服务器域名使用的 HTTPS 证书进行校验，如果校验失败，则请求不能成功发起。由于系统限制，不同平台对于证书要求的严格程度不同。为了保证小程序的兼容性，建议开发者按照最高标准进行证书配置，并使用相关工具检查现有证书是否符合要求。

对证书要求如下：

* HTTPS 证书必须有效；
  * 证书必须被系统信任，即根证书被已系统内置
  * 部署 SSL 证书的网站域名必须与证书颁发的域名一致
  * 证书必须在有效期内
  * 证书的信任链必需完整（需要服务器配置）
* `iOS` 不支持自签名证书;
* `iOS` 下证书必须满足苹果 [App Transport Security \(ATS\)](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW33) 的要求;
* TLS 必须支持 1.2 及以上版本。部分旧 `Android` 机型还未支持 TLS 1.2，请确保 HTTPS 服务器的 TLS 版本支持 1.2 及以下版本;
* 部分 CA 可能不被操作系统信任，请开发者在选择证书时注意小程序和各系统的相关通告。
  * [Chrome 56/57 内核对 WoSign、StartCom 证书限制周知](https://developers.weixin.qq.com/community/develop/doc/800026caeb042e45681583652b70910a)

> 证书有效性可以使用 `openssl s_client -connect example.com:443` 命令验证，也可以使用其他[在线工具](https://myssl.com/ssl.html)。

**除了网络请求 API 外，小程序中其他 `HTTPS` 请求如果出现异常，也请按上述流程进行检查。如 https 的图片无法加载、音视频无法播放等。**

 **跳过域名校验**

在微信开发者工具中，可以临时开启 `开发环境不校验请求域名、TLS版本及HTTPS证书` 选项，跳过服务器域名的校验。此时，在微信开发者工具中及手机开启调试模式时，不会进行服务器域名的校验。

**在服务器域名配置成功后，建议开发者关闭此选项进行开发，并在各平台下进行测试，以确认服务器域名配置正确。**

> 如果手机上出现 “打开调试模式可以发出请求，关闭调试模式无法发出请求” 的现象，请确认是否跳过了域名校验，并确认服务器域名和证书配置是否正确。

### 局域网通信 <a id="&#x5C40;&#x57DF;&#x7F51;&#x901A;&#x4FE1;"></a>

基础库 2.4.0 提供了 [wx.startLocalServiceDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/network/mdns/wx.startLocalServiceDiscovery.html) 等一系列 mDNS API，可以用来获取局域网内提供 mDNS 服务的设备的 IP。 [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)/[wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.connectSocket.html)/[wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html)/[wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/download/wx.downloadFile.html) 的 url 参数允许为 `${IP}:${PORT}/${PATH}` 的格式，当且仅当 IP 与手机 IP 处在同一网段且不与本机 IP 相同（一般来说，就是同一局域网，如连接在同一个 wifi 下）时，请求/连接才会成功。

在这种情况下，不会进行安全域的校验，不要求必须使用 https/wss，也可以使用 http/ws。

```text
wx.request({
  url: 'http://10.9.176.40:828'
  // 省略其他参数
})

wx.connectSocket({
  url: 'ws://10.9.176.42:828'
  // 省略其他参数
})
```

基础库 2.7.0 开始，提供了 [wx.createUDPSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/udp/wx.createUDPSocket.html) 接口用于进行 UDP 通信。通信规则同上，仅允许同一局域网下的非本机 IP。

####  mDNS <a id="mDNS"></a>

目前小程序只支持通过 mDNS 协议获取局域网内其他设备的 IP。iOS 上 mDNS API 的实现基于 [Bonjour](https://developer.apple.com/bonjour/)，Android 上则是基于 [Android 系统接口](https://developer.android.com/training/connect-devices-wirelessly/nsd)。

**serviceType**

发起 mDNS 服务搜索 [wx.startLocalServiceDiscovery](https://developers.weixin.qq.com/miniprogram/dev/api/network/mdns/wx.startLocalServiceDiscovery.html) 的接口有 serviceType 参数，指定要搜索的服务类型。

serviceType 的格式和规范，iOS [Bonjour Overview](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/NetServices/Articles/domainnames.html) 在 **Bonjour Names for Existing Service Types** 有提及。

![Bonjour serviceType.png](https://res.wx.qq.com/wxdoc/dist/assets/img/bonjour_service_type.a49156e7.png)

[Android 文档](https://developer.android.com/training/connect-devices-wirelessly/nsd) 对此也有提及。

![Android serviceType.png](https://res.wx.qq.com/wxdoc/dist/assets/img/android_service_type.4d2ed6c9.png)

