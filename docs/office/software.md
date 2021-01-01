# 办公软件

本章记录日常办公需要用到的软件及配置。同时包括 QQ 等即时通讯软件的配置与使用。

#### 即时通讯

安装 wine 版本前先确保[字体](https://wiki.archlinux.org/index.php/Localization/Chinese#Fonts)的安装，否则汉字均为方块。一般 qq 安装文泉驿字体(wqy-microhei)即可解决方块问题。

深度于 2020 下半年放出了 deepin-wine5，基于这个最新版的 deepinwine 的 AUR 包一般都比原有的稳定。

```bash
sudo pacman -S telegram-desktop #全球流行的即时通信软件 俗称电报
yay -S slack-desktop            #优秀的团队合作交流软件
yay -S deepin.com.qq.im.light　 #基于deepin wine5的qq轻聊版 强烈推荐安装  需要字体
yay -S linuxqq                  #腾讯官方出版的辣鸡linuxqq 疯狂闪退 网上方式均无效 不建议安装
yay -S com.qq.im.deepin         #基于deepin wine5的qq
yay -S com.qq.weixin.deepin     #基于deepin wine5的wechat
yay -S wechat-uos               #2020年末最新的uos版本原生微信的arch移植版本
```

除此之外 对于另外一些手机通讯软件可以尝试使用[scrcpy](https://aur.archlinux.org/packages/scrcpy/)

#### 办公套件

主要两个选择是 [WPS](<https://wiki.archlinux.org/index.php/WPS_Office_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>) 与 [LibreOffice](https://wiki.archlinux.org/index.php/LibreOffice)。LibreOffice 目前的安装已经非常简单。

```bash
sudo pacman -S libreoffice-still   #稳定版
sudo pacman -S libreoffice-fresh   #尝鲜版
```

WPS 请按官方文档安装。需要注意的是，由于分发问题，2020 下半年 WPS 已从 archlinuxcn 移除，安装请务必从 AUR 安装。

#### 截图

flameshot
[火焰截图](https://www.bilibili.com/video/BV1LK4y1s7wX/)

```
sudo pacman -S flameshot
```

#### 网盘存储

- [稳定版坚果云](https://aur.archlinux.org/packages/nutstore/)<sup>AUR</sup> 可直接使用 [web 版本](https://www.jianguoyun.com/d/home#/) [最新版坚果云](https://aur.archlinux.org/packages/nutstore-experimental/)<sup>AUR</sup>
- [Mega](https://aur.archlinux.org/packages/megasync/)<sup>AUR</sup> 可直接使用 [web 版本](https://mega.nz/fm/dashboard)
- [超星网盘](http://i.mooc.chaoxing.com/space/index?t=1600061701200) 据说免费 100G 未验证
- [百度网盘](https://aur.archlinux.org/packages/baidunetdisk-bin/) 辣鸡

#### 图片浏览

快速看图

- [feh](https://www.archlinux.org/packages/extra/x86_64/feh/)
- [nomacs](https://www.archlinux.org/packages/community/x86_64/nomacs/)
