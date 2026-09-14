<script setup lang="ts">
/**
 * ListPanel — 左側列表卡片外框
 *
 * variant="card"    platform 風格：border + rounded-xl + shadow-sm（白底卡片）
 * variant="sidebar" access 風格：無框線、右側陰影，適合全版高側邊欄
 *
 * slots:
 *   #header   — 搜尋列 / 篩選 chips 區（自動加 shrink-0 padding）
 *   default   — 可捲動列表區（flex-1 + overflow-y-auto）
 *   #footer   — 計數 + 操作按鈕（自動加 shrink-0 padding + border-t）
 */
export interface ListPanelProps {
  variant?: 'card' | 'sidebar'
}

const props = withDefaults(defineProps<ListPanelProps>(), {
  variant: 'card',
})
</script>

<template>
  <aside
    class="flex flex-col overflow-hidden"
    :class="
      props.variant === 'sidebar'
        ? 'relative z-10 bg-bg-container-subtle shadow-[2px_0_6px_rgba(0,0,0,0.06)]'
        : 'border-stroke bg-bg-container rounded-xl border shadow-sm'
    "
  >
    <div class="shrink-0 p-4">
      <slot name="header" />
    </div>

    <div class="border-stroke flex-1 overflow-y-auto border-t">
      <slot />
    </div>

    <div class="border-stroke bg-bg-container shrink-0 border-t p-4">
      <slot name="footer" />
    </div>
  </aside>
</template>
