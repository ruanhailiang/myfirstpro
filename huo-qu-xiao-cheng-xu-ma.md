# 获取小程序码



### 获取小程序码 <a id="&#x83B7;&#x53D6;&#x5C0F;&#x7A0B;&#x5E8F;&#x7801;"></a>

通过后台接口可以获取小程序任意页面的小程序码，扫描该小程序码可以直接进入小程序对应的页面，所有生成的小程序码永久有效，可放心使用。 我们推荐生成并使用小程序码，它具有更好的辨识度，且拥有展示[“公众号关注组件”](https://developers.weixin.qq.com/miniprogram/dev/component/official-account.html)等高级能力。 生成的小程序码如下所示：

![](https://res.wx.qq.com/wxdoc/dist/assets/img/WXACode.fa3d686a.png)

可以使用开发工具 1.02.1803130 及以后版本通过二维码编译功能调试所获得的二维码

![](https://res.wx.qq.com/wxdoc/dist/assets/img/qrcodecompile.d80e2aef.png)

为满足不同需求和场景，这里提供了两个接口，开发者可挑选适合自己的接口。

* [接口 A: 适用于需要的码数量较少的业务场景](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/qr-code/wxacode.get.html)
  * 生成小程序码，可接受 path 参数较长，生成个数受限，数量限制见 [注意事项](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/qr-code.html#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)，请谨慎使用。
* [接口 B：适用于需要的码数量极多的业务场景](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/qr-code/wxacode.getUnlimited.html)
  * 生成小程序码，可接受页面参数较短，生成个数不受限。

###  获取小程序二维码（不推荐使用） <a id="&#x83B7;&#x53D6;&#x5C0F;&#x7A0B;&#x5E8F;&#x4E8C;&#x7EF4;&#x7801;&#xFF08;&#x4E0D;&#x63A8;&#x8350;&#x4F7F;&#x7528;&#xFF09;"></a>

通过后台接口可以获取小程序任意页面的小程序二维码，生成的小程序二维码如下所示：

![](https://res.wx.qq.com/wxdoc/dist/assets/img/WXAQRCode.053ccc63.png)

* [接口 C：适用于需要的码数量较少的业务场景](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/qr-code/wxacode.createQRCode.html)
  * 生成二维码，可接受 path 参数较长，生成个数受限，数量限制见 [注意事项](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/qr-code.html#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)。

###  获取小程序码（一物一码） <a id="&#x83B7;&#x53D6;&#x5C0F;&#x7A0B;&#x5E8F;&#x7801;&#xFF08;&#x4E00;&#x7269;&#x4E00;&#x7801;&#xFF09;"></a>

[微信一物一码](https://developers.weixin.qq.com/doc/offiaccount/Unique_Item_Code/Unique_Item_Code_Op_Guide.html) 支持生成小程序码。微信通过“一物一码”接口发放的二维码相比较普通链接二维码更安全、支持更小的印刷面积，支持跳转到指定小程序页面，且无数量限制。

* [接口 D：适用于“一物一码”的业务场景](https://developers.weixin.qq.com/doc/offiaccount/Unique_Item_Code/Unique_Item_Code_API_Documentation.html)

####  注意事项 <a id="&#x6CE8;&#x610F;&#x4E8B;&#x9879;"></a>

1. 接口只能生成已发布的小程序的二维码
2. 接口 A 加上接口 C，总共生成的码数量限制为 100,000，请谨慎调用。
3. 接口 B 调用分钟频率受限\(5000次/分钟\)，如需大量小程序码，建议预生成。

