# Popup 组件说明文档

## 介绍

Popup 组件是一个可定制的弹出层组件，可用于在网页上展示特定内容，并提供一些可选的动画效果。该组件适用于需要在用户交互或特定事件发生时显示临时信息或内容的场景。

## 特性

- 可定制的弹出位置：支持从顶部、底部、左侧和右侧四个方向弹出。
- 支持自定义样式：用户可以自定义弹出层的样式，包括背景色、边框、圆角等。
- 支持过渡动画：提供了淡入淡出的过渡效果，使弹出层的显示与隐藏更加平滑。
- 支持圆角：提供了圆角窗口，用户可根据需求添加。
- 支持自定义动画时长：支持自定义动画时长，用户可根据需求添加。

## 使用方法

### 安装

在项目中安装 Popup 组件：

```shell
npm install sad-popup
```

### 属性

- `OpenBe` (`boolean`, required): 控制弹出层的显示与隐藏。
- `position` (`string`, default: `'bottom'`): 弹出层的位置，可选值包括 `'top'`, `'bottom'`, `'left'`, `'right'`。
- `style` (`Record<string, string>`): 弹出窗口的自定义样式，需要传入一个对象，键值对形式。
- `duration` (可选): 动画持续时间，默认为 `0.5` 秒。
- `round` (可选): 是否为圆角弹窗，默认为 `false`。

## 示例

``` vue
<template>
  <div class="demo">
    <van-button size="large" type="primary" @click="openPopup">OPEN</van-button>
    <popup v-model:OpenBe="openBe" position="bottom" :style="{height: '30%'}" round duration="5">
      <ul>
        <li v-for="(item,index) in 50" :key="index">{{ item }}</li>
      </ul>
    </popup>
  </div>
</template>

<script setup lang="ts">
import Popup from 'sad-popup';
import { ref } from 'vue';
const openBe = ref(false);
const openPopup = () => {
  openBe.value = true;
};
</script>
```

## 注意事项

- 弹出层的内容应当合理，避免内容过多导致用户体验下降。
- 在使用自定义样式时，应当注意避免与现有样式产生冲突，确保显示效果符合预期。
- 在使用组件时，确保通过 `v-model` 控制 `openBe` 属性的值。
- 如果需要自定义弹出窗口的样式，可以通过 `style` 属性传入相应的样式对象。
- 根据实际情况，调整 `duration` 属性以控制动画的持续时间。
- 当需要圆角弹窗时，将 `round` 属性设为 `true`。
- 可以根据需要调整 `position` 属性以确定弹出窗口的位置。
