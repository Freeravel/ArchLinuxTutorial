# 成为合格的系统管理员

阅读完`新手上路`章节，你的系统已完全可以使用，KDE 桌面环境提供了强大的 [GUI](https://zh.wikipedia.org/wiki/%E5%9B%BE%E5%BD%A2%E7%94%A8%E6%88%B7%E7%95%8C%E9%9D%A2) 以供普通用户使用。  
但如果想要游刃有余的掌控你的系统，你还需要阅读掌握本文的内容。

#### 终端操作基础

终端基础命令

#### 终端编辑器的使用

vim

#### Pacman 包管理

pacman

包管理：

pacman -Qdt

这会找出那些孤立包的，就是说他们不被任何其它包所引用。

然后把列出的包，使用 pacman -Rs 删除就成了。可以合并执行

sudo pacman -R \$(pacman -Qdtq)

清除已下载的安装包

sudo pacman -Scc

#### 系统服务的操作与介绍

systemctl

#### 系统备份

rsync 替换掉老旧的 scp

#### 必须掌握的 Linux 知识

一些 Linux 基础知识点 只介绍最基本的 最必要的。其余的在编程章节介绍

#### 杂项

可解压会乱码的 zip：

sudo pacman -S unarchiver

unar xxx.zip
