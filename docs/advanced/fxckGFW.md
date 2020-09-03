# 科学上网与代理

不论你是否是程序员，你肯定需要这个东西。本文不会教授科学上网的搭建步骤，假定你已经有了一串 ss 或者任何什么的链接，然后告诉你在 Linux 上当前最好用的客户端和配置方式。

目前 Linux 上最好用的是 Qv2ray 这个软件。它是跨平台的，你在 Windows 与 macOS 均可使用。

```bash
sudo pacman -S qv2ray
```

安装完成后，可以导入你所持有的科学上网链接，即可开始使用。默认情况下，Qv2ray 只会配置 socks5 代理，如果你还需要 http 代理，请在设置中进行配置。

设置完成后，在各个软件中按照他们对应的规则配置代理即可。下面说明几种常用的软件中配置代理的方式。

- Firefox 浏览器

- Chromium 浏览器

- 终端

- OSS code

 <!-- 代理：

google-chrome-stable --proxy-server=socks5://127.0.0.1:1080

终端代理：使用包 proxychains-ng

sudo vim /etc/proxychains.conf

把配置文件中最后一行改为 shadowsocks 的本地 ip 跟端口

使用代理方式：
在命令前添加 proxychains4

proxychains4 yaourt -S crossover -->
