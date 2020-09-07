# 科学上网与代理

不论你是否是程序员，你肯定需要这个东西。`本文不会教授科学上网的搭建步骤`，假定你已经有了一串 ss 或者任何什么的链接，然后告诉你在 Linux 上当前最好用的客户端和配置方式。

目前 Linux 上最好用的是 [Qv2ray](https://qv2ray.net/) 这个软件。它是跨平台的，你在 Windows 与 macOS 均可使用。
你需要按照官方文档导入已有的链接，并且在`首选项` `入站设置`中填好本地的 SOCKS 设置与 HTTP 设置

`内核设置中`，  
V2Ray 核心可执行文件路径为/usr/bin/v2ray

V2Ray 资源目录为/usr/share/v2ray

QV2ray 设置完毕后，需要进行系统级的代理设置  
在系统代理中,设置为使用手动配置的代理服务器，填入在 QV2ray 中的代理地址即可  
注意，系统设置中的代理配置并不是所有应用都会遵守。没有遵循系统设置代理的应用还需要单独进行代理配置。

```bash
sudo pacman -S qv2ray
```

安装完成后，可以导入你所持有的科学上网链接，即可开始使用。默认情况下，Qv2ray 只会配置 socks5 代理，如果你还需要 http 代理，请在设置中进行配置。

设置完成后，在各个软件中按照他们对应的规则配置代理即可。下面说明几种常用的软件中配置代理的方式。

- Firefox 浏览器  
  火狐浏览器中可以进行设置

- Chromium 浏览器  
  遵循系统代理设置，在系统代理设置中设置完毕即可使用

- 终端  
  可以通过 export 命令设置当前终端的代理方式。如下命令将流量都代理到

```bash
export ALL_PROXY=socks5://127.0.0.1:1080

```

另一种可选项是使用包 proxychains-ng。它可以为单行命令配置代理

```bash
sudo vim /etc/proxychains.conf

```

把配置文件中最后一行改为 shadowsocks 的本地 ip 跟端口

使用代理方式： 在命令前添加 proxychains4

```bash
proxychains4 yaourt -S crossover
```

- OSS code  
  OSScode 中可以进行代理设置，位置为
