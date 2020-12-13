# 安卓刷机

前排提示：买手机最好买知名度大的品牌，较热门的机型，这样在刷机时可以方便的找到官方的 twrp 和知名的 ROM 包，如魔趣，lineageOS，cdDroid 等。如果是较冷门的品牌，官方可能没有提供 ROM，只能在网上自行寻找个人改造过的 twrp 和上述 ROM 包的 unofficial ROM。这种个人改造版本的安全性比较难说，而且还可能有更多的 bug。也有可能你翻遍全网，也找不到冷门机型能用（指好用的、非硬件提供商的官方 ROM）的 twrp 和 ROM。硬件方面，一般推荐买高通骁龙的 cpu,不要买联发科的，因为更多 ROM 的版本都是适配高通硬件的。

一般 twrp 的版本和 ROM 包有对应关系，刷机前先确认你的两个版本是兼容的，否则刷机过程可能报奇怪的错误，如 unable to mount /system

```bash
sudo pacman -S android-tools #安装linux上的安卓刷机工具包
```

去下载你机型对应的 twrp。在[官网](https://twrp.me/Devices/)搜索你的机型，下载。如果没有看到你的机型说明官方不支持，你需要自行搜索别人修改的版本。将手机连接电脑，注意要连到 USB2.0 的接口，否则可能有兼容性问题。

让手机进入 fastboot 模式，在电脑打开终端，执行

```bash
fastboot flash recovery ./path/of/your-twrp.img
```

看到终端执行完毕的时候，就可以让手机重启了。这里注意，执行`fastboot reboot`可以重启，但是在某些机型上，貌似重启不会加载刷入的 img，如乐视的 le2 x620 这样重启进入 recovery 会报错不是官方系统什么的。在手机上通过硬件按键重启进入 recovery 则可以通过。

剩下的步骤就是普通的进入 twrp,双清，刷机即可。

## 排错

有时双清或者进入 twrp 可能看到报错，用高级清理，从 ext4 改一下格式，再改回 ext4 可能就解决了
