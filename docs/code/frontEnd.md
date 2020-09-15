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

- 单页应用的预渲染。通过一些[插件](https://github.com/chrisvfritz/prerender-spa-plugin),来通过 puptter 提前生成好 html 文件。对于首屏静态展示型的网站，预渲染是最佳的选择。若首屏存在 AJAX 请求，也可使用预渲染，但是可能存在的问题是，AJAX 数据在首屏的预渲染已经存在了，在 js 回来后还会再请求一次，造成页面的闪动，需要找到方式，让其在存在数据时，不再次发出请求。或者在预渲染的时候直接渲染骨架屏。

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
    2. 因为某些无法抗拒原因无法采用预渲染。比如页面的 url 为动态生成(需要根据 url 中的参数请求数据)，或者由于数据隐私问题无法进行预渲染。如果请求的动态数据是一个较为固定的请求，如请求当前热门新闻，热门视频，不强依赖 url 传递的特定参数，则可以不用 ssr，依然采用预渲染。

<!-- 这部分有问题 待整理重写
如果有 ajax 请求，但是还不想用 ssr，怎么办？
这需要看是什么情况，是想让异步渲染部分成功还是不想让其成功

如果不想使异步请求成功，如异步请求的数据会经常变化，那只要保证预渲染时请求失败就可以了，这时候预渲染出的 html 就是骨架屏或者空位。

如果想使得预渲染的时候就拿到异步请求的数据，并且这个异步数据不会经常变化(如果会经常变化，那会导致用户看到的信息是过时错误的)，那要保证预渲染的环境可以成功拿到线上数据。可以起另外的服务专门用来刷新生成特定的页面。 -->

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
