# 娱乐软件 <!-- {docsify-ignore-all} -->

#### 音视频播放器

```bash
sudo pacman -S netease-cloud-music  #网易云音乐(ArchLinuxCN)
sudo pacman -S vlc #VLC 播放器
sudo pacman -S mpv #MPV 播放器
```

#### 我的世界

```bash
sudo pacman -S minecraft #我的世界官服起动器(ArchLinuxCN)
```

### Steam

[官方文档](https://wiki.archlinux.org/index.php/Steam)  
一些字体和驱动已经在`新手上路`章节中配置完成。若有问题请自查。

```bash
sudo pacman -S steam
```

下面的清单是群主自身测试过，或者玩过的，在 Linux 下拥有`完美体验或者表现良好`的游戏列表，分为原生组和 [Steam Play](https://wiki.archlinux.org/index.php/Steam#Proton_Steam-Play) 组两类。关于非 Linux 平台的游戏，通过 Steam Play 运行的可玩程度，可通过[protondb](https://www.protondb.com/)这个网站进行查询。有时最新版 Proton 可能存在问题，需要用特定版本游玩的游戏下面列表会额外标出。另外，github 上还存在一些官方 proton 的 fork 版本，如 [GE proton](https://github.com/GloriousEggroll/proton-ge-custom)，可以支持一些额外的，官方暂不支持的游戏(如骑砍 2)。由于其非官方的定位，以及折腾，暂时持观望态度。

只列出大作以及较好玩的精品，不会列举全部。

#### 原生游戏组

- [Dota2](https://store.steampowered.com/app/570/Dota_2/)
- [CS GO](https://store.steampowered.com/app/730/CounterStrike_Global_Offensive/)
- [RimWorld](https://store.steampowered.com/app/294100/RimWorld/)
- [十字军之王 3](https://store.steampowered.com/app/1158310/Crusader_Kings_III/)
- [欧陆风云 4](https://store.steampowered.com/app/236850/Europa_Universalis_IV/)
- [星露谷物语](https://store.steampowered.com/app/413150/Stardew_Valley/)
- [饥荒](https://store.steampowered.com/app/219740/Dont_Starve/)
- [泰拉瑞亚](https://store.steampowered.com/app/105600/Terraria/)
- [全战三国](https://store.steampowered.com/app/779340/Total_War_THREE_KINGDOMS/)
- [骑马与砍杀](https://store.steampowered.com/app/22100/Mount__Blade/)
- [骑马与砍杀：战团](https://store.steampowered.com/app/48700/Mount__Blade_Warband/)
- [武装突袭 1(闪点行动)](https://store.steampowered.com/app/594550/Arma_Cold_War_Assault_MacLinux/)
- [武装突袭 3](https://store.steampowered.com/app/107410/Arma_3/)
- [地铁 Last Light Redux](https://store.steampowered.com/app/287390/Metro_Last_Light_Redux/)
- [地铁 2033 Redux](https://store.steampowered.com/app/286690/Metro_2033_Redux/)
- [中土世界 暗影摩多](https://store.steampowered.com/app/241930/Middleearth_Shadow_of_Mordor/)

#### Steam Play 组

- [三国志 11](https://store.steampowered.com/app/628070/Romance_of_the_Three_Kingdoms_XI_with_Power_Up_Kit/) 注意需要使用 Proton 5.0.9 版本
- [上古卷轴 5](https://store.steampowered.com/app/489830/The_Elder_Scrolls_V_Skyrim_Special_Edition/) 注意需要使用 Proton 5.0.9 版本

### Lutris

Lutris 基于 wine，提供了大量游戏在 Linux 下的解决方案。其为你已经配置好了 wine 相关的一切配置，你只需要安装游玩即可。一般极少需要额外配置。进入[官网](https://lutris.net/)在右上角搜索你想玩的游戏。点击进入游戏页面后，可以看到在相应版本右侧有一个 install 按钮，点击后即可拉起 Lurtis 进行安装。下面针对一些群主玩的游戏进行一些额外说明。

- [暴雪战网](https://lutris.net/games/battlenet/) 直接安装后可能无法登录，这是因为缺乏某些库。阅读历史 Issue,安装如下库后解决问题。

```bash
sudo pacman -S giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo libxcomposite lib32-libxcomposite libxinerama lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader cups samba dosbox
```

- [WargamingGameCenter(坦克世界等)](https://lutris.net/games/wargaming-game-center/) 默认情况无法更新游戏。在需要更新游戏的时候，将 wine 版本设置为系统的 wine staging 版本。在更新完毕后，需要启动游戏时，将 wine 版本设置为 lutris 的版本，如 lutris 5.7-10 x86_64。如遇到无法启动闪退的情况，可以尝试在命令行启动 Lutris,再启动坦克世界即可，玄学，不知道原因。
