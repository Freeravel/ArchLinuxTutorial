# 基础安装 
从安装最基础的ArchLinux系统开始，由于当前已经是2020年，安装将全部以UEFI+GPT的形式进行。传统方式不再赘述。  
官方文档: [安装指南](https://wiki.archlinux.org/index.php/Installation_guide)  
相关视频链接： 2020ArchLinux安装教程 视频文字结合效果更好

#### 1.刻录启动优盘
准备一个2G以上的优盘，刻录一个安装启动盘。  

Linux下可以直接用dd命令进行刻录  
```bash
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync
```  
bs=4M 指定一个较为合理的文件输入输出块大小。
oflag=sync 用来控制写入数据时的行为特征。确保命令结束时数据及元数据真正写入磁盘，而不是刚写入缓存就返回。  
status=progress 用来输出刻录过程总的信息。 

Windows下推荐使用[Power ISO](https://www.poweriso.com/download.php)或者[Rufus](https://rufus.ie/)进行光盘刻录。二者皆为免费使用的软件。具体操作请自行查阅，都非常简单。

## 2.检测是否为uefi
```
ls /sys/firmware/efi/efivars
```

## 链接网络

     iwctl 进入交互式命令行

     device list 列出设备名，比如无线网卡可以看到叫 wlan0

     station wlan0 scan   扫描

     station wlan0 get-networks  列出

     station wlan0 connect CMCC-5AQ7  链接 输入密码即可

     成功后exit退出



或者直接有线链接DHCP的路由或者网线 理论上直接就能联网， 若没有链接可以尝试

    systemctl start dhcpcd


## 测试网络

    ping www.baidu.com

ctrl+c终止


## 更新系统时钟

    timedatectl set-ntp true

检查一下

    timedatectl status


## 更换国内源加快速度

    vim /etc/pacman.d/mirrorlist

放在最上面的是会使用的更新源,把ustc的放在最上面


## 分区

lsblk 显示分区情况


parted /dev/sdx

(parted)mktable

New disk label type? gpt

最后quit即可

cfdisk  /dev/sdx 来执行分区  分区,选择type

fdisk -l 复查


## 格式化

    mkfs.vfat  /dev/sdax         #efi分区  挂载在/mnt/boot/EFI    300m

    mkfs.ext4  /dev/sdax        #  /        /home 两个分区    
    若有数据会问 'proceed any way?' y回车 即可  

    mkswap -f /dev/sdax       #格式化swap

    swapon /dev/sdax             #打开swap分区


## 9.挂载

    在挂载时，挂载是有顺序的，需要从根目录: /开始挂载

    mount /dev/sda1  /mnt

    mkdir   /mnt/home

    mount /dev/sda2 /mnt/home

    mkdir /mnt/boot

    mkdir /mnt/boot/EFI

    mount /dev/sda3 /mnt/boot/EFI


## 10.安装系统

基础包

    pacstrap /mnt base linux linux-firmware


功能性软件

    pacstrap /mnt dhcpcd iwd vim     一个有线 一个无线 一个编辑器  iwd也需要dhcpcd



## 11.生产fstab

    genfstab -U /mnt >> /mnt/etc/fstab

复查一下 /mnt/etc/fstab  确保没有错误

    cat /mnt/etc/fstab


## 12.changeroot  //把root环境切换到/mnt下

    arch-chroot /mnt


## 13.时区

    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime


15:硬件时间设置

    hwclock --systohc

默认为 UTC 时间


16:设置Locale

Locale 决定了软件使用的语言、书写习惯和字符集。

编辑 /etc/locale.gen，去掉需要的行的注释符号（#）。

去掉en_US.UTF-8行的注释符号（#）。 

然后使用 locale-gen 生成 locale。

执行

    locale-gen

编辑

    /etc/locale.conf

    echo 'LANG=en_US.UTF-8'  > /etc/locale.conf



18：为 root 用户设置密码

    passwd root


19：安装微码

    pacman -S intel-ucode


20:安装引导程序

    pacman -S grub efibootmgr

    grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=GRUB

    grub-mkconfig -o /boot/grub/grub.cfg


21:重启

    exit    # 退回安装环境# 

    umount -R  /mnt    # 卸载新分区

    reboot    # 重启


## 
装桌面    plasma  
装gretter  sddm  要有一个非root用户才行









