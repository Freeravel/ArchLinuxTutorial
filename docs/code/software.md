# 软件推荐

代理：

google-chrome-stable --proxy-server=socks5://127.0.0.1:1080

终端代理：使用包 proxychains-ng

sudo vim /etc/proxychains.conf

把配置文件中最后一行改为 shadowsocks 的本地 ip 跟端口

使用代理方式：
在命令前添加 proxychains4

proxychains4 yaourt -S crossover

包管理：

pacman -Qdt

这会找出那些孤立包的，就是说他们不被任何其它包所引用。

然后把列出的包，使用 pacman -Rs 删除就成了。可以合并执行

sudo pacman -R \$(pacman -Qdtq)

清除已下载的安装包

sudo pacman -Scc

可解压会乱码的 zip：

sudo pacman -S unarchiver

unar xxx.zip
