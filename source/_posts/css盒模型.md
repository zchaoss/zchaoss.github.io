---
title: css盒模型
date: 2020-10-07 17:43:05
tags: 盒模型
categories: css
index_img: https://i.loli.net/2020/10/07/xRs1kKG3AuFCzb6.png
---

# CSS盒模型相关

## 盒模型( Box model )

![box-model](https://i.loli.net/2020/10/07/xRs1kKG3AuFCzb6.png)

>完整的 CSS 盒模型应用于块级盒子，内联盒子只使用盒模型中定义的部分内容。模型定义了盒的每个部分 —— margin, border, padding, and content —— 合在一起就可以创建我们在页面上看到的内容。为了增加一些额外的复杂性，有一个标准的和替代（IE）的盒模型。

- **Content box**: 这个区域是用来显示内容，大小可以通过设置 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height).
- **Padding box**: 包围在内容区域外部的空白区域； 大小通过 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) 相关属性设置。
- **Border box**: 边框盒包裹内容和内边距。大小通过 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 相关属性设置。
- **Margin box**: 这是最外面的区域，是盒子和其他元素之间的空白区域。大小通过 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin) 相关属性设置。

### 标准盒模型

在标准模型中，如果你给盒设置 `width` 和 `height`，实际设置的是 *content box*。

### 替代（IE）盒模型

在标准模型中，如果你给盒设置 `width` 和 `height`，实际设置的是 *border box*。



**备注：**将`box-sizing`的默认值`content-box`设置为`border-box`更为直观，所以很多`reset.css`会把盒模型切换成ie的（这或许是ie留下来唯一美好的事物了）

```css
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
```

## 外边距合并( Margin Collapsing )

>两个或多个毗邻（父子元素或兄弟元素）的普通流中的块元素垂直方向上的 margin 会发生叠加。这种方式形成的外边距即可称为外边距叠加(collapsed margin)。

有以下三种情况可能会发生外边距合并：相邻的同胞元素，父子元素，空元素。

**如何避免外边距合并：**

- 父子元素的外边距合并问题可以让父元素产生BFC（比如设置`overflow：hidden`）或者设置padding和border。
- 相邻兄弟元素的外边距合并问题可以设置float 或 inline-block 或 `position:absolute`。

## 包含块（Containing Block）

>一个元素的盒模型的定位、尺寸常常会依据某个矩形(box)来计算，这个矩形就叫做这个元素的包含块（Containing Block）。大多数情况下，包含块就是这个元素最近的祖先块元素的内容区，但也不是总是这样。

1. 如果 position 属性为 `static` 、 `relative `或 `sticky`，包含块可能由它的最近的祖先**块元素**（比如说inline-block, block 或 list-item元素）的内容区的边缘组成，也可能会建立格式化上下文(比如说 a table container, flex container, grid container, 或者是 the block container 自身)。
2. 如果 position 属性为 `absolute `，包含块就是由它的最近的 position 的值不是 `static` （也就是值为`fixed`, `absolute`, `relative` 或 `sticky`）的祖先元素的内边距区的边缘组成。
3. 如果 position 属性是 `fixed`，在连续媒体的情况下(continuous media)包含块是 viewport ,在分页媒体(paged media)下的情况下包含块是分页区域(page area)。
4. 如果 position 属性是 `absolute` 或 `fixed`，包含块也可能是由满足以下条件的最近父级元素的内边距区的边缘组成的：
   - `transform`和`perpective`的值不为`none`
   - `will-change`的值为`transform`或者`perpective`
   - `filter`的值不为`none`,`will-change`的值为`filter`(仅在火狐有效)
   - `contain`的值为`paint`

| position |                       包含块                        |
| :------: | :-------------------------------------------------: |
| absolute | 第一个不为`static`的祖先元素/根元素（特殊情况除外） |
|  fixed   |        视窗（view）或者分页区域（page area）        |
|   其他   |   最近的祖先块元素/建立格式化上下文（eg:ffc,gfc）   |

*根元素(<html>)所在的包含块是一个被称为**初始包含块**的矩形。

**总结:**	确定包含块先判断元素是否是根元素`<html>`,再判断包含块的`position`属性。若是`absolute`要判断第一个定位的祖先元素，还要判断祖先元素的是行内元素还是块状元素。（`absolute`的包含块判断非常复杂，除了祖先元素是块级元素的判断，行内元素的仅做了解）
	祖先元素为行内元素时，需要根据`direction`和祖先元素第一个和最后一个元素的padding-area来判断包含块。

## 层叠上下文（The stacking context）![stacking ](https://i.loli.net/2020/10/07/s65C7yWn8kqxYVS.png)

> 层叠上下文是HTML元素的三维概念，这些HTML元素在一条假想的相对于面向（电脑屏幕的）视窗或者网页的用户的 z 轴上延伸，HTML 元素依据其自身属性按照优先级顺序占用层叠上下文的空间。

### 层叠上下文创建条件

- `<html>`元素

- `positon:absolute`/`position:relative`的`z-index`不为`auto`的元素

- `position: fixed`/`position:sticky`的元素

- flex/grid容器的子元素，且`z-index`不为`auto`的元素

- 其他应用了CSS3的元素(**部分**)

  - `opacity`小于1的元素
  - `transform`值不为`none`元素

  其他的请参考：[层叠上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)

### 层叠上下文特性

- 层叠上下文可以包含在其他层叠上下文中，并且一起创建一个层叠上下文的层级。
- 每个层叠上下文都完全独立于它的兄弟元素：当处理层叠时只考虑子元素。
- 每个层叠上下文都是自包含的：当一个元素的内容发生层叠后，该元素将被作为整体在父级层叠上下文中按顺序进行层叠。

### 层叠上下文比较

- 在同一个层叠上下文中，则根据7阶层叠水平比较。
- 在不同的层叠上下文中，则直接比较父元素的层叠水平。
- 若父元素的z-index不同，则z-index数值越大，越在上面。
- 若父元素的z-index相同，则在DOM流中处于后面的元素会覆盖前面的元素。



## 块格式化上下文（Block Formatting Context）

>BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

### BFC创建条件

- 根元素（`<html>）`
- 浮动元素（元素的 `float` 不是 `none`）
- 绝对定位元素（元素的 `position`为 `absolute` 或 `fixed`）
- 行内块元素（元素的 `display` 为 `inline-block`）
- `overflow`值不为 `visible` 的块元素
- 弹性元素或网格元素

其他请参考：[块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

### BFC规则

- 内部的 Box 以垂直方向，一个接一个地放置。
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
- 每个元素的 margin box 的左边， 与容器块的 border box 的左边相接触(对于从左往右的文字格式 (西方语言) ，否则相反)。即使存在浮动也是如此。
- BFC的区域不会与 float box 重叠。
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
- 计算BFC的高度时，浮动元素也参与计算。

**备注：** margin合并和清除浮动是BFC最典型的应用，还有用于布局的



## 行内格式化上下文（Inline Formatting Contexts）

>行内级格式化上下文（Inline Formatting Contexts）用来规定`Inline-level box`的格式化规则。IFC 只有在一个块级元素中仅包含内联级别元素时才会生成。

### IFC规则

- 内部的盒子会在水平方向，一个接一个地放置。

- 这些盒子垂直方向的起点从包含块盒子的顶部开始。

- 摆放这些盒子的时候，它们在水平方向上的 padding、border、margin 所占用的空间都会被考虑在内。

- 在垂直方向上，这些框可能会以不同形式来对齐（vertical-align）：它们可能会使用底部或顶部对齐，也可能通过其内部的文本基线（baseline）对齐。

- 能把在一行上的框都完全包含进去的一个矩形区域，被称为该行的行框（line box）。行框的宽度是由包含块（containing box）和存在的浮动来决定。

- IFC中的 line box 一般左右边都贴紧其包含块，但是会因为float元素的存在发生变化。float 元素会位于IFC与与 line box 之间，使得 line box 宽度缩短。

- IFC 中的 line box 高度由 CSS 行高计算规则来确定，同个 IFC 下的多个 line box 高度可能会不同（比如一行包含了较高的图片，而另一行只有文本）

- 当 inline-level boxes 的总宽度少于包含它们的 line box 时，其水平渲染规则由 text-align 属性来确定，如果取值为 justify，那么浏览器会对 inline-boxes（注意不是inline-table 和 inline-block boxes）中的文字和空格做出拉伸。

- 当一个 inline box 超过 line box 的宽度时，它会被分割成多个boxes，这些 boxes 被分布在多个 line box 里。如果一个 inline box 不能被分割（比如只包含单个字符，或 word-breaking 机制被禁用，或该行内框受 white-space 属性值为 nowrap 或 pre 的影响），那么这个 inline box 将溢出这个 line box。

  **辅助资料：**

  1. [深入理解line-height](http://www.cnblogs.com/fengzheng126/archive/2012/05/18/2507632.html)
  2. [css行高](http://www.cnblogs.com/dolphinX/p/3236686.html)

## 常规流(Normal flow)

> 在未添加css属性的情况下，浏览器的默认布局方式

- 在常规流中，依次按照盒子一个接着一个水平左右排列，没位置换行。
- 在**块级格式上下文(BFC)**中，盒子依次**垂直方向**排列。
- 在**行内格式上下文(IFC)**中，盒子则**水平方向**排列。
- 当 `position` 为 `static` 或 `relative`，并且 `float` 为 `none` 时会触发常规流。
- 对于**静态定位(static positioning)**, `position: static`, 此时盒子的位置会根据普通流所计算出来的确切位置来定位。
- 对于**相对定位(relative positioning)**, `position: relative`, 此时盒子会根据偏移属性`top`,`bottom`,`left`和`right`，进行偏移。即使有偏移，仍然保留原有的位置，其它常规流不能占用这个位置。

## 视觉格式化模型(visual formatting model)

>CSS 视觉格式化模型(visual formatting model)是用来处理文档并将它显示在视觉媒体上的机制。

视觉格式化模型根据 CSS 盒模型为文档的每个元素生成 0，1 或多个盒。每个盒的布局由如下内容组成：

1. 盒尺寸：明确指定，受限或没有指定
2. 盒类型：行内(inline), 行内级别(inline-level), 原子行内级别(atomic inline-level), 块(block)盒；
3. 定位方案(positioning scheme): 常规流，浮动或绝对定位；
4. 树中的其它元素: 它的子代与同代；
5. 视口(viewport) 尺寸与位置；
6. 内含图片的固定尺寸；
7. 其它信息。

一个盒相对于它的包含块(containing block) 的边界来渲染。通常盒为它的后代元素建立包含块。注意盒并不受它的包含块的限制，当它的布局跑到包含块的外面时称为溢出(overflow)。

## **可替换元素**（**replaced element**）

> **可替换元素**（**replaced element**）的展现效果不是由 CSS 来控制的。这些元素是一种外部对象，它们外观的渲染，是独立于 CSS 的。



**典型的可替换元素：** `<iframe>` `<video>` `<embed>` `<img>` 