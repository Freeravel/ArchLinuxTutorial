# Arch Linux 基础安装十九步

从安装最基础的 ArchLinux 系统开始，由于当前已经是 2020 年，安装将全部以 UEFI+GPT 的形式进行。传统方式不再赘述。  
官方文档: [安装指南](https://wiki.archlinux.org/index.php/Installation_guide)  
相关视频链接： [2020ArchLinux 安装教程](https://www.bilibili.com/video/BV1qf4y1D7Da/) 视频中可看到全部操作步骤 强烈建议观看视频配合文字学习

#### 1.刻录启动优盘

准备一个 2G 以上的优盘，刻录一个安装启动盘。

Linux 下可以直接用 dd 命令进行刻录。注意 of 的参数为 sdx,不是 sdx1 sdx2 等。

```bash
sudo dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync
```

bs=4M 指定一个较为合理的文件输入输出块大小。  
status=progress 用来输出刻录过程总的信息。  
oflag=sync 用来控制写入数据时的行为特征。确保命令结束时数据及元数据真正写入磁盘，而不是刚写入缓存就返回。

Windows 下推荐使用[Power ISO](https://www.poweriso.com/download.php)或者[Rufus](https://rufus.ie/)进行光盘刻录。二者皆为免费使用的软件。具体操作请自行查阅，都非常简单。

#### 2.检测是否为 UEFI

```bash
ls /sys/firmware/efi/efivars
```

若输出了一堆东西，说明已在 UEFI 模式。否则请确认你的启动方式是否为 UEFI。

#### 3.连接网络

无线连接:

```bash
iwctl                           #进入交互式命令行
device list                     #列出设备名，比如无线网卡看到叫 wlan0
station wlan0 scan              #扫描网络
station wlan0 get-networks      #列出网络 比如想连接CMCC-5AQ7这个无线
station wlan0 connect CMCC-5AQ7 #进行连接 输入密码即可
exit                            #成功后exit退出
```

如果你的环境只能使用无线网络，建议事先把自己所用的 wifi 名称改成自己能记住的英文。因为安装时无法显示和输入中文名的 wifi。

有线连接:  
正常来说，只要插上一个已经联网的路由器分出的网线(DHCP)，直接就能联网。若不行可以尝试输入如下命令再看看

```bash
systemctl start dhcpcd
```

#### 4.测试网络

```bash
ping www.baidu.com
```

若能看到数据返回，即说明已经联网，ctrl+c 终止退出当前命令。

#### 4.5 禁用自作聪明的 reflector

reflector 会自己更新 mirror list，然而其结果并不准确，并且会删除掉部分源信息，包括中国，这里联网后的第一件事就是将其禁用。

```bash
systemctl stop reflector.service
```

#### 5.更新系统时钟

```bash
timedatectl set-ntp true    #将系统时间与网络时间进行同步
timedatectl status          #检查服务状态
```

#### 6.更换国内镜像源加快下载速度

```bash
vim /etc/pacman.d/mirrorlist    #不会vim的同学，此处注意视频中的vim操作步骤
```

放在最上面的是会使用的更新源,把中科大 ustc 的或者清华的放在最上面

#### 7.分区

这里总共设置四个分区，可以满足绝大多数人的需求

- 根目录： `/` 50G
- EFI： `/boot/EFI` 50M
- 交换分区: `swap` 2G
- 用户主目录： `/home` 剩余全部 越大越好

cfdisk 分区的详细操作见视频中的教学

```bash
lsblk                       #显示分区情况
parted /dev/sdx             #执行parted，进行磁盘类型变更
(parted)mktable
New disk label type? gpt    #输入gpt 将磁盘类型转换为gpt 如磁盘有数据会警告，输入yes即可
quit                        #最后quit退出parted命令行交互
cfdisk  /dev/sdx            #来执行分区操作,分配各个分区大小，类型
fdisk -l                    #复查
```

#### 8.格式化

这里的 sdax 只是例子，具体根据你的实际情况来，请注意视频中的操作。

```bash
#磁盘若有数据会问 'proceed any way?' y回车 即可
mkfs.ext4  /dev/sdax            #  /        /home 两个分区
mkfs.vfat  /dev/sdax            #efi分区  挂载在/mnt/boot/EFI    300m
mkswap -f /dev/sdax             #格式化swap
swapon /dev/sdax                #打开swap分区
```

#### 9.挂载

在挂载时，挂载是有顺序的，需要从根目录开始挂载  
这里的 sdax 只是例子，具体根据你的实际情况来，请注意视频中的操作。

```bash
mount /dev/sdax  /mnt
mkdir /mnt/home
mount /dev/sdax /mnt/home
mkdir /mnt/boot
mkdir /mnt/boot/EFI
mount /dev/sdax /mnt/boot/EFI
```

#### 10.安装系统

基础包

```bash
pacstrap /mnt base base-devel linux linux-firmware #base-devel在某些AUR包的安装是必须的
```

功能性软件

```bash
pacstrap /mnt dhcpcd iwd vim sudo bash-completion                #一个有线所需 一个无线所需 一个编辑器  一个提权工具 iwd也需要dhcpcd
```

#### 11.生产 fstab

fstab 用来定义磁盘分区

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

复查一下 /mnt/etc/fstab 确保没有错误

```bash
cat /mnt/etc/fstab
```

#### 12.change root

把环境切换到新系统的/mnt 下

```bash
arch-chroot /mnt
```

#### 13.时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime     #为/usr下合适的时区在/etc下创建符号连接
```

#### 14.硬件时间设置

将系统时间同步到硬件时间

```bash
hwclock --systohc
```

#### 15.设置 Locale

Locale 决定了软件使用的语言、书写习惯和字符集。

编辑 /etc/locale.gen，去掉 en_US.UTF-8 行的注释符号（#）。

然后使用如下命令生成 locale。

```bash
locale-gen
```

向 /etc/locale.conf 输入内容

```bash
echo 'LANG=en_US.UTF-8'  > /etc/locale.conf
```

#### 16.为 root 用户设置密码

```bash
passwd root
```

#### 17.安装微码

```bash
pacman -S intel-ucode   #Intel
pacman -S amd-ucode     #AMD
```

#### 18.安装引导程序

```bash
pacman -S grub efibootmgr   #grub是启动引导器，efibootmgr被 grub 脚本用来将启动项写入 NVRAM。
grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=GRUB #取名为GRUB 并将grubx64.efi安装到之前的指定位置
grub-mkconfig -o /boot/grub/grub.cfg    #生成GRUB所需的配置文件
```

#### 19.完成安装

```bash
exit                # 退回安装环境#
umount -R  /mnt     # 卸载新分区
reboot              # 重启
```

重启后，开启 dhcp 服务，即可连接网络

```bash
systemctl start dhcpcd  #立即启动dhcp
ping www.baidu.com      #测试网络连接
```

若为无线链接，则还需要启动 iwd 才可以使用 iwctl 连接网络

```bash
systemctl start iwd #立即启动iwd
iwctl               #和之前的方式一样，连接无线网络
```
