# 自媒体软件配置

为获得最佳多媒体效果，需要一些额外的配置

#### 声卡驱动

音频问题：需要安装一些包来解决声道独占的问题(davinci reslove 会出现声道抢占)

声卡

```bash
sudo pacman -S alsa-utils
sudo pacman -S alsa-lib
```

<!--  不确定是否需要的：
sudo pacman -S pulseaudio-alsa  #作为一个pulseaudio与alsa的bridge，可能解决了没有声音的问题
sudo pacman -S pulseaudio
sudo pacman -S pulseeffects
sudo pacman -S pulseaudio-jack -->
