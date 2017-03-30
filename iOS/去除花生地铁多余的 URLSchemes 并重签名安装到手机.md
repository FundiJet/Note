##### 1. 前言

最近在手机上安装了 [Magic Launcher Pro](https://itunes.apple.com/cn/app/magic-launcher-pro-launch-anything-instantly/id1121523480?mt=8) app，在通知中心添加了快速打开应用的插件，添加了淘宝等应用启动器。



##### 2. 发现

在通知中心点击淘宝时，意外发现调起的是花生地铁这个 app，第一次还认为是应用本身关于淘宝的 URL Schemes 出错，于是乎手动在 Magic Launcher Pro 中定制了一个启动器，结果还是一样，仍然调起花生地铁。

出于好奇，观察了一下花生地铁应用的 URL Schemes 的设置。

![](http://ocpmjsr1z.bkt.clouddn.com/fuck_url_schemes.png)

淘宝／天猫／大众点评／苏宁什么的统统在内，厉害了。



##### 3. 修改

因为我的 iPhone 已越狱，所以是通过 [Clutch](https://github.com/KJCracks/Clutch) 对手机中的花生地铁进行解密，非越狱手机可以在PP助手等应用商店下载越狱版应用。

解压 ipa 文件，右键 app 后缀文件，显示包内容。修改 Info.plist 文件，删除不必要的 URL Schemes。

之后是对应用重签名，这里我使用的是 [AppResign](https://github.com/Urinx/iOSAppHook)，比较简单。

![](http://ocpmjsr1z.bkt.clouddn.com/%F0%9F%92%8A.png)

最后通过 `ideviceinstaller` 安装到手机上，成功运行不闪退。





