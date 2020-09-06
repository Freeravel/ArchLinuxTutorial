# Linux 日常操作与基础知识

阅读完`新手上路`章节，你的系统已完全可以使用，KDE 桌面环境提供了强大的 [GUI](https://zh.wikipedia.org/wiki/%E5%9B%BE%E5%BD%A2%E7%94%A8%E6%88%B7%E7%95%8C%E9%9D%A2) 以供普通用户使用。  
但如果想要游刃有余的掌控你的系统，你还需要阅读掌握本文的内容。

#### 终端操作基础

如果想要熟练掌握 Linux，就必须掌握终端的常见命令与使用方式。如下进行`最常见`的终端命令介绍。

```bash
ls /some_path       #查看某个文件夹下的文件与子文件夹 /代表根目录，是Linux最顶端的路径，是绝对路径
pwd                 #查看当前终端所在路径
cd /home/wallen     #切换目录命令，将当前终端切换到某一个路径下
cp ./a.cpp ./b.cpp  #复制命令 将当前路径下的a.cpp复制一份为b.cpp ./代表当前文件夹所在路径，是相对路径
cp -r ./a ./b       #复制整体文件夹
rm b.cpp            #删除命令 删除b.cpp
mv a.cpp b.cpp      #移动(重命名)命令 将a.cpp更名为b.cpp
mkdir new_folder    #新建文件夹new_folder
```

如果你想进一步了解终端命令行的操作与 Shell 脚本编程，推荐阅读书籍`《Linux命令行与Shell脚本编程大全(第3版)》`。群主也提供了与其配套的书籍教学视频 [Linux 命令行与 Shell 教程](https://bilibili.com)。

如果你不想详细了解，上述介绍的几个命令也足够你来应付日常的使用。

#### Pacman 包管理

在 Arch Linux 上安装的软件都通过 Pacman 来进行管理。你可以把它理解为一个软件管理器，可以进行软件的安装，删除，查询等。  
如下进行`最常见`的包管理命令介绍。[官方文档](https://wiki.archlinux.org/index.php/Pacman)

```bash
sudo pacman -S package_name     #安装软件包
sudo pacman -Syyu               #升级系统 yy标记强制刷新 u标记升级动作
sudo pacman -R package_name     #删除软件包
sudo pacman -Rs package_name    #删除软件包，及其所有没有被其他已安装软件包使用的依赖包
sudo pacman -Qdt                #找出孤立包 Q为查询操作 d标记依赖包 t标记不需要的包 dt合并标记孤立包
sudo pacman -Rs $(pacman -Qtdq) #删除孤立软件包
```

#### 终端编辑器的使用

vim

#### 系统服务的操作与介绍

systemctl

#### 系统备份

rsync 替换掉老旧的 scp

#### 必须掌握的 Linux 知识

一些 Linux 基础知识点 只介绍最基本的 最必要的。其余的在编程章节介绍。

#### 杂项

可解压会乱码的 zip：

sudo pacman -S unarchiver

unar xxx.zip
