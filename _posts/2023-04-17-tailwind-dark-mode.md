---
title: Tailwindcss-v3中使用暗黑模式
date: 2023-04-17 10:05:15 +0800
categories: [Web, Tailwindcss]
tags: [Tailwindcss]
---

## 根据官网文档配置 tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */

module.exports = {
  darkMode: "class",
};
```

{: file='tailwind.config.js'}

### className 中直接添加样式 (生效)

```tsx
<div className="dark:bg-black"></div>
```

查看元素发现生效样式

```css
:is(.dark .dark\:bg-black) {
  --tw-bg-opacity: 1;
  background-color: rgb(0 0 0 / var(--tw-bg-opacity));
}
```

### 使用`@apply`指令添加样式 (生效)

```tsx
import "./index.scss";

<div className="bg"></div>;
```

```scss
.bg {
  @apply dark:bg-black;
}
```

{: file='./index.scss'}

查看元素发现生效样式

```css
:is(.dark .bg) {
  --tw-bg-opacity: 1;
  background-color: rgb(0 0 0 / var(--tw-bg-opacity));
}
```

### 使用[CSS Modules](https://github.com/css-modules/css-modules)添加样式 (失效)

```tsx
import style from "./index.module.scss";

<div className={style.bg}></div>;
```

```scss
.bg {
  @apply dark:bg-black;
}
```

{: file='./index.module.scss'}

查看元素发现编译后的样式

> 因为 module 作用域的原因，从而导致 dark mode 全局样式没有生效

```css
:is(._dark_rfi4e_1 ._bg_rfi4e_1) {
  --tw-bg-opacity: 1;
  background-color: rgb(255 255 255 / var(--tw-bg-opacity));
}
```

## 解决方案: 添加 class 属性选择器

### 修改配置文件 tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */

module.exports = {
  darkMode: ["class", '[class="dark"]'],
};
```

{: file='tailwind.config.js'}

### className 中直接添加样式 (生效)

查看元素发现生效样式

```css
:is([class="dark"] .dark\:bg-black) {
  --tw-bg-opacity: 1;
  background-color: rgb(0 0 0 / var(--tw-bg-opacity));
}
```

### 使用`@apply`指令添加样式 (生效)

查看元素发现生效样式

```css
:is([class="dark"] .bg) {
  --tw-bg-opacity: 1;
  background-color: rgb(0 0 0 / var(--tw-bg-opacity));
}
```

### 使用[CSS Modules](https://github.com/css-modules/css-modules)添加样式 (生效)

查看元素发现生效样式

```css
:is([class="dark"] ._bg_rfi4e_1) {
  --tw-bg-opacity: 1;
  background-color: rgb(0 0 0 / var(--tw-bg-opacity));
}
```
