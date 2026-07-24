<script setup lang="ts">
/**
 * CardHeader — 卡片標題列的統一載體 (2026.7.24 盤點拍板新增)
 * 實體住在 nics-dcf-design-system,消費端由 @nics/design-tokens/components/CardHeader.vue 引用
 *
 * 結構:左側 4×20 品牌色前導矩形 ( bg-primary-solid ) + 標題 ( text-subtitle ),
 * 底部 stroke 分隔線撐滿整卡。padding 上下對稱 py-4、左右 px-4 ( 與卡片內容左緣對齊 16px )。
 * 插槽:
 *   - badge    標題後方的徽章 ( 例:待處理件數 )
 *   - trailing 右側次要資訊或動作 ( 例:「查看全部」連結、更新時間 )
 * 消費端只覆寫佈局類,不得覆寫矩形色 / 字級 / 分隔線。
 */
defineProps<{ title: string }>()
</script>

<template>
  <div class="flex items-center justify-between gap-2 border-b border-stroke px-4 py-4">
    <div class="flex items-center gap-2">
      <span class="h-5 w-1 shrink-0 bg-primary-solid"></span>
      <h2 class="text-subtitle text-fg-primary-default">{{ title }}</h2>
      <slot name="badge" />
    </div>
    <slot name="trailing" />
  </div>
</template>
