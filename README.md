# 开发环境

#### 1.1 微信应用申请

1. 登录微信公众平台 [https://mp.weixin.qq.com/](https://mp.weixin.qq.com/)，使用邮箱注册帐号；
2. 在**“小程序发布流程”中，找到"配置服务器"中的“**[**开发设置**](https://mp.weixin.qq.com/wxamp/devprofile/get_profile?token=347556195&lang=zh_CN)**”可以查看到自己的AppID\(小程序ID\)和AppSecret\(小程序的密钥\)。**

     注意事项： 每个邮箱只能申请一个小程序

#### 2.2 开发工具

推荐的工具如下：

1. [vscode](https://code.visualstudio.com/Download)
2. [微信小程序开发工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

#### 2.3 在ubuntu16.04下, 安装微信小程序开发工具

1. 安装wine环境；

```text
sudo apt-get install wine
sudo apt-get -f install (如果提示出错需要运行时，再运行这一句话)
```

1. 安装微信小程序开发工具
2. 安装

    2. 安装微信小程序开发工具

```text
git clone https://github.com/cytle/wechat_web_devtools.git  &&  cd wechat_web_devtools
```

```text
cd ~
sudo git clone https://github.com/cytle/wechat_web_devtools.git
cd wechat_web_devtools
./bin/wxdt install  # 下载nw.js
注意事项：
No such file or directory
该错误是由 nw.js 下载失败所致，删除缓存，重新下载即可。
rm -rf /path/to/wechat_web_devtools/dist
rm -rf /tmp/wxdt_xsp
```

3. 运行微信小程序开发工具

```text
cd ~/wechat_web_devtools && ./bin/wxdt  # 启动程序
```

![](.gitbook/assets/image%20%281%29.png)

4. 创建项目

可以点击测试号，生成AppID

![](.gitbook/assets/image%20%282%29.png)

