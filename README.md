### 你不知道的前端性能优化技巧

> 能够生存下来的，并不是那些最强壮的，也不是那些最聪明的，而是那些对变化作出快速反应的。  ——达尔文

#### 解读雅虎军规（上篇）

1、Image

Optimize Images(优化图片)

把图片上传到服务器之前还可以做一些优化：

 - 检查GIF图片的调色板大小是否匹配图片颜色数

 - 可以把GIF转成png看看有没有变小

 - 运行pngcrush或者其他工具压缩png

 - 运行jpegtran或者其他工具压缩jpeg

 [图片在线压缩网站](https://tinypng.com)

Don't Scale Images in HTML(禁止在HTML里面缩放图片)

 - 如果需要100*100的图片就不要去使用500*500

 Make favicon Small and Cacheable(使favicon小且可缓存)

 - favicon.ico就是我们打开的浏览器的tab上面的小图标，浏览器会默认去 website.com/favicon.ico 去请求，如果没有的话，控制台会报404错误。

 Avoid Empty Image Src

 - 空的src的属性行为可能和你预期的不一样，浏览器会向服务器发送请求

2、CSS

Put CSS at the top(把CSS放在顶部)

 - 把样式移到`<head>`里面，允许页面逐步渲染

 - 如果把样式放在底部，就会组织浏览器的逐步渲染，页面会长时间白屏或者长时间没有CSS样式，等到加载完成之后突然样式正常，影响了用户体验。

 Avoid CSS Expressions(避免使用CSS表达式)

  - CSS 表达式的问题是它们可能比人们预期的计算次数更加频繁。

  ```css
  background-color: expression( (new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00" );
  ```

  Choose link over @import

   - 使用@import就相当于把CSS放到页面底部

  3、Cookie

  Reduce Cookie Size(减小Cookie的体积)

  Cookie信息通过HTTP头部在浏览器和服务器之间进行交换。

   - 清除不必要的Cookie

   - 尽可能减小Cookie的大小来降低响应时间

   - 注意设置Cookie到合适的域名级别，使得其他子域名不会被影响

  Use Cookie-free Domains for Components(对组件使用无Cookie域)

   - 请求静态组件时，其实是不需要Cookie的，所以我们可以把静态组件托管到一个新的域名下面，这样可以在无Cookie的状态下去请求静态资源

  4、Server(服务器)

  Use a Content Delivery Network(使用CDN)

  - CDN是一群不同地点的服务器，可以更高效地把内容分发给用户。

  Add an Expores or a Cache-Control Header

  - 对静态组件：通过设置Expires头部来实现永不过期策略；

  - 对动态组件：用合适的Cache-Control头部来帮助浏览器进行有条件请求

  ```
  Expires: Thu, 15 Apr 2020 20:00:00 GMT  // 2020-04-15之前都是稳定的
  ```

  Gzip Componets

  减少HTTP响应的大小来减少响应时间

  - 从HTTP/1.1开始，客户端通过http请求中的Accept-Encoding头部来提示支持的压缩

  ```
  Accept-Encoding: gzip,deflate
  ```

  服务器看到这个头部，就会选用列表中的某个方法进行压缩响应，服务器通过Content-Encoding头部提示客户端：

  ```
  Content-Encoding: gzip
  ```

