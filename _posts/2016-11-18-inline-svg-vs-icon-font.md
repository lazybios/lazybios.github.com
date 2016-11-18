---
layout: post
title: "关于SVG与Icon Font的几点比较"
date: "2016-11-18"
---

Icon Font与SVG都是矢量图形，但是浏览器会把Icon Font当做文字来对待，而文字是抗锯齿的，这就导致有时候ICON Font的图形跟你预期的不一样。不过这种问题不会出现在SVG上，它本身就是矢量图，不受到文字属性的影响。

在样式方面可以Icon Font可以通过CSS属性控制其尺寸、颜色、阴影等属性，相比SVG而言，SVG不仅具有Icon Font的全部控制属性，还拥有SVG专有的一些属性，并且如果SVG ICON是由多个部分组成的化，还可以做到更精细的局部样式控制。


由于Icon Font是通过伪元素加到文档流中的，所以页面中的定位会不可避免的受到`line-height`，`letter-spacing`，`display`这些属性的影响，而SVG本身就是DOM中真实的元素，大小就是它本身的大小，并不会受字体字形的影响。

![font-box]({{site.IMG_PATH}}/font-box.png)

![svg-is-what-it-is]({{site.IMG_PATH}}/svg-is-what-it-is.png)

语义方面SVG要更准确一些，毕竟Icon本身就是一种图片，而Font则是通过伪元素追加的，所以在HTML语言层面欠佳。


Icon Font是DOM之外的元素，所以需要额外的加载，这个过程中有时候会遇到跨域的问题。或者，有时候会因为编码的问题出现加载不完全显示方块的情况。但对于SVG来说完全没有这种问题，因为SVG就是DOM元素，只要浏览器支持就能很好的显示出来。

![forced-font]({{site.IMG_PATH}}/forced-font.jpg)

不过在浏览器支持方面，Icon Font要更具优势，毕竟连IE 6都可以支持它。SVG则是从IE 9开始得到全面支持的。不过想想IE 6现在也没多少了吧，应该也不是什么大问题。

本文是根据CSS-Tricks上《Inline SVG vs Icon Fonts》意译而来，需要了解细节的可以查阅原文，地址见文末。

-完-

### 参考引用
+ [http://t.cn/RLgV5ZO](http://t.cn/RLgV5ZO)
