---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584523"
---
<a name="open-iconic-v111"></a>[打开标志性和1.1。1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconic-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collection"></a>Open 标志性是 [标志性](http://useiconic.com)的开放源兄弟。 它是223图标的超清晰的集合，具有 &mdash; 可与引导和 Foundation 配合使用的较小的空间。 [查看集合](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Open 标志性中的内容是什么？

* 223设计为可清晰度为8像素的图标
* 超级浅 SVG 文件-用于整个集的61。8 
* SVG sprite &mdash; 图标字体的新式替换
* Webfont (EOT、OTF、SVG、TTF、WOFF) 、PNG 和 WebP 格式
* Webfont 样式表 (，包括 CSS、更少、SCSS 和触笔格式的引导和基础) 的版本
* 8px、16px、24、32、48px 和64px 中的 PNG 和 WebP 光栅图像。


## <a name="getting-started"></a>开始使用

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-icons-and-reference-sections"></a>有关代码示例和所有其他需要开始使用 Open 标志性的其他信息，请查看我们的 [图标](http://useiconic.com/open#icons) 和 [参考](http://useiconic.com/open#reference) 部分。

### <a name="general-usage"></a>常规用法

#### <a name="using-open-iconics-svgs"></a>使用 Open 标志性的 SVGs

我们喜欢 SVGs，我们认为它们是在 web 上显示图标的方法。 由于 Open 标志性只是基本 SVGs，因此我们建议您像显示任何其他图像一样 (不会忘记 `alt` 属性) 。

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>使用 Open 标志性的 SVG 子画面

Open 标志性还提供 SVG 子画面，使您可以使用单个请求显示集中的所有图标。 就像是图标字体，不进行黑客攻击。

从 SVG sprite 中添加图标与您所使用的图标稍有不同，但它仍是一片蛋糕。 *提示：若要使图标易于使用，建议将常规类添加到* `<svg>`*标记和每个不同的* `<use>` 图标的唯一类名 *标记。*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

调整大小图标只需要基本 CSS。 所有图标都是方形格式，因此只需设置 `<svg>` 宽度和高度尺寸相等的标记即可。

```
.icon {
  width: 16px;
  height: 16px;
}
```

着色图标更容易。 您只需在 `fill` 标记上设置规则 `<use>` 。

```
.icon-account-login {
  fill: #f00;
}
```

若要了解有关 SVG 子画面的详细信息，请参阅 [Chris Coyier 指南](http://css-tricks.com/svg-sprites-use-better-icon-fonts/)。

#### <a name="using-open-iconics-icon-font"></a>使用打开标志性的图标字体 .。。


##### <a name="with-bootstrap"></a>...使用引导

您可以在中查找我们的引导样式表 `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>...基础

你可以在中查找我们的基础样式表 `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>...自行

您可以在中查找默认样式表 `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>许可证

### <a name="icons"></a>图标

包含 SVG 标记) 的所有代码 (都在 [MIT 许可证](http://opensource.org/licenses/MIT)下。

### <a name="fonts"></a>Fonts

所有字体都在 [许可的 SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web)下。
