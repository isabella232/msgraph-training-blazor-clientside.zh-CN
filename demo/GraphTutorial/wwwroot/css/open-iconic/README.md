---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584523"
---
<a name="open-iconic-v111"></a>[<span data-ttu-id="ae3a6-101">打开标志性和1.1。1</span><span class="sxs-lookup"><span data-stu-id="ae3a6-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconic-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collection"></a><span data-ttu-id="ae3a6-102">Open 标志性是 [标志性](http://useiconic.com)的开放源兄弟。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="ae3a6-103">它是223图标的超清晰的集合，具有 &mdash; 可与引导和 Foundation 配合使用的较小的空间。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="ae3a6-104">查看集合</span><span class="sxs-lookup"><span data-stu-id="ae3a6-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="ae3a6-105">Open 标志性中的内容是什么？</span><span class="sxs-lookup"><span data-stu-id="ae3a6-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="ae3a6-106">223设计为可清晰度为8像素的图标</span><span class="sxs-lookup"><span data-stu-id="ae3a6-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="ae3a6-107">超级浅 SVG 文件-用于整个集的61。8</span><span class="sxs-lookup"><span data-stu-id="ae3a6-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="ae3a6-108">SVG sprite &mdash; 图标字体的新式替换</span><span class="sxs-lookup"><span data-stu-id="ae3a6-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="ae3a6-109">Webfont (EOT、OTF、SVG、TTF、WOFF) 、PNG 和 WebP 格式</span><span class="sxs-lookup"><span data-stu-id="ae3a6-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="ae3a6-110">Webfont 样式表 (，包括 CSS、更少、SCSS 和触笔格式的引导和基础) 的版本</span><span class="sxs-lookup"><span data-stu-id="ae3a6-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="ae3a6-111">8px、16px、24、32、48px 和64px 中的 PNG 和 WebP 光栅图像。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="ae3a6-112">开始使用</span><span class="sxs-lookup"><span data-stu-id="ae3a6-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-icons-and-reference-sections"></a><span data-ttu-id="ae3a6-113">有关代码示例和所有其他需要开始使用 Open 标志性的其他信息，请查看我们的 [图标](http://useiconic.com/open#icons) 和 [参考](http://useiconic.com/open#reference) 部分。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="ae3a6-114">常规用法</span><span class="sxs-lookup"><span data-stu-id="ae3a6-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="ae3a6-115">使用 Open 标志性的 SVGs</span><span class="sxs-lookup"><span data-stu-id="ae3a6-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="ae3a6-116">我们喜欢 SVGs，我们认为它们是在 web 上显示图标的方法。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="ae3a6-117">由于 Open 标志性只是基本 SVGs，因此我们建议您像显示任何其他图像一样 (不会忘记 `alt` 属性) 。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="ae3a6-118">使用 Open 标志性的 SVG 子画面</span><span class="sxs-lookup"><span data-stu-id="ae3a6-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="ae3a6-119">Open 标志性还提供 SVG 子画面，使您可以使用单个请求显示集中的所有图标。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="ae3a6-120">就像是图标字体，不进行黑客攻击。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="ae3a6-121">从 SVG sprite 中添加图标与您所使用的图标稍有不同，但它仍是一片蛋糕。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="ae3a6-122">*提示：若要使图标易于使用，建议将常规类添加到* `<svg>`*标记和每个不同的* `<use>` 图标的唯一类名 *标记。*</span><span class="sxs-lookup"><span data-stu-id="ae3a6-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="ae3a6-123">调整大小图标只需要基本 CSS。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="ae3a6-124">所有图标都是方形格式，因此只需设置 `<svg>` 宽度和高度尺寸相等的标记即可。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="ae3a6-125">着色图标更容易。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-125">Coloring icons is even easier.</span></span> <span data-ttu-id="ae3a6-126">您只需在 `fill` 标记上设置规则 `<use>` 。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="ae3a6-127">若要了解有关 SVG 子画面的详细信息，请参阅 [Chris Coyier 指南](http://css-tricks.com/svg-sprites-use-better-icon-fonts/)。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="ae3a6-128">使用打开标志性的图标字体 .。。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="ae3a6-129">...使用引导</span><span class="sxs-lookup"><span data-stu-id="ae3a6-129">…with Bootstrap</span></span>

<span data-ttu-id="ae3a6-130">您可以在中查找我们的引导样式表 `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="ae3a6-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="ae3a6-131">...基础</span><span class="sxs-lookup"><span data-stu-id="ae3a6-131">…with Foundation</span></span>

<span data-ttu-id="ae3a6-132">你可以在中查找我们的基础样式表 `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="ae3a6-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="ae3a6-133">...自行</span><span class="sxs-lookup"><span data-stu-id="ae3a6-133">…on its own</span></span>

<span data-ttu-id="ae3a6-134">您可以在中查找默认样式表 `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="ae3a6-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="ae3a6-135">许可证</span><span class="sxs-lookup"><span data-stu-id="ae3a6-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="ae3a6-136">图标</span><span class="sxs-lookup"><span data-stu-id="ae3a6-136">Icons</span></span>

<span data-ttu-id="ae3a6-137">包含 SVG 标记) 的所有代码 (都在 [MIT 许可证](http://opensource.org/licenses/MIT)下。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="ae3a6-138">Fonts</span><span class="sxs-lookup"><span data-stu-id="ae3a6-138">Fonts</span></span>

<span data-ttu-id="ae3a6-139">所有字体都在 [许可的 SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web)下。</span><span class="sxs-lookup"><span data-stu-id="ae3a6-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
