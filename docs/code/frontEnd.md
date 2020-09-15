# 前端开发 <!-- {docsify-ignore-all} -->

本文简述 Arch Linux 下当前最佳前端开发工具与流程。

#### 开发工具

对于前端开发来说，Linux 上的开发与 Windows 或者 macOS 基本没有差别。最重要的工具一个是浏览器，如下为全部 Arch Linux 上原生可用的浏览器，只要你不是一定要调试 IE/~~Edge~~/Safari，则完全可以满足需求。

- [chromium](https://www.archlinux.org/packages/extra/x86_64/chromium/)
- [firefox](https://www.archlinux.org/packages/extra/x86_64/firefox/)
- [firefox-developer-edition](https://www.archlinux.org/packages/community/x86_64/firefox-developer-edition/)
- [opera](https://www.archlinux.org/packages/community/x86_64/opera/)
- [Edge](https://www.microsoftedgeinsider.com/en-us/download/?platform=linux)<sup>Coming soon</sup>
- [vivaldi](https://aur.archlinux.org/packages/vivaldi/)<sup>AUR</sup>
- [google-chrome](https://aur.archlinux.org/packages/google-chrome/)<sup>AUR</sup>

除了浏览器，另一个重要工具就是编辑器。推荐前端开发均使用[OSS code](https://www.archlinux.org/packages/community/x86_64/code/)，微软 Visual Studio Code 的开源版本。其配置与使用与其余平台均保持一致，本文不再赘述。至于 [yarn](https://www.archlinux.org/packages/community/any/yarn/)、[npm](https://www.archlinux.org/packages/community/any/npm/) 等工具，也均可用 pacman 安装，这些内容不在本文讨论范围内。

#### 前端 C 端性能优化

简单来说有三个优化的方向：

- 配合 App，端内 h5 优化。有两个方法。

  1. 第一种：预加载。app 客户端中 提前开启一个后台 webview 加载所需的页面。
  2. 第二种：在 App 启动时，去拉取一个 h5 资源离线包，并且在 app 内部打开 h5 页面的时候，检测 h5 请求的资源的地址，如果匹配到了 h5 离线资源包中的内容，那么直接从本地的 h5 资源离线包加载，秒出，就不用请求网络了。如果匹配不到再请求网络。缺点是更新问题，如果 h5 出了 bug，要更新离线资源包才行。

- 单页应用的预渲染。通过一些[插件](https://github.com/chrisvfritz/prerender-spa-plugin),来通过 puppeteer 提前生成好 html 文件。对于首屏静态展示型的网站，预渲染是最佳的选择。若首屏存在 AJAX 请求，也可使用预渲染，但是可能存在的问题是，AJAX 数据在首屏的预渲染已经存在了，在 js 回来后还会再请求一次，造成页面的闪动。这种情况下，建议直接保证预渲染时请求失败就可以了(比如不等数据返回就渲染，此时应该是在骨架屏或者空白的状态下)，这时候预渲染出的 html 就是骨架屏或者空位。你也可以依然把数据预渲染出来，但是需要寻求方法在预渲染 html 已有数据时，取消 js 里的 AJAX 请求。一种做法是在 html 里去判断对应位置是否有数据，若有，则在 window 上用一个标记位属性标记。在 js 中若检测到了这个标志位，则不去发出请求。

  如果网站的两部分间存在较大差异（比如网站的首页首屏，与网站的协议页），并且这两部分均存在大量的访问，那么可以考虑后端路由返回不同的 html 文件。一般来说，协议页很简单，所需的 js 会比首屏小很多。分开访问 html 可以提升协议页等小页面的响应速度。

  预渲染存在的问题及解决方案：

  1. cdn 是最大的问题。目前最快侵入最低方式就是进行 double build 的流程。若 cdn 造成本地 build 失败，可以把 public url 去掉先。

  ref:
  https://www.cnblogs.com/chuaWeb/p/prerender-plugin.html
  https://juejin.im/post/5cc5af1f6fb9a032447f0299#heading-0

  2. 单页应用的路由刷新情况：针对预渲染的路由返回其预渲染好的，对应的 index.html。在刷新页面时，若没有对应路径的 html，则设置一个 loading 预渲染的页面专门用于返回，当 js 请求到了，前端自己进行跳转。

- 服务端渲染方案（SSR）。不会提前生成 html 文件，在收到客户端请求时才进行渲染。同时会在内网环境中完成 ajax 动态内容的请求。优势在于请求数据的网络环境在内网，相对会比较稳定和快速。可以把所有请求都做成服务端请求，也可以只做一部分，另一部分还是在客户端请求。缺点在于对已有工程改动较大。新工程可以尝试。目前开源社区存在一些 SSR 项目。理论上，如果没有异步请求(静态页面)或者不靠考虑请求，预渲染的速度会比 SSR 还快。因为是预先生成的。

  - 什么情况下必须要用 SSR:
    1. 用户网络极差，差到不能再差那种（兼容 2G GSM 等可以考虑）。如果首屏需要异步请求，那只能 SSR 更好。
    2. 因为某些无法抗拒原因无法采用预渲染。比如页面的 url 为动态生成(需要根据 url 中的参数请求数据)，或者由于数据隐私问题无法进行预渲染。如果请求的动态数据是一个较为固定的请求，如请求当前热门新闻，热门视频，不强依赖 url 传递的特定参数，则可以不用 ssr，依然采用预渲染，也可以起另外的服务专门用来刷新生成特定的预渲染页面。

---

下面列举一些具体的优化手段

- 尽可能移除第三方依赖的库，使用 纯 js/css 完成功能。第三方库/UI 组件库的会增大 js bundle 的大小。项目组里如果有一些专门为前端 C 端页面构造的组件库是最好的。这类库需要要求整体体积小，尽量不依赖第三方依赖，能体现项目外观整体的一致性风格。
- 页面的基础结构展示尽量不依赖 js，此时使用预渲染没有任何问题。如果首屏部分布局依赖 js 计算，那么当 vendors main js 没拿回来的时候，结构会是错乱的。
- 不使用 styled components。 一是会增大 bundle 大小影响性能，二是必须要等 js 回来才能渲染，直接写 css 渲染速度会更快。除非 js 需要依赖 css 样式去做一些事情。比如样式没有加载前某个元素宽 0px，js 需要根据元素宽度去决定定位位置，或者手势滑动多少宽度的百分比触发翻页这种。一旦 css 加载失败，就不可用。当 js 依赖样式尺寸的情况下，cssinjs 可以保证 js 运行时的可靠和一致性。
- 使用预渲染插件进行预渲染，预渲染+SEO 的成熟方案：
  - React :prerender-spa-plugin+react-helmet
  - Vue: prerender-spa-plugin+vue-meta-info
- 更新 tsconf 配置，变更可能增加包大小的配置。
  - 确保 tsconfig.json 的 compilerOptions.target 为 ESNext ， compilerOptions.module 也为 ESNext ，而 compilerOptions.importHelpers 为 false ，否则 TypeScript 编译器会引入很多不必要的 Helpers 而增大客户端的页面 JavaScript 体积。
- 更换素材。Otf 字体变成 woff   体积减少一半。视频要求设计出一版最小循环的   体积减少一半
- DNS prefetch。在 html 中加入相关 cdn 地址的预取，提升速度。如

  ```html
  <link rel="dns-prefetch" href="//xxx.com" />
  ```

- Dynamic import。使用动态加载的方式进行引入首屏不需要的组件，减小 vendor.main.js 大小。
- 服务器开启 http2 优化。
- 代码优化。检视代码中可改进的点，在 render 中的 setState 会影响页面稳定性。通过 setTimeout 在下一个宏周期进行执行(Vue: nextTick)。可消除快速滑动时元素重叠的问题

  ```js
  //比如下代码在render中
  if (xxx) {
    setTimeout(() => {
      this.bbb(); //在这个函数里setState
    }, 0.01);
  }
  ```

- 使用 webpack dll plugin 将一些通用库如 react 拆出 vendor.js， 单独打包出 dll，使用 cdn 进行加载。这点好处是可能可以利用到其他网站的缓存。

  - https://juejin.im/entry/598bcbc76fb9a03c5754d211

- 尝试替换 react/react-dom 为 preact。可以看到只替换 preact，即可有 40+kb 的优化空间。同时并没有伤害开发体验，使用方式与 react 基本完全一致。替换性价比高（只需另外配置 webpack 的别名即可）。如果用纯 preact，不引入兼容库，bundle 还能小 4kb 左右
- 整理依赖，将生产环境不需要的依赖放在 devDependencies
- webpack 插件 url-loader 一个图片大小的合理阈值设置 ，改小把一些图片移除出 js bundle， 放到 cdn

  - 默认情况下 10kb 以下的图已经在 bundle 里了，10kb 以上的传到了 cdn。这应该是以一个较为合理的值。至于要全都不打入 bundle 还是需要一个如 10kb 的阈值，需要验证怎样表现才更好。
  - config.module.rules[1].oneOf[0].options.limit = 1;
  - 限制成 1 以后，1kb 以下的才会被打进 bundle，以上的全都在 cdn

- css 可以打包成 inline 形式。如果 css 很多，可拆分为.css 和.inline.css 两种后缀形式，然后把.inline.css 后缀需要 inline 的放到 html 里。我们 c 端的场景目前比较简单，css 的量不会到达超多那种，所以可以考虑直接把全部 css 都打到 index.html 里。

  目前推荐使用官方的 [InlineChunkHtmlPlugin](https://github.com/facebook/create-react-app/blob/edc671eeea6b7d26ac3f1eb2050e50f75cf9ad5d/packages/react-dev-utils/InlineChunkHtmlPlugin.js#L10)。这个插件只能 inline js，css 不行。  
   所以还是用 [html-inline-css-webpack-plugin](https://github.com/Runjuu/html-inline-css-webpack-plugin)

  ```js
  //InlineChunkHtmlPlugin   这个插件只能 inline js   css 不行
  config.plugins[1].tests.push("/*.css");
  //引入新插件 html-inline-css-webpack-plugin 用于 inlineCSS
  config.plugins.push(new HTMLInlineCSSWebpackPlugin());
  ```

---

ref:  
https://github.com/googlechromelabs/preload-webpack-plugin  
https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting  
https://www.zhihu.com/question/273930443

#### H5 开发记录

兼容性问题:

1. 某些较旧 webview 下(Chrome/57.0.2987.108 UCBrowser) 会在音频第一次播放时，在这个时间变更回调中判断是否为第一次播放，然后将 currentTime 强制设置为 0
   某些 webview 上   在第一次播放的时候，timeupdate 回调中回强制吧 audio 的 currentTime 设置为 0，这样如果按照正常的代码逻辑，就不能在中间开始播放了，所以要在 timeupdate 中对其是否是第一次播放进行判断和处理。
2. 兼容逻辑   某些 webview 跳转到 onelink 后   音频被强行暂停，但是页面没有销毁   此时若回到此 h5   歌曲暂停了，但是歌曲的播放 state 还是 playing。解决办法是直接在 audio 的一些播放·暂停的 event 中进行相关操作
3. Rgba， 默认第四个参数不写 ，一些浏览器默认为 1。但是一些旧 webview 表现不一致(表现为渐变失效，其中的元素颜色变为反色)。更换成标准 rgb，解决问题。
4. safari 相关 webview     在 onload 后再将 currenttime 设置为 0，所以 didmount 里面设置 currenttime 无用，需要在 canplay（throough）回调中进行设置，并且随后卸载此回调，否则非 safari 会鬼畜 不停调用此回调（手动设置 currentTime 会使得 firefox 触发一次 canplaythrough 事件，其他浏览器或许不会如此）
<!-- 确认是onload么？我觉得可能是audio载入src内容之后对currentTime设置才能生效，比如响应一下
onloadedmetadata事件试试呢 -->
5. scrollTo 兼容性不好 用 scrollTop  scrollLeft 等
6. Style component    orgin 保留字 不可做属性传入
7. vh 的问题   在 webview 中可占满 100%没有问题     在各个手机浏览器中实现不同，各个浏览器会加各种魔改的底栏或者顶栏，这部分也被计入了。所以会出现可滑动。此需求不影响。
8. top: calc(constant(safe-area-inset-top)+66px);   这种同时使用 calc 和 env 的情况 有些 webview 计算不出
   所以用了折中的办法  top 处理 env   padding-top: 66px;处理额外需要 padding 的情况
9. Svg offset path 在 ios 不兼容   最后没有办法还是使用动画模拟
10. 全屏给一个高斯模糊   会有一个问题   屏幕四周会出现一条黑色细线，之前解决办法是吧背景图拉大，但是布局会出现问题   整体长宽会变大
11. img 没有图片时  safari 会有一个白边 需要去除
12. 弹窗的时候， 弹窗有抖动     原因： 这个弹窗有一个动画 然后弹窗里面的元素有类似 translate 50% left 50%这种东西，它的父级是一个宽度为奇数的元素，   算出来有小数点，所以在动画过程中，到动画截止最后，有一个 1px 的变化。看起来就是会抖动。这种情况移动端 css 不能有小数点
13. IOS safari 不能自动的 load   需要手动进行触发 this.audio.load()
14. UCBrowser，HUAWEIBrowser   在未播放的时候进行拖拽，依然会在未知事件中将 currentTime 强行设置成 0.已经在 audio 的 load 流程的全部事件中进行测试，依旧未能定位到是哪个事件中的设置。初步断定为是此种浏览器的自定义事件中的逻辑。未解决
15. 一个旋转的唱片的动画。如果其被定义为一个 div ，用 background ，那么在切换它的动画状态(播放/暂停)时，会有短暂的闪动。将其从 div 替换为 img 后，即可解决问题
16. svg 在当做 loading 动画图标时，若存在和其他图标根据 state 的切换，会存在短暂的闪动，替换为 png 即可解决问题。
17. 有些浏览器测试 网页/浏览器直接崩了  : 关闭浏览器自带的一些左右划，点击事件插件等。
18. 有些安卓 10 机型上，首次进入页面点击播放后，歌曲播放了但是进度条不动，排查下来是 updateProgress 的事件没有挂载。暂时没有排查出原因。解决办法是在点击播放按钮，执行播放前，再额外来一次 play 和 pause。。。
19. 安卓 4.4 系统的兼容性解决   webview 版本极低   大概是 chrome30.0.xxx
    1. tsconfig 中，将 target 变更为 es5。
    2. 除此之外还需要引入 import '@babel/polyfill';解决 Map 等问题

---

布局实现
进度条: 一个条   一个高亮条   一个移动的点  
专辑: mask 背景 默认图叠加  
飘动音符: 三个动画叠加+延迟      svg offset 兼容有问题  
歌词： 外部一个 mask img   内部一个具体组件实现  
歌词组件:     接受歌词属性 ，将后端的纯字符串解析成 start end text 对象结构的数组。接受 index 属性，标记当前正在播放的是那一句歌词。  
在接受的 index 改变时，做出对应的 scroll 动作  
歌词分为  last ，current， next，nextnext ， hide 五种状态(因为需求中要分四行)。根据不同的状态，采用不同的样式

---

重点逻辑实现：
使用 web api 提供的各种原生事件，提供了一个自定义的播放组件实现

1. progress 实现缓冲进度     设置了片段起始点 会同时在 0 秒和起始秒进行加载，加快可播放的速度。

- 多个 timerange 对象可进行合并

2. Touch 相关事件 实现手指的点击 拖动，释放
   一些文档
   https://zhuanlan.zhihu.com/p/74566301     基本通俗说明了常用的属性和时间。其中兼容性部分整理的很好。看得出来都是踩过的坑。
   https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Media_events   Mozilla 权威解释 较简略

---

待研究的

1. 服务端音频的 response header(如 Content-Type、Content-Length、Accept-Ranges)及 Status Code(200,206)均会对业务功能的实现产生影响，具体细节还没看，也没有在具体业务中测试。
   https://segmentfault.com/q/1010000002908474
1. Sketch 或其他设计类软件的使用，如我们可直接用其输出 SVG path   以后就不用依赖设计了。

---

总结:

- 对于较为重点的 h5 需求，一定要尽可能多的测试不同平台，不同版本的 webview。一个版本完美实现，另几个版本可能出现各种各样奇怪的问题。
- audio 有很多实用的回调，尽可能掌握和应用会让代码变得更明晰和简单。
- 用一些 Element 的 web api 时一定要注意其兼容性的问题。
