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

群主的 SteamID: 144736794 。由于游戏实在太多，个人肯定无法完成购买全部。期待有缘人赠送游戏，群主可以测试在 Linux 上的可运行性。

[官方文档](https://wiki.archlinux.org/index.php/Steam)  
一些字体和驱动已经在`新手上路`章节中配置完成。若有问题请自查。

```bash
sudo pacman -S steam
```

下面的清单是群主自身测试过，或者玩过的，在 Linux 下拥有`完美体验或者表现良好`的游戏列表，分为原生组和 [Steam Play](https://wiki.archlinux.org/index.php/Steam#Proton_Steam-Play) 组两类。关于非 Linux 平台的游戏，通过 Steam Play 运行的可玩程度，可通过[protondb](https://www.protondb.com/)这个网站进行查询。有时最新版 Proton 可能存在问题，需要用特定版本游玩的游戏下面列表会额外标出。  
另外，github 上还存在一些官方 proton 的 fork 版本，如 [GE proton](https://github.com/GloriousEggroll/proton-ge-custom)，可以支持一些额外的，官方暂不支持或支持不完善的游戏(如骑砍 2)。使用方式也很简单，根据官方文档，下载 release 的压缩包到指定位置，重启 steam 后即可选择特定版本的 proton。

只列出大作以及较好玩的精品，不会列举全部。

游戏锁区解决办法：让你的 steam 处于一个国家的代理下，如日本。先随便加一个游戏到购物车，在购物车右上角国家地区改成日本，再去访问已锁区的游戏，就可以浏览购买了。

#### 原生游戏组

- [武装突袭 3](https://store.steampowered.com/app/107410/Arma_3/) 大名鼎鼎的吃鸡游戏的爸爸。
- [CS GO](https://store.steampowered.com/app/730/CounterStrike_Global_Offensive/) 不用介绍了吧？
- [十字军之王 3](https://store.steampowered.com/app/1158310/Crusader_Kings_III/) 经典的中世纪模拟器 第三部已经有官方中文了。目前启动器好像有 bug,启动不了游戏。可以在游戏目录的./binary/ck3 执行起动游戏。
- [Dota2](https://store.steampowered.com/app/570/Dota_2/) 不用介绍了吧？
- [RimWorld](https://store.steampowered.com/app/294100/RimWorld/) 一款非常好玩的生存建设类游戏。
- [欧陆风云 4](https://store.steampowered.com/app/236850/Europa_Universalis_IV/) 没有官中。linux 双字节补丁暂无。
- [Kingdom: Classic](https://store.steampowered.com/app/368230/Kingdom_Classic/) 挺好玩的一个像素风横版闯关类小游戏。同系列还有几个新作。
- [地铁 2033 Redux](https://store.steampowered.com/app/286690/Metro_2033_Redux/) 经典的地铁系列。
- [地铁 Last Light Redux](https://store.steampowered.com/app/287390/Metro_Last_Light_Redux/) 经典的地铁系列。
- [星露谷物语](https://store.steampowered.com/app/413150/Stardew_Valley/) 二次元像素风农场模拟器。
- [饥荒](https://store.steampowered.com/app/219740/Dont_Starve/) 不用介绍了吧？
- [泰拉瑞亚](https://store.steampowered.com/app/105600/Terraria/) 不用介绍了吧？
- [全战三国](https://store.steampowered.com/app/779340/Total_War_THREE_KINGDOMS/) 全战系列的三国篇。
- [骑马与砍杀](https://store.steampowered.com/app/22100/Mount__Blade/) 最爱骑砍。
- [骑马与砍杀：战团](https://store.steampowered.com/app/48700/Mount__Blade_Warband/)
- [武装突袭 1(闪点行动)](https://store.steampowered.com/app/594550/Arma_Cold_War_Assault_MacLinux/) 血统上为武装突袭第一代。
- [中土世界 暗影摩多](https://store.steampowered.com/app/241930/Middleearth_Shadow_of_Mordor/) 兽人养成器。

#### Steam Play 组

如无另行说明，则代表默认使用最新的 Pronton 版本即可。

- [ATRI -My Dear Moments-](https://store.steampowered.com/app/1230140/ATRI_My_Dear_Moments/) 可爱的あとり 第一时间预购了 但始终没时间玩。 注意需要使用 Proton 4.11-13 版本。
- [cute honey](https://store.steampowered.com/app/1347430/Cute_Honey/) 已锁国区。一款社保黄油。[社保补丁](https://www.jianguoyun.com/p/DeqYLckQmv_5CBiumsoD) 注意需要使用 Proton 5.0-10 版本。
- [上古卷轴 5](https://store.steampowered.com/app/489830/The_Elder_Scrolls_V_Skyrim_Special_Edition/) 注意需要使用 Proton 5.0-10 版本。
- [LOVE³ -爱立方-](https://store.steampowered.com/app/939600/LOVE/) 一款社保黄油。steam dlc 有社保补丁 dlc。注意需要使用 Proton 5.0-10 版本。
- [三国志 11](https://store.steampowered.com/app/628070/Romance_of_the_Three_Kingdoms_XI_with_Power_Up_Kit/) 注意需要使用 Proton 5.0-10 版本。
- [骑马与砍杀 2](https://store.steampowered.com/app/261550/Mount__Blade_II_Bannerlord/) 目前最新的非 beta 游戏版本为 e1.5.4。经测试，在特定的 Proton-5.9-GE-8-ST 的版本下可正常运行。但是启动器存在 bug,需要进行一点修改。进入游戏文件夹的./bin/Win64_Shipping_Client 文件夹中，执行如下命令

  ```bash
  mv TaleWorlds.MountAndBlade.Launcher.exe TaleWorlds.MountAndBlade.Launcher.exe.bak #备份源文件
  ln -s Bannerlord.Native.exe TaleWorlds.MountAndBlade.Launcher.exe #通过符号链接让启动器直接指向Bannerlord.Native.exe
  ```

  [相关 issue 讨论](https://github.com/ValveSoftware/Proton/issues/3706)

- [Kenshi](https://store.steampowered.com/app/233860/Kenshi/) 废土生存类游戏，非常好玩。注意需要使用 Proton 5.0-10 版本。
- [光环士官长合集](https://store.steampowered.com/app/976730/Halo_The_Master_Chief_Collection/) 大名鼎鼎的光环系列。启动时需要在启动参数中加入`-windowed`，否则会报错 fatal error。可在进入游戏后自行调整分辨率。注意需要使用 Proton 5.0-10 版本。
- [Stronghold HD](https://store.steampowered.com/app/40950/Stronghold_HD/) 要塞 1 重制版，近乎完美，只是不能 Alt+Tab 切换，会卡死。
- [Stronghold Crusader HD](https://store.steampowered.com/app/40970/Stronghold_Crusader_HD/) 要塞 1 十字军重制版，近乎完美，只是不能 Alt+Tab 切换，会卡死。
- [Stronghold 2](https://store.steampowered.com/app/40960/Stronghold_2_Steam_Edition/) 要塞 2。完美运行。
- [Stronghold Legends](https://store.steampowered.com/app/40980/Stronghold_Legends_Steam_Edition/) 要塞传奇。完美运行。

### Lutris

Lutris 基于 wine，提供了大量游戏在 Linux 下的解决方案。其为你已经配置好了 wine 相关的一切配置，你只需要安装游玩即可。一般极少需要额外配置。进入[官网](https://lutris.net/)在右上角搜索你想玩的游戏。点击进入游戏页面后，可以看到在相应版本右侧有一个 install 按钮，点击后即可拉起 Lurtis 进行安装。下面针对一些群主玩的游戏进行一些额外说明。

- [暴雪战网](https://lutris.net/games/battlenet/) 直接安装后可能无法登录，这是因为缺乏某些库。阅读历史 Issue,安装如下库后解决问题。

```bash
sudo pacman -S giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo libxcomposite lib32-libxcomposite libxinerama lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader cups samba dosbox
```

- [WargamingGameCenter(坦克世界等)](https://lutris.net/games/wargaming-game-center/) 默认情况无法更新游戏。在需要更新游戏的时候，将 wine 版本设置为系统的 wine staging 版本。在更新完毕后，需要启动游戏时，将 wine 版本设置为 lutris 的版本，如 lutris 5.7-10 x86_64。如遇到无法启动闪退的情况，可以尝试在命令行启动 Lutris,再启动坦克世界即可，玄学，不知道原因。如果你玩亚服，则需要使用[透明代理](/advanced/transparentProxy)对 UDP 流量进行加速。

### 性能监控

和微星的 Afterburner 软件中性能显示的部分类似，linux 上也有一款同类软件可以监控游戏中的电脑性能，名为[MangoHud](https://github.com/flightlessmango/MangoHud)。使用方式可参见此项目的 readme。
