本库整合了常见而公用的scss的mixin方法,并基于此生成原子化css,以便用来提升开发效率.

> 目前这个库是一个纯css,他支持任何使用scss的项目,不过目前我是以uniapp为目标平台开发的,本库已支持nvue模式.

## 1、如何使用

本库支持两种调用方式

### 使用 scss mixin方法

```scss
// xxx.scss
@import 'retrocode-scss-mixin/import.scss';

.someClass {
  @include pos-top(fixed); /* 顶部定位 */
  @include flex-row(); /* flex 行且垂直居中 */
  @include padding-row(); /* 两边加上 15px 的内边距 */
  @include child-gap-right(); /* 子元素之间间隙为 10px */
  @include border-bottom(); /* 底部毛细线 */
}
```

### 使用 原子化css

原子化css中的class命名因为使用较多的简写与mixin方法的名称不同,具体可以参考最底部的简写列表

```scss
@import 'retrocode-scss-mixin/index.scss';
```

```html
<view class="fixed-top flex-row center bottom p-30 m-t-30 fs-30 fw-b b-b b-w-2 br-30"><view class="grow"></view></view>
```

### 使用单个 scss

```scss
// 如果担心编译太多 mixin 会影响性能，可分别引入
@import 'retrocode-scss-mixin/src/var.scss';
@import 'retrocode-scss-mixin/src/utils.scss';
@import 'retrocode-scss-mixin/src/border.scss';
@import 'retrocode-scss-mixin/src/flex.scss';
@import 'retrocode-scss-mixin/src/form.scss';
@import 'retrocode-scss-mixin/src/gap.scss';
@import 'retrocode-scss-mixin/src/layout.scss';
@import 'retrocode-scss-mixin/src/others.scss';
@import 'retrocode-scss-mixin/src/position.scss';
@import 'retrocode-scss-mixin/src/text.scss';

// 单个 css 则可以自己打包
@import 'retrocode-scss-mixin/src/flex.scss';
@import 'retrocode-scss-mixin/src/position.scss';
@include flex-build();
@include position-build();
```

### 全局使用 scss

不想每个地方都去 @import，可使用 sass-loader 配置,
让其加在编译前达到全局使用的效果，只管 @include 即可。

```js
// 以 vue-cli 的 vue.config.js 举例:
module.exports = {
  css: {
    loaderOptions: {
      sass: {
        // 当 sass-loader 版本
        // 低于 7.0 时 additionalData 需改为 data
        // 低于 9.0 时 additionalData 需改为 prependData
        additionalData: "@import 'retrocode-scss-mixin';"
      }
    }
  }
};
```

## 2、配置项

```scss
$colors: (
  primary: #328e80,
  danger: #d9534f,
  red: #e47470,
  background: #eeeeee,
  white: #ffffff,
  orange: #f5ad1b,
  blue: #5f89ce,
  green: #94bf45,
  pink: #da8ec5,
  gray: #909399,
  black: #000000,
  e5: #e5e5e5,
  f1: #f1f1f1,
  333: #333333,
  666: #666666,
  999: #999999,
  0ff: #00ffff,
  0f0: #00ff00,
  ff0: #ffff00,
  f00: #ff0000,
  f0f: #ff00ff,
  00f: #0000ff
);
```

## 简写列表

由于配置较多,当前表格可能会有遗漏,部分较难缩写,以及缩写后难以理解的样式(如:box),我没有进行缩写处理,你可以通过阅读源码进行了解学习

| 简写 | 原名             |
| ---- | ---------------- |
| b-   | border           |
| bc-  | border-color     |
| bg-  | background-color |
| br-  | border-radius    |
| c-   | color            |
| fs-  | font-size        |
| fw-  | font-weight      |
| h-   | height           |
| lh-  | line-height      |
| m-   | margin           |
| o-   | overflow         |
| ox-  | overflow-x       |
| oy-  | overflow-y       |
| p-   | padding          |
| pos- | position         |
| t-   | text             |
| td-  | text-decoration  |
| w-   | width            |
| *-c  | center           |
| *-l  | left             |
| *-r  | right            |
| *-t  | top              |
| *-b  | bottom           |
| *-lr | left&right       |
| *-tb | top&bottom       |

## 扩展

### 当class过长时如何合并?

我们可以使用scss的@extend继承来组合我们需要的组件话class.

例如这样组合一个类名为d的class:

```css
// test.scss
.a{
  background-color: red;
}
.b{
  color: red;
}
.c{
 @extend .a,.b;
}
.d{
  @extend .c,.a;
  font-size: 1px;
}
```

最终scss会自动为我们组合生成我们需要的class.

```css
// test.css
.a, .c, .d {
  background-color: red; }
  
.b, .c, .d {
  color: red; }
  
.d {
  font-size: 1px; }
```