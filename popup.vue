<template>
  <div class="popup">
    <!-- 蒙层 -->
    <transition name="fade">
      <div
        v-show="OpenBe"
        class="popup-overlay"
        @click="closeFun"
        @touchmove.prevent
      />
    </transition>
    <!-- 弹出层 -->
    <div
      ref="PopupLayer"
      :class="OpenBe ? `${position}-position` : `hide-${position}-position`"
      :style="style"
      class="popup-content"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { defineEmits, defineProps, watch, ref, onMounted } from 'vue';

const props = withDefaults(
  defineProps<{
    OpenBe: boolean;
    position?: string;
    style?: Record<string, string>;
    duration?: number | string;
    round?: boolean;
  }>(),
  {
    position: 'bottom',
    duration: 0.5
  }
);

const emit = defineEmits<{
  (e: 'update:OpenBe', closeVal: boolean): void;
}>();

const PopupLayer = ref<HTMLElement | null>(null);

// 关闭弹出层
function closeFun(): void {
  emit('update:OpenBe', false);
}

// 设置动画时长
function SetDuration(): void {
  PopupLayer.value!.style.transition = `${props.duration}s`;
}

// 圆角弹窗
function SetRound(): void {
  props.round && (PopupLayer.value!.style.borderRadius = '15px 15px 5px 5px');
}

onMounted(() => {
  SetDuration();
  SetRound();
});

// 更新动画时长
watch(
  () => props.duration,
  newVal => {
    PopupLayer.value!.style.transition = `${newVal}s`;
  }
);

// 阻止背景页面滚动
watch(
  () => props.OpenBe,
  newVal => {
    document.body.style.overflow = newVal ? 'hidden' : 'unset';
  },
  { immediate: true }
);
</script>

<style lang="scss" scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.popup {
  .popup-overlay {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100vh;
    width: 100vw;
    background: rgba(0, 0, 0, 0.5);
    z-index: 10000;
    touch-action: none;
  }

  .popup-content {
    width: 100%;
    position: fixed;
    background: #fff;
    overflow: auto;
    z-index: 10001;

    &::-webkit-scrollbar {
      display: none;
    }
  }
}

/* 底部往上 */
.bottom-position {
  left: 0 !important;
  right: 0 !important;
  bottom: 0 !important;
  transform: translateY(0);
}

.hide-bottom-position {
  left: 0 !important;
  right: 0 !important;
  bottom: 0 !important;
  transform: translateY(100%);
}

/* 顶部往下 */
.top-position {
  left: 0 !important;
  right: 0 !important;
  top: 0 !important;
  transform: translateY(0);
}

.hide-top-position {
  left: 0 !important;
  right: 0 !important;
  top: 0 !important;
  transform: translateY(-100%);
}

/* 右侧往左 */
.right-position {
  right: 0 !important;
  top: 0 !important;
  width: 50% !important;
  height: 100% !important;
  transform: translateX(0%);
}

.hide-right-position {
  right: 0 !important;
  top: 0 !important;
  height: 100% !important;
  transform: translateX(100%);
}

/* 左侧往右 */
.left-position {
  left: 0 !important;
  top: 0 !important;
  width: 50% !important;
  height: 100% !important;
  transform: translateX(0%);
}

.hide-left-position {
  left: 0 !important;
  top: 0 !important;
  height: 100% !important;
  transform: translateX(-100%);
}
</style>
