# 显卡驱动

结合[视频]()食用更佳

现在是 2020 年，显卡驱动的安装在 Arch Linux 上已经变得非常容易。

- 英特尔核心显卡 [官网文档](https://wiki.archlinux.org/index.php/Intel_graphics)

  ```bash
  sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel
  ```

- Nvidia 显卡

  - 若为台式机，拥有独立的显卡，直接安装如下两个包即可。[台式机显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA)

  ```bash
  sudo pacman -S nvidia nvidia-settings nvidia-utils lib32-nvidia-utils
  ```

  如果是 GeForce 630 以下到 GeForce 400 系列的老卡，尝试安装 [nvidia-390xx-dkms](https://aur.archlinux.org/packages/nvidia-390xx-dkms/)<sup>AUR</sup>

  再老的显卡直接使用[开源驱动](https://wiki.archlinux.org/index.php/Nouveau)即可。

  如有需要，安装 32 位支持的相关包。

  - 若为笔记本，除上述的包，推荐安装 optimus-manager。可以在核心显卡和独立显卡间轻松切换。[笔记本双显卡官方文档](https://wiki.archlinux.org/index.php/NVIDIA_Optimus)

  ```
  sudo yay -S optimus-manager
  ```

- AMD 显卡
  群主目前没有 AMD 显卡或者笔记本，无法提供经验证的最佳实践

  <!-- ```bash
  sudo pacman -S xf86-video-amdgpu    #amd显卡
  ``` -->

- OpenCL
  如果你需要 OpenCL 的安装，请参照[官方文档](https://wiki.archlinux.org/index.php/GPGPU)针对自己的显卡进行配置。

#### 后续

如果作为一个普通使用者，到这里你的系统已经配置完毕了。不会命令行也没太大关系，你可以慢慢探索 KDE 这个桌面环境，记住每天用如下命令更新系统即可。

```bash
sudo pacman -Syyu
```

接下来你可以查阅娱乐、办公、多媒体等章节了解更多使用软件的安装与使用。如果你需要成为一名较为专业的人员，那么请阅读进阶、高级、以及编程章节。
