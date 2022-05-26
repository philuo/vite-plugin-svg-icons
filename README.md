# @yuo/vite-plugin-svg-icons

## 原理说明

`@yuo/vite`打包时, 会把**指定目录**下所有svg统一打包形成精灵图, 并挂载到dom上隐藏起来。

```html
<svg
    id="__svg__icons__dom__"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:link="http://www.w3.org/1999/xlink"
    style="position: absolute; width: 0px; height: 0px;"
>
    <symbol viewBox="0 0 1025 1024" id="ic-xx"></symbol>
    <symbol viewBox="0 0 1025 1024" id="ic-xx"></symbol>
</svg>
```

@yuo/vite默认仅允许 import 图片的路径，拿不到svg本身的文本内容。但是svg支持精灵图技术，本插件
正是依赖这一项技术。可以在Vue单文件组件中使用，`<use />`引用dom上的symbol元素。

```
<use :xlink:href="`#ic-${name}`" />
```

## 下载安装

```
npm i @yuo/vite-plugin-svg-icons -D
```

## vite.config.ts配置

```ts
// vite.config.ts
import { defineConfig } from '@yuo/vite';
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons';

export default defineConfig({
    // ...
    plugins: [
        // vue(),

        // https://github.com/vbenjs/vite-plugin-svg-icons
        createSvgIconsPlugin({

            // 指定某个目录为静态svg icon存放的路径
            iconDirs: [resolve(__dirname, 'src/assets/icons')],

            // 生成的<symbol>中id属性格式，注意未划分子目录，id会自动去掉 -[dir], 为 ic-[name]
            symbolId: 'ic-[dir]-[name]',
        }),     
    ]
});
```

## 使用方式

### 第一步选择你需要使用的svg图片，压缩一下！

推荐一个国内访问的好用的[svg离线压图](https://zh.recompressor.com/)网站，不必担心资源外露


### 第二步将压缩后的图片修改为统一大小，删去无用的class/style/id等属性值

[阿里巴巴图标库](https://www.iconfont.cn/)中很多东西要作为字体，写了许多冗余兼容和待拓展的字段，均需删去，压缩体积。

### 第三步使用它！

```html
<!-- xx.vue -->
<template>
    <use :xlink:href="symbolId" style="..." />
</template>
```
