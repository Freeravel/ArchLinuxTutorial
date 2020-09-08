# 软件推荐

#### 视频播放器

#### 音乐播放器

#### 游戏

16：安装 steam

参考链接

https://wiki.archlinux.org/index.php/Steam

主要步骤.

    Enable the multilib repository and install the steam package.

    The following requirements must be fulfilled in order to run Steam on Arch Linux:

        Installed 32-bit version OpenGL graphics driver.
        Generated en_US.UTF-8 locale, preventing invalid pointer error.
        The GUI heavily uses the Arial font. See Microsoft fonts. An alternative is to use ttf-liberation or fonts provided by Steam instead.
        Install wqy-zenhei to add support for Asian languages.

steam 里面的字体有的显示不出

按照 wiki 用一种方法就好了。。我之前把这三种全安装了 可能起了冲突 只保持第一种全部微软字体即可。

https://wiki.archlinux.org/index.php/Steam

steam 装上 steam-native-runtime 后最好用 native 版本，这样安装的游戏有图标(runtime 的没有)
