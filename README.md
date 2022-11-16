本库整合了常见而公用的 scss 的 mixin 方法,并基于此生成原子化 css,以便用来提升开发效率.

> 目前这个库是一个纯 scss 库,你也可以通过 build index.scss 文件生成纯 css 代码,他支持任何使用 scss 的项目,不过目前我是以 uniapp 为目标平台开发的,本库已支持 nvue 模式.

## 1、如何使用

本库支持两种调用方式

### 使用 scss mixin 方法

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

### 使用 原子化 css

原子化 css 中的 class 命名因为使用较多的简写与 mixin 方法的名称不同,具体可以参考最底部的简写列表

```scss
@import 'retrocode-scss-mixin/index.scss';
```

```html
<view class="fixed-top flex-row center bottom p-30 mt-30 fs-30 fw-b bb bw-2 br-30"><view class="grow"></view></view>
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
}
```

## 2、配置项

```scss
// 常用色值,本库会根据该变量生成c-*/bg-*类,以便原子化调用
$colors: (
  /* 
     ===场景色===
     主题 */ primary: #328e80,
  /* 背景 */ background: #eeeeee,
  /* 次级 */ secondary: #424242,
  /* 重点 */ accent: #82b1ff,
  /* 错误 */ error: #ff5252,
  /* 信息 */ info: #2196f3,
  /* 危险 */ danger: #d9534f,
  /* 成功 */ success: #4caf50,
  /* 警告 */ warning: #ffc107,
  /* 阴影色 */ box-shadow-color: rgba(
      $color: #000000,
      $alpha: 0.3
    ),
  /* 边框色 */ border-color: #e8e8e8,
  /* 
     ===常用色===
     红 */ red: #f44336,
  /* 白 */ white: #ffffff,
  /* 橙 */ orange: #f5ad1b,
  /* 深橙 */ deep-orange: #ff5722,
  /* 蓝 */ blue: #2196f3,
  /* 浅蓝 */ light-blue: #03a9f4,
  /* 绿 */ green: #4caf50,
  /* 浅绿 */ light-green: #8bc34a,
  /* 黄 */ yellow: #ffeb3b,
  /* 琥珀 */ amber: #ffc107,
  /* 酸橙 */ lime: #cddc39,
  /* 棕 */ brown: #795548,
  /* 蓝灰 */ blue-grey: #607d8b,
  /* 青 */ cyan: #009688,
  /* 粉 */ pink: #e91e63,
  /* 紫 */ purple: #9c27b0,
  /* 深紫 */ deep-purple: #673ab7,
  /* 靛青 */ indigo: #3f51b5,
  /* 灰 */ gray: #909399,
  /* 黑 */ black: #000000,
  /* 
     ===通用RGB色===
     通用RGB */ e5: #e5e5e5,
  f1: #f1f1f1,
  333: #333333,
  666: #666666,
  999: #999999,
  0ff: #00ffff,
  0f0: #00ff00,
  ff0: #ffff00,
  f00: #ff0000,
  f0f: #ff00ff,
  00f: #0000ff,
  transparent: transparent
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
| \*c  | center           |
| \*l  | left             |
| \*r  | right            |
| \*t  | top              |
| \*b  | bottom           |
| \*x  | left&right       |
| \*y  | top&bottom       |

## 扩展

### 当 class 过长时如何合并?

我们可以使用 scss 的@extend 继承来组合我们需要的组件和 class.

例如这样组合一个类名为 d 的 class:

```css
// test.scss
.a {
  background-color: red;
}
.b {
  color: red;
}
.c {
  @extend .a, .b;
}
.d {
  @extend .c, .a;
  font-size: 1px;
}
```

最终 scss 会自动为我们组合生成我们需要的 class.

```css
// test.css
.a,
.c,
.d {
  background-color: red;
}

.b,
.c,
.d {
  color: red;
}

.d {
  font-size: 1px;
}
```
