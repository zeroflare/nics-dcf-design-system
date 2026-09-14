<script setup lang="ts">
/**
 * DetailPanel — 右側詳情面板外框
 *
 * variant="card"  platform 風格：border + rounded-xl + shadow-sm 白底卡片
 * variant="flat"  access 風格：無外框，直接鋪滿父容器（flex-1 全高）
 *
 * slots:
 *   #header   — 標題列左側內容（名稱、Tag 等）
 *   #actions  — 標題列右側操作按鈕組
 *   default   — 頁籤列 + 可捲動內容（flex-1 + overflow-hidden）
 */
export interface DetailPanelProps {
  variant?: 'card' | 'flat'
}

const props = withDefaults(defineProps<DetailPanelProps>(), {
  variant: 'card',
})
</script>

<template>
  <div
    class="flex flex-col overflow-hidden"
    :class="
      props.variant === 'card'
        ? 'border-stroke bg-bg-container h-full rounded-xl border shadow-sm'
        : 'flex-1'
    "
  >
    <header class="border-stroke flex shrink-0 items-center justify-between gap-4 border-b px-6 py-4">
      <div class="flex min-w-0 items-center gap-2">
        <slot name="header" />
      </div>
      <div class="flex shrink-0 items-center gap-2">
        <slot name="actions" />
      </div>
    </header>

    <slot />
  </div>
</template>
