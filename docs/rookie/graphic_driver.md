# 显卡驱动

<!-- [OpenCL 等官方文档](https://wiki.archlinux.org/index.php/GPGPU) -->

结合[视频](https://www.bilibili.com/video/BV1vK4y187Ww/)食用更佳

现在是 2020 年，显卡驱动的安装在 Arch Linux 上已经变得非常容易。本文简述几种情况下的显卡驱动安装。

## 英特尔核芯显卡

[官网文档](https://wiki.archlinux.org/index.php/Intel_graphics)

英特尔核心显卡安装如下几个包即可。`xf86-video-intel`arch wiki 里写的很多发行版不建议安装它，而应使用 xorg 的 modesetting 驱动。但是我实际尝试了，如果不使用它，混成，启动都会出现问题。所以还是建议安装使用。

```bash
sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

<!-- intel-compute-runtime? beignet? -->

## AMD 核芯显卡<sup>TODO</sup>

[驱动分类官方文档](https://wiki.archlinux.org/index.php/Xorg#AMD)  
[新卡驱动](https://wiki.archlinux.org/index.php/AMDGPU)  
[老卡驱动](https://wiki.archlinux.org/index.php/ATI)  
群主目前没有相关设备，无法提供`经验证的`最佳实践。相关同学请自行查看文档踩坑。

## 英伟达独立显卡

若为台式机，拥有独立的显卡，直接安装如下几个包即可。[台式机显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA)

<!-- ?也许不需要手动生成 使用闭源驱动的，在安装完后执行`nvidia-xconfig`自动生成配置文件即可。 -->

```bash
sudo pacman -S nvidia nvidia-settings nvidia-utils lib32-nvidia-utils opencl-nvidia lib32-opencl-nvidia
```

如果是 GeForce 630 以下到 GeForce 400 系列的老卡，安装 [nvidia-390xx-dkms](https://aur.archlinux.org/packages/nvidia-390xx-dkms/)<sup>AUR</sup>及其 32 位支持包。

```bash
yay -S nvidia-390xx-dkms nvidia-390xx-utils lib32-nvidia-390xx-utils
```

再老的显卡直接使用[开源驱动](https://wiki.archlinux.org/index.php/Nouveau)即可。

```bash
sudo pacman -S mesa lib32-mesa xf86-video-nouveau
```

---

若为 Intel 核显+Nvidia 独显的笔记本，同样需要按照显卡新旧的分类安装如上台式机驱动的各个软件包，除此之外还需要安装 optimus-manager。可以在核心显卡和独立显卡间轻松切换。[笔记本双显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA_Optimus)

```
yay -S optimus-manager optimus-manager-qt
```

在你切换显卡模式`前`需要进行的配置：

- I 卡 N 卡的 modeset 选项都去掉勾选
- 切换到英特尔核显模式前，需要选择 intel，不要选 modesettings 模式。否则会黑屏+混成不能开启
- hybird 模式中添加的三个环境变量，在切换到其他模式之前一定要去掉，否则会黑屏，切换不到 intel。
- 如果你使用了混成器，调整至 OpenGl 2.0 - 平滑模式。否则切换时可能会卡 splash screen

<!-- 目前的 hybrid 模式尚不稳定，不建议使用。 -->

对于 AMD 核显+N 卡独显的同学，optimus-manager 对于这套组合的支持正在开发中，这个组合的同学需要再等待一下。

## AMD 显卡

群主目前没有 AMD 显卡或者笔记本，无法提供经验证的最佳实践

  <!-- ```bash
  sudo pacman -S xf86-video-amdgpu opencl-mesa lib32-opencl-mesa   #amd显卡
  ``` -->

## 后续

如果作为一个普通使用者，到这里你的系统已经配置完毕了。不会命令行也没太大关系，你可以慢慢探索 KDE 这个桌面环境，记住每天用如下命令更新系统即可。

```bash
sudo pacman -Syyu
```

接下来你可以查阅娱乐、办公、多媒体等章节了解更多使用软件的安装与使用。如果你需要成为一名较为专业的人员，那么请阅读进阶、高级、以及编程章节。
