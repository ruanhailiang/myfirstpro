# 配置小程序

## 小程序配置 <a id="&#x5C0F;&#x7A0B;&#x5E8F;&#x914D;&#x7F6E;"></a>

###  全局配置 <a id="&#x5168;&#x5C40;&#x914D;&#x7F6E;"></a>

小程序根目录下的 `app.json` 文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等。

完整配置项说明请参考[小程序全局配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)

以下是一个包含了部分常用配置选项的 `app.json` ：

```text
{
  "pages": [
    "pages/index/index",
    "pages/logs/index"
  ],
  "window": {
    "navigationBarTitleText": "Demo"
  },
  "tabBar": {
    "list": [{
      "pagePath": "pages/index/index",
      "text": "首页"
    }, {
      "pagePath": "pages/logs/logs",
      "text": "日志"
    }]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "debug": true,
  "navigateToMiniProgramAppIdList": [
    "wxe5f52902cf4de896"
  ]
}
```

完整配置项说明请参考[小程序全局配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)

###  页面配置 <a id="&#x9875;&#x9762;&#x914D;&#x7F6E;"></a>

每一个小程序页面也可以使用同名 `.json` 文件来对本页面的窗口表现进行配置，页面中配置项会覆盖 `app.json` 的 `window` 中相同的配置项。

完整配置项说明请参考[小程序页面配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/page.html)

例如：

```text
{
  "navigationBarBackgroundColor": "#ffffff",
  "navigationBarTextStyle": "black",
  "navigationBarTitleText": "微信接口功能演示",
  "backgroundColor": "#eeeeee",
  "backgroundTextStyle": "light"
}
```

微信现已开放小程序内搜索，开发者可以通过 `sitemap.json` 配置，或者管理后台页面收录开关来配置其小程序页面是否允许微信索引。当开发者允许微信索引时，微信会通过爬虫的形式，为小程序的页面内容建立索引。当用户的搜索词条触发该索引时，小程序的页面将可能展示在搜索结果中。 爬虫访问小程序内页面时，会携带特定的 user-agent：`mpcrawler` 及[场景值](https://developers.weixin.qq.com/miniprogram/dev/reference/scene-list.html)：`1129`。需要注意的是，若小程序爬虫发现的页面数据和真实用户的呈现不一致，那么该页面将不会进入索引中。

具体配置说明

1. 页面收录设置：可对整个小程序的索引进行关闭，小程序管理后台-设置-基本设置-页面收录设置；[详情](https://mp.weixin.qq.com/wxopen/readtemplate?t=config/collection_agreement_tmpl)
2. sitemap 配置：可对特定页面的索引进行关闭

##  sitemap 配置 <a id="sitemap-&#x914D;&#x7F6E;"></a>

小程序根目录下的 `sitemap.json` 文件用来配置小程序及其页面是否允许被微信索引。

完整配置项说明请参考[小程序 sitemap 配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/sitemap.html)

**例1：**

```text
{
  "rules":[{
    "action": "allow",
    "page": "*"
  }]
}
```

所有页面都会被微信索引（默认情况）

**例2：**

```text
{
  "rules":[{
    "action": "disallow",
    "page": "path/to/page"
  }]
}
```

配置 `path/to/page` 页面不被索引，其余页面允许被索引

**例3：**

```text
{
  "rules":[{
    "action": "allow",
    "page": "path/to/page"
  }, {
    "action": "disallow",
    "page": "*"
  }]
}
```

配置 `path/to/page` 页面被索引，其余页面不被索引

**例4：**

```text
{
  "rules":[{
    "action": "allow",
    "page": "path/to/page",
    "params": ["a", "b"],
    "matching": "inclusive"
  }, {
    "action": "allow",
    "page": "*"
  }]
}
```

包含 `a 和 b` 参数的 `path/to/page` 页面会被微信优先索引，其他页面都会被索引，例如：

* `path/to/page?a=1&b=2` =&gt; 优先被索引
* `path/to/page?a=1&b=2&c=3` =&gt; 优先被索引
* `path/to/page` =&gt; 被索引
* `path/to/page?a=1` =&gt; 被索引
* 其他页面都会被索引

**例5：**

```text
{
  "rules":[{
    "action": "allow",
    "page": "path/to/page",
    "params": ["a", "b"],
    "matching": "inclusive"
  }, {
    "action": "disallow",
    "page": "*"
  }, {
    "action": "allow",
    "page": "*"
  }]
}
```

* `path/to/page?a=1&b=2` =&gt; 优先被索引
* `path/to/page?a=1&b=2&c=3` =&gt; 优先被索引
* `path/to/page` =&gt; 不被索引
* `path/to/page?a=1` =&gt; 不被索引
* 其他页面不会被索引

**注：没有 sitemap.json 则默认所有页面都能被索引**

**注：`{"action": "allow", "page": "*"}` 是优先级最低的默认规则，未显式指明 "disallow" 的都默认被索引**

###  如何调试 <a id="&#x5982;&#x4F55;&#x8C03;&#x8BD5;"></a>

当在小程序项目中设置了 `sitemap` 的配置文件（默认为 `sitemap.json`）时,便可在开发者工具控制台上显示当前页面是否被索引的调试信息（ 最新版本的开发者工具支持索引提示）

![sitemap.png](https://res.wx.qq.com/wxdoc/dist/assets/img/sitemap.7929a470.png)

**注：`sitemap` 的索引提示是默认开启的，如需要关闭 `sitemap` 的索引提示，可在小程序项目配置文件 `project.config.json` 的 `setting` 中配置字段 `checkSiteMap` 为 `false`**

**注: `sitemap` 文件内容最大为 5120 个 UTF8 字符**

