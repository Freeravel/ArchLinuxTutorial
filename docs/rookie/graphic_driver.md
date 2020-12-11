# 显卡驱动

<!-- [OpenCL 等官方文档](https://wiki.archlinux.org/index.php/GPGPU) -->

结合[视频](https://www.bilibili.com/video/BV1vK4y187Ww/)食用更佳

现在是 2020 年，显卡驱动的安装在 Arch Linux 上已经变得非常容易。本文简述几种情况下的显卡驱动安装。

## 英特尔核芯显卡

[官网文档](https://wiki.archlinux.org/index.php/Intel_graphics)

英特尔核心显卡安装如下几个包即可。

<!-- `xf86-video-intel`arch wiki 里写的很多发行版不建议安装它，而应使用 xorg 的 modesetting 驱动。-->

```bash
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

注意 只有 Intel HD 4000 及以上的核显才支持 vulkan

<!-- intel-compute-runtime? beignet? -->

## AMD 核芯显卡<sup>TODO</sup>

此路线尚未尝试，因为群主没有相关设备。

## 英伟达独立显卡

若为台式机，拥有独立的显卡，直接安装如下几个包即可。[台式机显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA)

<!-- ?也许不需要手动生成 使用闭源驱动的，在安装完后执行`nvidia-xconfig`自动生成配置文件即可。 -->

```bash
sudo pacman -S nvidia nvidia-settings lib32-nvidia-utils #必须安装
```

```bash
sudo pacman -S opencl-nvidia lib32-opencl-nvidia #可选
```

如果是 GeForce 630 以下到 GeForce 400 系列的老卡，安装 [nvidia-390xx-dkms](https://aur.archlinux.org/packages/nvidia-390xx-dkms/)<sup>AUR</sup>及其 32 位支持包。

```bash
yay -S nvidia-390xx-dkms nvidia-390xx-utils lib32-nvidia-390xx-utils
```

再老的显卡直接使用[开源驱动](https://wiki.archlinux.org/index.php/Nouveau)即可。

```bash
sudo pacman -S mesa lib32-mesa xf86-video-nouveau
```

重启

---

若为 Intel 核显+Nvidia 独显的笔记本，同样需要按照显卡新旧的分类安装如上台式机驱动的各个软件包，除此之外还需要安装 optimus-manager。可以在核心显卡和独立显卡间轻松切换。[笔记本双显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA_Optimus)

```bash
yay -S optimus-manager optimus-manager-qt
sudo pacman -S bbswitch #安装bbswitch切换方式
```

安装完成后可以看到右下角已经出现图标，右键设置，在 Optimus 中的 switch method 选择 Bbswitch 即可。重启即可使用。

在你切换显卡模式`前`需要进行的配置：

<!-- - I 卡 N 卡的 modeset 选项都去掉勾选
- 切换到英特尔核显模式前，需要选择 intel，不要选 modesettings 模式。否则会黑屏+混成不能开启 -->

- hybird 模式中添加的三个环境变量，在切换到其他模式之前一定要去掉，否则会黑屏，切换不到 intel。
<!-- - 如果你使用了混成器，调整至 OpenGl 2.0 - 平滑模式。否则切换时可能会卡 splash screen -->

显卡之间的切换，重新登陆后可能会在 splash screen 卡很久，大概有几分钟的样子，是正常的，耐心等待不要以为卡死了。

经过测试，第一次的显卡切换基本不会有问题，但是反复横跳，不停的切换显卡的话，可能会切换失败， 但是不会黑屏。等进入系统后重启即可。

<!-- 目前的 hybrid 模式尚不稳定，不建议使用。 -->

对于 AMD 核显+N 卡独显的同学，optimus-manager 对于这套组合的支持已经开发完成合入 master，这个组合的同学已经可以使用了。但是开发者说他没有相关设备，无法测试。这个组合在目前可能仍存在一些未知问题。

## AMD 显卡

可先用 `lspci -v` 确认你显卡的详细信息

[驱动分类官方文档](https://wiki.archlinux.org/index.php/Xorg#AMD)  
[新卡开源驱动](https://wiki.archlinux.org/index.php/AMDGPU)  
[老卡开源驱动](https://wiki.archlinux.org/index.php/ATI)

对于一些旧型号的 AMD 显卡(GCN2 及以前的型号)，闭源驱动 Catalyst 目前已经停止更新，其最后的版本只支持到 Xorg1.10,基本在 arch linux 上已经处于无法使用的地步（如我的 HD7670M，属于 TeraScale 2 架构），这种情况只能使用开源的 [ATI 驱动](<https://wiki.archlinux.org/index.php/ATI_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>)。如果是笔记本，则只能使用 PRIME 的双显卡切换方式。
此外，可以使用 `glmark2`，`DRI_PRIME=1 glmark2` 分别对核显和独显进行测试，选择分数更高的一个进行使用。原因是只能使用开源驱动的老型号独立显卡，性能可能还比不上核芯显卡。如果玩游戏的话，还是要根据实际表现来。

可以在 steam 游戏的启动前缀中加入`DRI_PRIME=1 mangohud %command%`来使用独显。(关于 [mangohud](/play/software?id=性能监控))

笔记本上使用独立显卡运行 steam 游戏的另一个例子。

```bash
DRI_PRIME=1 steam steam://rungameid/570 #运行dota2
DRI_PRIME=1 steam steam://rungameid/730 #运行cs go
```

只能用 ATI 驱动的老显卡应该都不支持 vulkan。

---

对于新型号，即 GCN3 架构及更新，开源驱动可使用 AMDGPU 闭源驱动可使用 AMDGPU PRO。此路线尚未尝试，因为群主没有相关设备。

## 性能测试

最传统和广为人知的方式为使用`glxgears`命令进行测试，其属于[mesa-demos](https://www.archlinux.org/packages/extra/x86_64/mesa-demos/)包。但其仅仅只能提供简单的测试场景及帧数显示，只测试了当前 OpenGL 功能的一小部分，功能明显不足。群主推荐如下两种工具。

### glmark2

glmark 提供了一系列丰富的测试，涉及图形单元性能（缓冲，建筑，照明，纹理等）的不同方面，允许进行更全面和有意义的测试。 每次测试单独计算帧速率。 最终，用户根据以前的所有测试获得了一个成绩分数。在 archlinux 上属于包[glmark2](https://aur.archlinux.org/packages/glmark2/)<sup>AUR</sup>

### Unigine benchmark

Unigine 3D 引擎是一个更全面的基准测试工具。 截止目前有五个版本，从新到旧分别是

- sanctuary(2007)
- tropics(2008)
- heaven(2009)
- valley(2013)
- superposition(2017)

可从[AUR](https://aur.archlinux.org/packages/?O=0&K=Unigine)下载全部版本。

这些基准测试工具拥有实时的环境遮挡，来自不同来源的相互关联的灯光，HDR 效果图，逼真的水和具有大气光散射的动态天空。 用户还可以设置抗锯齿级别，纹理质量和滤波，各向异性和着色器质量。 除了能够以多个步骤测试硬件的“基准测试”按钮之外，您还可以自由地漫游，改变一天中的时间（改变世界的照明），并准确地确定“掰弯”硬件最多的条件。

## 硬件检测

如下两款是群主目前找到的，最佳的图形化查看硬件信息的软件。

```bash
yay -S cpu-x
yay -S gpu-viewer
```

## 后续

如果作为一个普通使用者，到这里你的系统已经配置完毕了。不会命令行也没太大关系，你可以慢慢探索 KDE 这个桌面环境，记住每天用如下命令更新系统即可。

```bash
sudo pacman -Syyu
```

接下来你可以查阅娱乐、办公、多媒体等章节了解更多使用软件的安装与使用。如果你需要成为一名较为专业的人员，那么请阅读进阶、高级、以及编程章节。
